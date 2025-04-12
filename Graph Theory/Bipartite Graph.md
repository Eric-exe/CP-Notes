A graph in graph theory where the vertices can be divided into two disjoint sets, and every edge connects a vertex in one set to a vertex in the other set.

Properties:
* Contains **NO** odd cycles

Bipartite Check
Choose any vertex and include it in one set (can be marked as $0$ or $1$). Adjacent vertices are forced to go to the other set. If an adjacent vertex is in the same set, the graph is not bipartite.

### Maximum Cardinality Matching - Hopcroft Karp
[Problem - 1423B - Codeforces](https://codeforces.com/problemset/problem/1423/B)
[Hopcroft–Karp algorithm - Wikipedia](https://en.wikipedia.org/wiki/Hopcroft%E2%80%93Karp_algorithm)
[Hopcroft–Karp algorithm](https://www.youtube.com/watch?v=lM5eIpF0xjA)
```cpp
class HopcroftKarp {
public:
    int n;
    vector<vector<int>> adj;
    vector<int> pair_u, pair_v, dist;
    queue<int> Q;

    HopcroftKarp(int n) : n(n), adj(n), pair_u(n, -1), pair_v(n, -1), dist(n + 1) {}

    void addEdge(int u, int v) { adj[u].push_back(v); }

    bool bfs() {
        for (int u = 0; u < n; u++) {
            if (pair_u[u] == -1) {
                dist[u] = 0;
                Q.push(u);
            } 
            else dist[u] = INT_MAX;
        }

        bool found = false;
        while (!Q.empty()) {
            int u = Q.front(); Q.pop();
            for (auto& v : adj[u]) {
                int pu = pair_v[v];
                if (pu == -1) found = true;
                else if (dist[pu] == INT_MAX) {
                    dist[pu] = dist[u] + 1;
                    Q.push(pu);
                }
            }
        }

        return found;
    }

    bool dfs(int u) {
        for (auto& v : adj[u]) {
            int pu = pair_v[v];
            if (pu == -1 || (dist[pu] == dist[u] + 1 && dfs(pu))) {
                pair_u[u] = v;
                pair_v[v] = u;
                return true;
            }
        }
        dist[u] = INT_MAX;
        return false;
    }

    int maxMatching() {
        int matching = 0;
        while (bfs()) {
            for (int u = 0; u < n; u++) {
                if (pair_u[u] == -1 && dfs(u)) matching++;
            }
        }
        return matching;
    }

    void clear() {
        for (int i = 0; i < n; i++) {
            pair_u[i] = -1;
            pair_v[i] = -1;
            dist[i] = INT_MAX;
        }
    }
};
```