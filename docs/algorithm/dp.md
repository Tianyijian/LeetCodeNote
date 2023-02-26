# 动态规划

## 0070. Climbing Stairs

> :green_circle:

爬楼梯需要n步，每次爬1或2步，共有多少种不同的爬完方式

### 方法一

- 简单动态规划，dp[i]代表爬i步的所有方式，则`dp[i] = dp[i-1] + dp[i-2]`。T: O(n), S: O(n)
- 动态规划五部曲：dp数组及下标含义，递推公式，初始化，遍历顺序，举例验证
* 简洁写法：动规一般依赖于前两个或前三个状态，因此可以将空间优化为O(1)

```cpp
class Solution {
public:
    int climbStairs(int n) {
        vector<int> dp(n + 1);
        dp[0] = 1;
        dp[1] = 1;
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
};
```

### 方法二

- 动态规划，完全背包问题。楼梯数是背包，1步或2步是物品。可扩充到最多m步的情况。T: O(n), S: O(n)

```cpp
class Solution {
public:
    int climbStairs(int n) {
        vector<int> dp(n + 1, 0);
        dp[0] = 1;
        for (int j = 0; j <= n; j++) {
            for (int i = 1; i <= 2; i++) {
                if (j >= i) dp[j] += dp[j - i];
            }
        }
        return dp[n];
    }
};
```

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

- 动态规划。`dp[i][j]`表示`word1[0~i]`转换为`word2[0~j]`需要的最少的操作次数。T: O(n^2), S: O(n^2)
- 注意动态数组的初始化，根据第i与第j个字符是否相等，选择不同的递归公式

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

## 1143. Longest Common Subsequence

>  :orange_circle:

求两个字符串的最长公共子序列

### 方法

- 动态规划，子序列不连续，`dp[i][j]`代表以`text1[i]`与`text2[j]`结尾的两个字符串的最长公共子序列。T: O(mn), S: O(mn)

```cpp
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int m = text1.size(), n = text2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1[i - 1] == text2[j - 1]) dp[i][j] = dp[i - 1][j - 1] + 1;
                else dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
        return dp[m][n];
    }
};
```

## 0198. House Robber

> :orange_circle:

打劫房屋，同时打劫两个相邻房屋时会自动报警。给每个房屋的金额，找到能打劫到的最大金额

### 方法

- 动态规划， dp[i] 考虑下标i-1以内的房屋，最多能打劫到的金额为dp[i]。T: O(n), S: O(n)
- `dp[i] = max(dp[i-2] + nums[i-1], dp[i-1]); dp[0] = 0, dp[1] = nums[0]`

```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(n + 1);
        dp[1] = nums[0];
        for (int i = 2; i <= n; i++) {
            dp[i] = max(dp[i - 1], dp[i - 2] + nums[i - 1]);
        }
        return dp[n];
    }
};
```

