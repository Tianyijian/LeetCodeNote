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