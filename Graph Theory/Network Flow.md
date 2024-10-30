### Dinic
```cpp
class Edge {
public:
    Edge* residual;
    ll to, cap;
    Edge(ll v, ll c) : to(v), cap(c) { residual = nullptr; }
    void augment(ll c) { cap -= c; residual->cap += c; }
};
 
class Dinic {
public:
    Dinic(int n) {
        this->n = n;
        this->mp = vector<vector<Edge*>>(n);
        this->layer = vector<int>(n);
    }
 
    ~Dinic() {
        for (auto& v : mp) {
            for (Edge* e : v) delete e;
        }
    }
 
    ll maxFlow(int s, int t) {
        this->s = s;
        this->t = t;
        ll flow = 0;
        while (bfs()) {
            vector<int> next(n, 0);
            while (ll pushed = dfs(s, LLONG_MAX, next)) flow += pushed;
        }
        return flow;
    }
 
    void addEdge(ll u, ll v, ll c) {
        Edge* e1 = new Edge(v, c);
        Edge* e2 = new Edge(u, 0);
        e1->residual = e2;
        e2->residual = e1;
        mp[u].push_back(e1);
        mp[v].push_back(e2);
    }
 
    bool bfs() {
        layer = vector<int>(n, -1);
        queue<int> q;
        q.push(s);
        layer[s] = 0;
        while (!q.empty()) {
            int u = q.front(); q.pop();
            for (Edge* e : mp[u]) {
                if (e->cap && layer[e->to] == -1) {
                    layer[e->to] = layer[u] + 1;
                    q.push(e->to);
                }
            }
        }
        return layer[t] != -1;
    }
 
    ll dfs(int u, ll f, vector<int>& next) {
        if (u == t) return f;
        for (int& i = next[u]; i < mp[u].size(); i++) {
            Edge* e = mp[u][i];
            if (e->cap && layer[e->to] == layer[u] + 1) {
                ll flow = dfs(e->to, min(f, e->cap), next);
                if (flow) {
                    e->augment(flow);
                    return flow;
                }
            }
        }
        return 0;
    }
 
private:
    int s, t, n;
    vector<vector<Edge*>> mp;
    vector<int> layer;
};
```

[[Tutorial, Flows] Project Selection Problem - Codeforces](https://codeforces.com/blog/entry/101354)

