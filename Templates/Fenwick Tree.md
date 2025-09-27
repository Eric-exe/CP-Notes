https://www.youtube.com/watch?v=RgITNht_f4Q

```cpp
template<typename T> class Fenwick {
public:
    int n, mod;
    vector<T> bit;
    Fenwick(int n, int mod = 0) : n(n), mod(mod) {
        bit = vector<T>(n + 1, 0);
    }

    void add(int idx, T val) {
        for (++idx; idx <= n; idx += idx & -idx) {
            bit[idx] += val;
            if (mod) bit[idx] %= mod;
        }
    }

    T query(int idx) {
        T res = 0;
        for (++idx; idx > 0; idx -= idx & -idx) {
            res += bit[idx];
            if (mod) res %= mod;
        }
        return res;
    }

    T query(int l, int r) {
        if (l > r) return 0;
        T res = query(r) - query(l - 1);
        if (mod) res = (res + mod) % mod;
        return res;
    }
};
```