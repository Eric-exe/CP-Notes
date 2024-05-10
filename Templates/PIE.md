```cpp
class Solution {
public:
    vector<int> coins;
    int k;
    long long findKthSmallest(vector<int>& coins, int k) {
        this->coins = coins;
        this->k = k;
        long long res = 0;
        long long l = 0, r = 1e18;

        while (l <= r) {
            long long m = l + (r - l) / 2;
            if (count(m) >= k) {
                res = m;
                r = m - 1;
            }
            else l = m + 1;
        }
        return res;
    }

    long long count(long long num) {
        int n = coins.size();
        long long res = 0;
        for (int mask = 1; mask < (1 << n); mask++) {
            long long lcmVal = 1;
            for (int i = 0; i < n; i++) if (mask & (1 << i)) lcmVal = lcm(lcmVal, coins[i]);
            if (__builtin_popcount(mask) % 2) res += num / lcmVal;
            else res -= num / lcmVal;
        }
        return res;
    }
};
```