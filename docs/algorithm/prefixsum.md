# 前缀和数组
## 介绍

- 前缀和技巧适用于快速、频繁地计算一个索引区间内的元素之和，用空间换时间
- 前缀和数组记录下标i之前的所有数组元素的总和，通过减法在O(1)时间内得到区间元素和
- `preSum[0] = 0, preSum[1] = nums[0], preSum[i] = sum(nums[0, i-1]) = preSum[i-1] + nums[i-1];`
- `sum(num[left, right]) = preSum[right+1]-preSum[left]`

## 0303. Range Sum Query - Immutable

> :green_circle:

给定一维数组，多次查询[left, right]范围内的元素总和

### 方法

- 构建一维前缀和数组即可

```cpp
class NumArray {
public:
    NumArray(vector<int>& nums) {
        preSum = vector<int>(nums.size() + 1, 0);
        for (int i = 0; i < nums.size(); i++) {
            preSum[i + 1] = preSum[i] + nums[i];
        }
    }
    
    int sumRange(int left, int right) {
        return preSum[right + 1] - preSum[left];
    }
private:
    vector<int> preSum;
};
```

## 0304. Range Sum Query 2D - Immutable

>  :orange_circle:

### 方法

- 构建二维前缀和数组，`preSum[i][j] = sum(matrix[0, i-1][0, j - 1])`

```cpp
class NumMatrix {
public:
    NumMatrix(vector<vector<int>>& matrix) {
        int m = matrix.size();
        int n = matrix[0].size();
        preSum = vector<vector<int>>(m + 1, vector<int>(n + 1, 0));
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                preSum[i][j] = preSum[i - 1][j] + preSum[i][j - 1] - preSum[i - 1][j - 1] + matrix[i - 1][j - 1];
            }
        }
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        return preSum[row2 + 1][col2 + 1] - preSum[row1][col2 + 1] - preSum[row2 + 1][col1] + preSum[row1][col1];
    }
private:
    vector<vector<int>> preSum;
};
```

## 0560. Subarray Sum Equals K

> :orange_circle:

给定一维数组和整数k，寻找元素和等于K的子数组，返回子数组数量。数组元素取值[-1000, 1000]

### 方法

- 数组元素有正数负数和0，不能保证滑动窗口移动时的总和大小
- 动态规划，`dp[i][j]`下标i-j的子数组的和