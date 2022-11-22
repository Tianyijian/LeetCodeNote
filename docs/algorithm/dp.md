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

## 0072. Edit Distance

> :red_circle:

给定两个字符串，返回将字符串1转换为字符串2的最少的操作次数。操作：插入，删除，替换一个字符

### 方法

- 动态规划。`dp[i][j]`表示word1[0~i]转换为word2[0~j]需要的最少的操作次数。T: O(n^2), S: O(n^2)

```cpp
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (int i = 1; i <= m; i++) dp[i][0] = i;
        for (int j = 1; j <= n; j++) dp[0][j] = j;
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1[i - 1] == word2[j - 1]) {
                    dp[i][j] = dp[i - 1][j - 1];
                } else {
                    dp[i][j] = min({dp[i - 1][j - 1], dp[i - 1][j], dp[i][j - 1]}) + 1;
                }
            }
        }
        return dp[m][n];
    }
};
```

## 0279. Perfect Squares

> :orange_circle:

给正整数n，返回和为n的最少的完全平方数个数

### 方法

- 动态规划，完全背包问题。物品为[1, sqrt(n)]，背包容量为n。dp[j]: 和为j的最少的完全平方数个数。T: O(nsqrt(n)), S: O(n)

```cpp
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n + 1, INT_MAX);
        dp[0] = 0;
        for (int i = 1; i <= sqrt(n); i++) {
            for (int j = i * i; j <= n; j++) {
                dp[j] = min(dp[j], dp[j - i * i] + 1);
            }
        }
        return dp[n];
    }
};
```

