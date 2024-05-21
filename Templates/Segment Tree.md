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