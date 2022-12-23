# 买卖股票问题

## 0121. Best Time to Buy and Sell Stock

> :green_circle:

给定一维数组，显示出股票每天的价格，选择某一天买入，未来的某一天卖出，返回交易的最大利润，没有利润则为0

### 方法一

* 动态规划-买卖股票，一次交易
* `dp[i][0]` 第i天持有股票，`dp[i][1]`第i天不持有股票，得到的最大现金
* `dp[i][0]`：第i-1天持有，第i天买入；`dp[i][1]`：第i-1天不持有，第i天卖出
* 最后一定是不持有股票金额最多，`dp[size-1][1]`

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> dp(n, vector<int>(2, 0));
        dp[0][0] = -prices[0];
        for (int i = 1; i < n; i++) {
            dp[i][0] = max(dp[i - 1][0], -prices[i]);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][0] + prices[i]);
        }
        return dp[n - 1][1];
    }
};
```

### 方法二


* 贪心，一遍遍历，取最左最小值作为买入点，最右最大值作为卖出点

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res = 0;
        int minLeft = INT_MAX;
        for (int i = 0; i < prices.size(); i++) {
            minLeft = min(prices[i], minLeft);
            res = max(res, prices[i] - minLeft);
        }
        return res;
    }
};
```

## 0309. Best Time to Buy and Sell Stock with Cooldown

> :orange_circle:

给定一维数组，显示出股票每天的价格。可以交易很多次，但卖出股票后下一天不能买入（冷冻期），必须卖出股票才能买入下一只股票

### 方法

- 动态规划-买卖股票，含冷冻期。T: O(n), S: O(n)
    * `dp[i][0]`买入状态，`dp[i][1]` 卖出状态，`dp[i][2]` 今日卖出，`dp[i][3]` 冷冻期
    * 递推公式：分析清楚达到每个状态，需要前一天是什么状态

```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int n = prices.size();
        vector<vector<int>> dp(n, vector<int>(4, 0));
        dp[0][0] = -prices[0];
        for (int i = 1; i < n; i++) {
            dp[i][0] = max(dp[i - 1][0], max(dp[i - 1][1], dp[i - 1][3]) - prices[i]);
            dp[i][1] = max(dp[i - 1][1], dp[i - 1][3]);
            dp[i][2] = dp[i - 1][0] + prices[i];
            dp[i][3] = dp[i - 1][2];
        }
        return max(dp[n - 1][1], max(dp[n - 1][2], dp[n - 1][3]));
    }
};
```

