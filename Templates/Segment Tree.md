### Point Update Range Query
```cpp
template<typename T> class SegTree {
private:
    ///////////////////////////////////////////////////
    static constexpr T DEFAULT_VALUE = 0;
    static constexpr T BAD = 0;
    T f(T a, T b) {
        return a + b;
    }
    ///////////////////////////////////////////////////
    int n;
    vector<T> tree;
    
    void build(int node, int l, int r, const vector<T>& v) {
        if (l == r) { tree[node] = l < v.size() ? v[l] : DEFAULT_VALUE; return; }
        int m = (l + r) / 2;
        build(node * 2, l, m, v);
        build(node * 2 + 1, m + 1, r, v);
        tree[node] = f(tree[node * 2], tree[node * 2 + 1]);
    }
    
    void update(int node, int l, int r, int idx, T val) {
        if (l == r) { tree[node] = val; return; }
        int m = (l + r) / 2;
        (idx <= m ? update(node * 2, l, m, idx, val) : update(node * 2 + 1, m + 1, r, idx, val));
        tree[node] = f(tree[node * 2], tree[node * 2 + 1]);
    }
    
    T query(int node, int l, int r, int begin, int end) {
        if (begin > r || end < l) return BAD;
        if (begin <= l && r <= end) return tree[node];
        int m = (l + r) / 2;
        return f(query(node * 2, l, m, begin, end), query(node * 2 + 1, m + 1, r, begin, end));
    }

public:
    SegTree(int n) : n(n), tree(4 * n, DEFAULT_VALUE) { build(1, 0, n - 1, {}); }
    SegTree(const vector<T>& v) : SegTree(v.size()) { build(1, 0, n - 1, v); }
    void update(int idx, T val) { update(1, 0, n - 1, idx, val); }
    T query(int l, int r) { return query(1, 0, n - 1, l, r); }
};
```

Usage
```cpp
// assume arr is vector<int>
int n = arr.size();
SegTree<int>* st1 = new SegTree<int>(n);
SegTree<int>* st2 = new SegTree<int>(arr);
```

### Range Update Range Query
```cpp
template<typename T> class SegTree {
private:
    ///////////////////////////////////////////////////
    static constexpr T DEFAULT_VALUE = 1e9;
    static constexpr T BAD = 0;
    T f(T a, T b) {
        return a + b;
    }
    ///////////////////////////////////////////////////
    int n;
    vector<T> tree;
    vector<T> lazyAdd;
    vector<T> lazySet;
    vector<bool> isLazySet;
    
    void build(int node, int l, int r, const vector<T>& v) {
        if (l == r) { tree[node] = l < v.size() ? v[l] : DEFAULT_VALUE; return; }
        int m = (l + r) / 2;
        build(node * 2, l, m, v);
        build(node * 2 + 1, m + 1, r, v);
        tree[node] = f(tree[node * 2], tree[node * 2 + 1]);
    }

    void push(int node, int l, int m, int r) {
        if (isLazySet[node]) {
            lazySet[node * 2] = lazySet[node * 2 + 1] = lazySet[node];
            isLazySet[node * 2] = isLazySet[node * 2 + 1] = true;
            tree[node * 2] = (m - l + 1) * lazySet[node];
            tree[node * 2 + 1] = (r - m) * lazySet[node];
            lazyAdd[node * 2] = lazyAdd[node * 2 + 1] = 0;
            lazySet[node] = 0;
            isLazySet[node] = false;
        }
        else if (lazyAdd[node] != 0) {
            if (!isLazySet[node * 2]) lazyAdd[node * 2] += lazyAdd[node];
            else {
                lazySet[node * 2] += lazyAdd[node];
                lazyAdd[node * 2] = 0;
            }

            if (!isLazySet[node * 2 + 1]) lazyAdd[node * 2 + 1] += lazyAdd[node];
            else {
                lazySet[node * 2 + 1] += lazyAdd[node];
                lazyAdd[node * 2 + 1] = 0;
            }

            tree[node * 2] += (m - l + 1) * lazyAdd[node];
            tree[node * 2 + 1] += (r - m) * lazyAdd[node];
            lazyAdd[node] = 0;
        }
    }
    
    void set(int node, int l, int r, int a, int b, T val) {
        if (a > r || b < l) return;
        if (a <= l && r <= b) {
            tree[node] = (r - l + 1) * val;
            lazyAdd[node] = 0;
            lazySet[node] = val;
            isLazySet[node] = true;
            return;
        }
        int m = (l + r) / 2;
        push(node, l, m, r);
        set(node * 2, l, m, a, b, val);
        set(node * 2 + 1, m + 1, r, a, b, val);
        tree[node] = f(tree[node * 2], tree[node * 2 + 1]);
    }

    void add(int node, int l, int r, int a, int b, T val) {
        if (a > r || b < l) return;
        if (a <= l && r <= b) {
            tree[node] += (r - l + 1) * val;
            if (lazySet[node] == 0) lazyAdd[node] += val;
            else lazySet[node] += val;
            return;
        }
        int m = (l + r) / 2;
        push(node, l, m, r);
        add(node * 2, l, m, a, b, val);
        add(node * 2 + 1, m + 1, r, a, b, val);
        tree[node] = f(tree[node * 2], tree[node * 2 + 1]);
    }
    
    T query(int node, int l, int r, int begin, int end) {
        if (begin > r || end < l) return BAD;
        if (begin <= l && r <= end) return tree[node];
        int m = (l + r) / 2;
        push(node, l, m, r);
        return f(query(node * 2, l, m, begin, end), query(node * 2 + 1, m + 1, r, begin, end));
    }

public:
    SegTree(int n) : n(n), tree(4 * n, DEFAULT_VALUE), lazyAdd(4 * n, 0), lazySet(4 * n, 0), isLazySet(4 * n, false) { build(1, 0, n - 1, {}); }
    SegTree(const vector<T>& v) : SegTree(v.size()) { build(1, 0, n - 1, v); }
    void rangeSet(int l, int r, T val) { set(1, 0, n - 1, l, r, val); }
    void rangeAdd(int l, int r, T val) { add(1, 0, n - 1, l, r, val); }
    T query(int l, int r) { return query(1, 0, n - 1, l, r); }
};
```

### Using Nodes (To Understand the Concept)
```cpp
class SegTree {
public:
    int l, r;
    SegTree* lChild;
    SegTree* rChild;
    int mx = -1e9;

    SegTree(int l, int r) {
        this->l = l;
        this->r = r;
        if (l != r) {
            int m = (l + r) / 2;
            lChild = new SegTree(l, m);
            rChild = new SegTree(m + 1, r);
            mx = max(lChild->mx, rChild->mx);
        }
    }

    void updateIdx(int idx, int val) {
        if (l == r) {
            mx = val;
            return;
        }
        int m = (l + r) / 2;
        if (idx <= m) lChild->updateIdx(idx, val);
        else rChild->updateIdx(idx, val);
        mx = max(lChild->mx, rChild->mx);
    }

    int getMx(int left, int right) {
        if (left > r || right < l) return -1e9;
        if (left <= l && right >= r) return mx;
        return max(lChild->getMx(left, right), rChild->getMx(left, right));
    }
};
```
