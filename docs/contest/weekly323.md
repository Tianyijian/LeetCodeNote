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

