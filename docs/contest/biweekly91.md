# Biweekly Contest 91
## 2465. Number of Distinct Averages

> :green_circle:

给定整数数组，长度为偶数，每次移除最大值和最小值，并记录两者的平均值，重复移除直至数组为空，返回独特的平均数个数

### 方法

- 数组排序后，左右指针依次向中间移动，使用集合记录两者的平均值，最后返回集合大小。T: O(nlogn), S: O(n)
- 注意平均值为double，要除以2.0。简便写法：直接添加两者和即可，不用计算平均值

```cpp
class Solution {
public:
    int distinctAverages(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int left = 0, right = nums.size() - 1;
        unordered_set<double> set; // set<int> set;
        while (left < right) {
            double num = (nums[left] + nums[right]) / 2.0; // set.insert(nums[left] + nums[right]);
            set.insert(num);
            left++;
            right--;
        }
        return set.size();
    }
};
```

## 2466. Count Ways To Build Good Strings

> :orange_circle:

从空串开始构建字符串，每次可以选择添加zero个'0'，或者one个'1'，最终长度在[low, high]之间的字符串为好串，返回不同好串的个数。由于数据很大，结果对10^9 + 7取余

### 方法

- 动态规划，dp[i]代表构建长度为i的子串的个数，其有两个来源，dp[i] = dp[i-zero] + dp[i-one]，初始化dp[0]=1
- 多个数据相加之后取余==每个数据取余之后再加和，因此计算过程中的每个结果都取余，防止数值溢出
- 注意`10^9+7`写法为`1e9+7`，`INT_MAX = 2^31-1 = 2147483647 > 2x1e9`

```cpp
class Solution {
public:
    int countGoodStrings(int low, int high, int zero, int one) {
        vector<int> dp(high + 1, 0);
        dp[0] = 1;
        int res = 0;
        int mod = 1e9 + 7;
        for (int i = 1; i <= high; i++) {
            if (i >= zero) dp[i] += dp[i - zero];
            if (i >= one) dp[i] += dp[i - one];
            dp[i] = dp[i] % mod;
            if (i >= low) {
                res += dp[i];
                res = res % mod;
            }
        }
        return res;
    }
};
```

## 2467. Most Profitable Path in a Tree

> :orange_circle:

## 2468. Split Message Based on Limit

> :red_circle: