```cpp
class Node {
public:
	// unordered_map<int, Node*> children;
	vector<Node*> children;
	bool end = false;
	Node() {
		children.resize(26, nullptr);
	}
};

void trieInsert(Node* node, string& s) {
	for (auto& c : s) {
		if (node->children[c - 'a'] == nullptr) node->children[c - 'a'] = new Node();
		node = node->children[c - 'a'];
	}
	node->end = true;
}
```
---
### XOR

### Intuition
Tries can be used to find the number of values $x$ where $x$ XOR $y \leq limit$. To do this, we can use a trie that represents the bits of the values of $x$. The trie will have the most significant bit near the root of the trie and the least significant bits near the leaves of the tree.
#### Greedy
To count the number of values, we first have to iterate from the most significant bit to the least. Note that there should be padding of 0s such that the trie can accommodate the biggest values.

First, we compare the most significant bit that we haven't processed:
```
0001010101 (limit)
0101010111 (curr)
^
```
---
**Case 1: Limit's Bit is 0**
If limit's most significant bit is 0, we must take the same bit as our current value. If we take the opposite bit, we go over the limit when XORed.

Example if we take the opposite bit even if we take the same bits greedily afterward:
```
v
0001010101 (limit)
0101010111 (curr)
1101010111 (our value)
---------- (XOR)
1000000000
```
**If we take the opposite bit, it goes over the limit.****

**NOTE**: If you wonder why 0 will always go over if we flip the bit, it is because we are greedily taking the opposite bit whenever possible such that when we finish the trie (if possible), our final bit representation XOR curr will equal limit. As such, if the current bit of limit is 0, if we take an opposite bit, it goes over the limit when XORed.

---
**Case 2: Limit's Bit is 1**
If limit's most significant bit is 1, we can be greedy. We can either take our current's bit or take the opposite. Examining both cases:

**Case 2.1: Take the Same Bit as Current**
If we take the same bit as the most significant bit of current, then the XOR of our value and curr will always be smaller than limit, even if we take the opposite of the bits afterwards:

Example:
```
v
1100101010 (limit)
0101011100 (curr)
0010100011 (our value)
---------- (XOR)
0111111111
```
We can see that if we take the same bit as current, the max XOR value will be smaller than limit. This means that we can take every possible number with the bit representation (in this example) $0xxxxxxxx$. This also means that after taking all possible numbers with this bit representation, we should flip our current bit and be greedy, taking more numbers if possible, which goes on to our next case:

**Case 2.2: Taking the Opposite Bit as Current**
If we take the opposite bit as the most significant bit of current, then the XOR of our value **may** be smaller than limit, depending on the bits we choose afterwards. We should traverse down this path after handling case 2.1.

---
**Case 3: End of Trie**
Assuming we reached the end of the trie (that means there were nodes), we have found the number of values that, when XORed with current, results in limit. As an example:
```
10111001 (limit)
01011101 (current)
11100100 (our value)
-------- (XOR)
10111001 (same as limit)
```
We should add the count to our result.
#### Code
```cpp
int BITS = 16;

class Node {
public:
    vector<Node*> child;
    int cnt = 0;
    Node() { child.resize(2, nullptr); }
};

class Solution {
public:

    int countGood(Node* node, int curr, int limit) {
        int res = 0;
        for (int i = BITS; i >= 0 && node; i--) {
            bool currBit = curr & (1 << i);
            bool limBit = limit & (1 << i);

            if (limBit == 0) {
                node = node->child[currBit];
                continue;
            }

            // limBit is 1, if we take the same bit as current
            // it will always be smaller than limit
            if (node->child[currBit]) res += node->child[currBit]->cnt;
            // be greedy by going to the other bit.
            // this will make the xor of the current bit be 1
            // or the same as limBit
            node = node->child[1 - currBit];
        }

        if (node) res += node->cnt;
        return res;
    }

    void trieInsert(Node* node, int val) {
        for (int i = BITS; i >= 0; i--) {
            bool bit = val & (1 << i);
            if (!node->child[bit]) node->child[bit] = new Node();
            node = node->child[bit];
            node->cnt++;
        }
    }

    int countPairs(vector<int>& nums, int low, int high) {
        Node* head = new Node();
        int res = 0;
        for (auto& num : nums) {
            res += countGood(head, num, high) - countGood(head, num, low - 1);
            trieInsert(head, num);
        }

        return res;
    }
};
```

### Bit Trie
```cpp
int BITS = 31;

class Node {
public:
    Node* child[2];
    int cnt = 0;
    int num = -1;

    Node() { child[0] = child[1] = NULL; }
};

void trieInsert(Node* node, int num, int inc) {
    for (int i = BITS; i >= 0; i--) {
        bool bit = num & (1 << i);
        if (!node->child[bit]) node->child[bit] = new Node();
        node = node->child[bit];
        node->cnt += inc;
    }
    node->num = num;
}

int getMax(Node* node, int num) {
    int res = -1;
    for (int i = BITS; i >= 0; i--) {
        bool bit = num & (1 << i);
        if (node->child[1 - bit] && node->child[1 - bit]->cnt) node = node->child[1 - bit];
        else if (node->child[bit] && node->child[bit]->cnt) node = node->child[bit];
        else break;
        if (node && node->cnt && node->num != -1) res = max(res, num ^ node->num);
    }
    return res;
}
```