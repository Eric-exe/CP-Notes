Notation(s):
- $\pi$ - permutation of $[n]$ 

Cycle: A cycle of $\pi$ is a list $(i, \pi(i), \pi^2(i),...,\pi^{k-1}(i))$ such that $\pi^k(i)=i$  but $\pi^j \neq i$ for $i=1,2,...,k-1$. $k$ is therefore the length of the cycle.

If $b$ is a permutation of $a$, then we can visualize it with a directed graph such that $i \rightarrow \pi(i)$. This directed graph can always be broken down into cycles.

In other words, the cycle decomposition of a permutation $\pi$ is the set of all cycles in $\pi$. 

### A Problem
What is the minimum number of swaps to get from $A$ to $B$?
Calculate the smallest cycles of $\pi$. Then, the answer is
$$
\sum(c_k-1)
$$
where $c_1,c2,...,c_k$ are the cycles of the permutation. This is because a $k$-length cycle can always be created with $k-1$ transpositions. 

### Some Intuition
Regarding minimum swaps to convert a permutation from $a$ to $b$, assuming $a$ and $b$ are strings:
* We can **greedily skip over characters that are already in the same position** (don't swap at all) (because the minimum cycle is 1 and thus, we need no transpositions).
	* This also means that when we swap characters, it is never worth it to swap a character that is already in their place. 
	* As such, the minimum number of swaps to get from $a$ to $b$ can solved like this using a BFS approach:
		* Scan from left to right:
			1. If $a = b$ then we don't need any swaps
			2. Using a linear scan, find the first index $i$ where $a[i] \neq b[i]$ 
			3. Run another linear scan, finding the indices $j$ where $a[j] = b[i]$ and $a[j] \neq b[j]$. This is because we don't want to remove a character that is already in their place (second conditional). After a swap, push these into the queue.

#### Code
```cpp
int n = s1.length();
set<string> seen;
seen.insert(s1);
queue<string> q;
q.push(s1);

int step = 0;
while (!q.empty()) {
	int sz = q.size();
	while (sz--) {
		string currStr = q.front(); q.pop();
		if (currStr == s2) return step;

		int ptr = 0;
		while (currStr[ptr] == s2[ptr]) ptr++;

		for (int i = ptr; i < n; i++) {
			if (currStr[i] == s2[ptr] && currStr[i] != s2[i]) {
				swap(currStr[ptr], currStr[i]);
				if (!seen.count(currStr)) {
					seen.insert(currStr);
					q.push(currStr);
				}
				swap(currStr[ptr], currStr[i]);
			}
		}
	}
	step++;
}
return -1;
```