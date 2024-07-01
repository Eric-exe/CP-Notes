#graph-theory 
Use Tarjan's

[Critical Connections in a Network - LeetCode](https://leetcode.com/problems/critical-connections-in-a-network/description/)
[Problem - F - Codeforces](https://codeforces.com/contest/1986/problem/F)

Tutorial: https://www.youtube.com/watch?v=wUgWX0nc4NY

---

CC's on undirected graphs
```cpp
void dfs(int node, 
        int parent,
        map<int, vector<int>>& g, 
        vector<int>& id,
        vector<int>& low, 
        vector<bool>& onStack, 
        stack<int>& st, 
        int& disc) {

    st.push(node);
    onStack[node] = true;
    id[node] = low[node] = disc++;

    for (auto& nxt : g[node]) {
        if (!id[nxt]) dfs(nxt, node, g, id, low, onStack, st, disc);
        if (nxt != parent && onStack[nxt]) low[node] = min(low[node], low[nxt]);
    }

    if (id[node] == low[node]) {
        while (true) {
            int v = st.top(); st.pop();
            onStack[v] = false;
            low[v] = id[node];
            if (v == node) break;
        }
    }
}

vector<int> cc(int n, map<int, vector<int>>& g) {
    vector<int> id(n, 0);
    vector<int> low(n, 0);
    vector<bool> onStack(n, false);
    stack<int> st;
    int disc = 1;
    for (int i = 0; i < n; i++) {
        if (!id[i]) dfs(i, -1, g, id, low, onStack, st, disc);
    }
    return low;
}

```

For SCCs (on directed graphs), just remove checking if nxt is parent.