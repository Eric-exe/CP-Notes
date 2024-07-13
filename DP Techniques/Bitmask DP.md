Commonly with index and a bitmask for memoization. Only good for small states.

Great for:
* Finding optimal solutions in a graph.
	* Longest path in a graph with cycles. No node can be visited twice in a path.
	* TSP
* Problems where a boolean status needs to be tracked.

Common uses:
* TSP
* Subset DP
* Hamiltonian path/cycles

Implementation for longest path in a graph with cycles with no nodes being visited twice in a path.
```cpp
int go(int idx, int mask) {
    if (dp[idx][mask] != -1) return dp[idx][mask];
    int res = 1;
    for (auto& nxt : mp[idx]) {
        if (mask & (1 << nxt)) continue;
        res = max(res, 1 + go(nxt, mask | (1 << nxt)));
    }
    return dp[idx][mask] = res;
}

// tc: O(n * 2^n)
```

### Problems
[Problem - G - Codeforces](https://codeforces.com/contest/1950/problem/G)