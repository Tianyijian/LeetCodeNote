# 动态规划

## 0053. Maximum Subarray

> :orange_circle:

给一个整数数组，找到和最大的子数组，返回最大和

### 方法一

* 动态规划
  * dp[i] 到下标i时最大的连续子序和

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        vector<int> dp(nums.size(), 0);
        int res = nums[0];
        dp[0] = nums[0];
        for (int i = 1; i < nums.size(); i++) {
            dp[i] = max(dp[i - 1], 0) + nums[i];
            res = max(res, dp[i]);
        }
        return res;
    }
};
```

### 方法二

* 贪心
  * 局部最优：当前“连续和”为负数时，放弃，从下一个元素开始计算
  * 全局最优：局部最优，记录最大“连续和”，达到全局最优
  * 注意输入全为负数的情况，result初始化为INT32_MIN

```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int curSum = 0;
        int maxRes = INT_MIN;
        for (int i = 0; i < nums.size(); i++) {
            if (curSum < 0) {
                curSum = 0;
            }
            curSum += nums[i];
            maxRes = max(maxRes, curSum);
        }
        return maxRes;
    }
};
```

