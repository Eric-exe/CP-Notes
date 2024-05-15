```cpp
class Solution {

public:

    int lengthOfLIS(vector<int>& nums) {
        // patience sort
        // https://www.youtube.com/watch?v=0wT67DOzqBg

        vector<int> piles = {INT_MIN};
        // to support non-decreasing, replace lower_bound
        // with upper_bound
        for (auto& num : nums) {
            auto it = lower_bound(piles.begin(), piles.end(), num);
            if (it == piles.end()) piles.push_back(num);
            else piles[it - piles.begin()] = num;
        }
        return piles.size() - 1;
    }
};
```