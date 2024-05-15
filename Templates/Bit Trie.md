Usage: Given an array of integers, after insertion, we can find the max/min bitwise operators in log(n) time if $n$ is the integer given in query.

```cpp
enum BitwiseOperator { AND, OR, XOR };
class BitTrie {
public:
    int BITS = 31;
    BitTrie* child[2];
    int num = -1, cnt = 0;
    BitTrie() { child[0] = child[1] = NULL; };

    void add(int num) {
        BitTrie* node = this;
        node->cnt++;
        for (int i = BITS; i >= 0; i--) {
            bool bit = num & (1 << i);
            if (!node->child[bit]) node->child[bit] = new BitTrie();
            node = node->child[bit];
            node->cnt++;
        }
        node->num = num;
    }

    void remove(int num) {
        BitTrie* node = this;
        node->cnt--;
        for (int i = BITS; i >= 0; i--) {
            bool bit = num & (1 << i);
            node = node->child[bit];
            node->cnt--;
        }
        node->num = -1;
    }

    int min(int num, BitwiseOperator op) {
        BitTrie* node = this;
        int res = INT_MIN;
        for (int i = BITS; i >= 0 && node && node->cnt; i--) {
            bool bit = num & (1 << i);
            bool desired = op == AND ? !bit : bit;
            if (node->child[desired] && node->child[desired]->cnt) node = node->child[desired];
            else node = node->child[!desired];
            if (node && node->num != -1) {
                res = std::min(res, op == AND ? num & node->num :
                                    op == OR  ? num | node->num :
                                                num ^ node->num);
            }
        }
        return res;
    }

    int max(int num, BitwiseOperator op) {
        BitTrie* node = this;
        int res = INT_MIN;
        for (int i = BITS; i >= 0 && node && node->cnt; i--) {
            bool bit = num & (1 << i);
            bool desired = op == AND ? bit : !bit;
            if (node->child[desired] && node->child[desired]->cnt) node = node->child[desired];
            else node = node->child[!desired];
            if (node && node->num != -1) {
                res = std::max(res, op == AND ? num & node->num :
                                    op == OR  ? num | node->num :
                                                num ^ node->num);
            }
        }
        return res;
    }
};
```

---
### Expanded Form (For more clarity)
```cpp
int BITS = 31;

class BitTrie {
public:
    BitTrie* child[2];
    int num = -1;
    BitTrie() { child[0] = child[1] = NULL; };

    void insert(int num) {
        BitTrie* node = this;
        for (int i = BITS; i >= 0; i--) {
            bool bit = num & (1 << i);
            if (!node->child[bit]) node->child[bit] = new BitTrie();
            node = node->child[bit];
        }
        node->num = num;
    }

    void remove(int num) {
        BitTrie* node = this;
        for (int i = BITS; i >= 0; i--) {
            bool bit = num & (1 << i);
            node = node->child[bit];
        }
        node->num = -1;
    }

    int getMaxXor(int num) {
        BitTrie* node = this;
        int res = -1;
        for (int i = BITS; i >= 0 && node; i--) {
            bool bit = num & (1 << i);
            if (node->child[1 - bit]) node = node->child[1 - bit];
            else node = node->child[bit];
            if (node && node->num != -1) res = max(res, num ^ node->num);
        }
        return res;
    }
};
```
---
### XOR Range

#### Intuition
Tries can be used to find the number of values $x$ where $x$ XOR $y \leq limit$. To do this, we can use a trie that represents the bits of the values of $x$. The trie will have the most significant bit near the root of the trie and the least significant bits near the leaves of the tree. We need to be greedy.
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

---
#### Code (Bit Trie with XOR range)
```cpp
enum BitwiseOperator { AND, OR, XOR };
class BitTrie {
public:
    int BITS = 31;
    BitTrie* child[2];
    int num = -1, cnt = 0;
    BitTrie() { child[0] = child[1] = NULL; };

    void add(int num) {
        BitTrie* node = this;
        node->cnt++;
        for (int i = BITS; i >= 0; i--) {
            bool bit = num & (1 << i);
            if (!node->child[bit]) node->child[bit] = new BitTrie();
            node = node->child[bit];
            node->cnt++;
        }
        node->num = num;
    }

    void remove(int num) {
        BitTrie* node = this;
        node->cnt--;
        for (int i = BITS; i >= 0; i--) {
            bool bit = num & (1 << i);
            node = node->child[bit];
            node->cnt--;
        }
        node->num = -1;
    }

    int min(int num, BitwiseOperator op) {
        BitTrie* node = this;
        int res = INT_MIN;
        for (int i = BITS; i >= 0 && node && node->cnt; i--) {
            bool bit = num & (1 << i);
            bool desired = op == AND ? !bit : bit;
            if (node->child[desired] && node->child[desired]->cnt) node = node->child[desired];
            else node = node->child[!desired];
            if (node && node->num != -1) {
                res = std::min(res, op == AND ? num & node->num :
                                    op == OR  ? num | node->num :
                                                num ^ node->num);
            }
        }
        return res;
    }

    int max(int num, BitwiseOperator op) {
        BitTrie* node = this;
        int res = INT_MIN;
        for (int i = BITS; i >= 0 && node && node->cnt; i--) {
            bool bit = num & (1 << i);
            bool desired = op == AND ? bit : !bit;
            if (node->child[desired] && node->child[desired]->cnt) node = node->child[desired];
            else node = node->child[!desired];
            if (node && node->num != -1) {
                res = std::max(res, op == AND ? num & node->num :
                                    op == OR  ? num | node->num :
                                                num ^ node->num);
            }
        }
        return res;
    }

    int countAtMostXor(int num, int k) {
        BitTrie* node = this;
        int res = 0;
        for (int i = BITS; i >= 0 && node && node->cnt; i--) {
            bool bit = num & (1 << i);
            bool desired = k & (1 << i);
            if (desired == 0) node = node->child[bit];
            else {
                res += node->child[bit] ? node->child[bit]->cnt : 0;
                node = node->child[!bit];
            }
        }
        return res + (node ? node->cnt : 0);
    }
};
```