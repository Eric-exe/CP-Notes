```cpp
class Solution {

public:

    int lengthOfLIS(vector<int>& nums) {
        // int n = nums.size();
        // vector<int> dp(n, 1);
        // for (int i = n - 1; i >= 0; i--) {
        //     for (int j = i + 1; j < n; j++) {
        //         if (nums[j] > nums[i]) dp[i] = max(dp[i], dp[j] + 1);
        //     }
        // }
        // return *max_element(dp.begin(), dp.end());

        // patience sort
        // https://www.youtube.com/watch?v=0wT67DOzqBg


        vector<int> piles = {INT_MIN};

        for (auto& n : nums) {
            auto it = lower_bound(piles.begin(), piles.end(), n);
            if (it == piles.end()) --it;
            int idx = it - piles.begin();
            if (n <= piles[idx]) piles[idx] = n;
            else piles.push_back(n);

        }
        return piles.size() - 1;
    }
};
```