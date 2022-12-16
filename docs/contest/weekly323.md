# Weekly Contest 323
> 2022.12.11

## 2500. Delete Greatest Value in Each Row

> :green_circle:

`mxn`二维正数矩阵，进行如下操作直至矩阵为空：删除每行中最大的值，将删除的最大值加入答案。每次操作后矩阵减少一列，返回得到的答案

### 方法

- 将每一行排好序，然后按照列取最大值，加入答案。T: O(M*NlogN), S: O(1)
- 从最大往最小加和，与从最小往最大加和，结果一样，因此升序与降序排列均可

```cpp
class Solution {
public:
    int deleteGreatestValue(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        for (auto& row : grid) sort(begin(row), end(row));
        int ans = 0;
        for (int j = 0; j < n; j++) {
            int maxRow = 0;
            for (int i = 0; i < m; i++) maxRow = max(maxRow, grid[i][j]);
            ans += maxRow;
        }
        return ans;
    }
};
```

## 2501. Longest Square Streak in an Array

> :orange_circle:

给整数数组，其子序列称做**方波**：长度至少为2，排序后后一个元素是前一个元素的平方。返回最长的**方波**的长度，没有则返回-1

### 方法

- 采用有序的哈希map记录元素及其作为方波结尾时的长度，从小到大遍历，如果一个数的平方数在map中，将其长度加上当前数的长度。T: O(nlogn), S: O(n)

```cpp
class Solution {
public:
    int longestSquareStreak(vector<int>& nums) {
        map<int, int> map;
        for (int num : nums) map[num] = 1;
        int maxVal = sqrt(map.rbegin()->first);
        int ans = 1;
        for (auto [k, v] : map) {
            if (k > maxVal) break;
            int n = k * k;
            if (map.find(n) != map.end()) {
                map[n] += v;
                ans = max(ans, map[n]);
            }
        }
        return ans == 1 ? -1 : ans;
    }
};
```

