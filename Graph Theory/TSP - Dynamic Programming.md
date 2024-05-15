[Traveling Salesman Problem | Dynamic Programming | Graph Theory (youtube.com)](https://www.youtube.com/watch?v=cY4HiiFHO1o)

```cpp
class Solution {
public:
    vector<int> nums;
    int dp[14][1 << 15];
    int n;
    vector<int> findPermutation(vector<int>& nums) {
        /*
        cyclic graph h-> TSP
        */
        this->nums = nums;
        memset(dp, -1, sizeof(dp));
        n = nums.size();
        getMnScore(0, 1);

        // reconstruction
        vector<int> res = {0};
        int mask = 1;
        while (res.size() != n - 1) {
            int bestScore = dp[res.back()][mask];
            for (int i = 0; i < n; i++) {
                if (mask & (1 << i)) continue;
                if (abs(res.back() - nums[i]) + dp[i][mask | (1 << i)] == bestScore) {
                    res.push_back(i);
                    mask |= 1 << i;
                    break;
                }
            }
        }
        for (int i = 0; i < n; i++) {
            if (mask & (1 << i)) continue;
            res.push_back(i);
            break;
        }
        return res;
    }

    int getMnScore(int prevNum, int mask) {
        if (mask == (1 << n) - 1) return abs(prevNum - nums[0]);
        if (dp[prevNum][mask] != -1) return dp[prevNum][mask];

        int res = INT_MAX;
        for (int i = 0; i < n; i++) {
            if (mask & (1 << i)) continue;
            res = min(res, getMnScore(i, mask | (1 << i)) + abs(prevNum - nums[i]));
        }

        return dp[prevNum][mask] = res;
    }
};
```