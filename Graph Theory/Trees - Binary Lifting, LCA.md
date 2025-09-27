### Binary Lifting:
```cpp
vector<vector<int>> parent;
void binaryLift(int u, int p, vector<vector<int>>& adj) {
    int n = adj.size();
    parent = vector<vector<int>>(n + 1, vector<int>(30, -1));
    
    queue<pair<int, int>> q;
    q.push({u, p});
    
    while (!q.empty()) {
        auto [u, p] = q.front(); q.pop();
        parent[u][0] = p;
        for (int i = 1; i < 30; i++) {
            if (parent[u][i - 1] == -1) break;
            parent[u][i] = parent[parent[u][i - 1]][i - 1];
        }
        for (auto& nxt : adj[u]) {
            if (nxt == p) continue;
            q.push({nxt, u});
        }
    }
}
```
---
### LCA
```cpp
vector<vector<int>> parent;
void binaryLift(int u, int p, vector<vector<int>>& adj) {
    int n = adj.size();
    parent = vector<vector<int>>(n + 1, vector<int>(30, -1));

    queue<pair<int, int>> q;
    q.push({u, p});

    while (!q.empty()) {
        auto [u, p] = q.front(); q.pop();
        parent[u][0] = p;
        for (int i = 1; i < 30; i++) {
            if (parent[u][i - 1] == -1) break;
            parent[u][i] = parent[parent[u][i - 1]][i - 1];
        }
        for (auto& nxt : adj[u]) {
            if (nxt == p) continue;
            q.push({nxt, u});
        }
    }
}

// assuming st and en already initialized
vector<int> st;
vector<int> en;
void dfs(int u, int p, int& t, vector<vector<int>>& adj) {
    st[u] = t++;
    for (auto& nxt : adj[u]) {
        if (nxt == p) continue;
        dfs(nxt, u, t, adj);
    }
    en[u] = t;
}

int lca(int u, int v) {
    int res = 0;
    for (int i = 29; i >= 0; i--) {
        if (parent[u][i] != -1 && !(st[parent[u][i]] <= st[v] && en[parent[u][i]] >= en[v])) {
            u = parent[u][i];
            res += (1 << i);
        }
    }
    // if u is already an ancestor of v, dont lift
    if (!(st[u] <= st[v] && en[u] >= en[v])) {
        u = parent[u][0];
        res++;
    }
    return res;
}
```

`lca(u, v)` gets the minimum distance `u` needs to traverse up the tree such that it becomes the LCA of `u` and `v`. Thus, the min distance between `u` and `v` is `lca(u, v) + lca(v, u)`