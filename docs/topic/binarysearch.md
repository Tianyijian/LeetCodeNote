# 二分查找

## 0240. Search a 2D Matrix II

> :orange_circle:

给一个二维整数矩阵，每行每列都是升序，查询是否存在某个值

### 方法一

- 从右上角开始查找，当前值大于目标值时，则当前列减1；当前值小于目标值时，当前行加1。T: O(m + n), S: O(1)

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = 0, n = matrix[0].size() - 1;
        while (n >= 0 && m < matrix.size()) {
            if (matrix[m][n] > target) {
                n--;
            } else if (matrix[m][n] < target) {
                m++;
            } else return true;
        }
        return false;
    }
};
```

### 方法二

- 二分搜索，在二维矩阵上直接进行二分搜索时，会漏掉一些矩阵，以下代码有问题

```cpp
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size();
        int lRow = 0, lCol = 0, rRow = m - 1, rCol = n - 1;
        while(lRow < rRow - 1 && lCol < rCol - 1) {
            int midRow = lRow + (rRow - lRow) / 2;
            int midCol = lCol + (rCol - lCol) / 2;
            if (matrix[midRow][midCol] > target) {
                rRow = midRow;
                rCol = midCol;
            } else if (matrix[midRow][midCol] < target) {
                lRow = midRow;
                lCol = midCol;
            } else return true;
        }
        for (int i = lRow; i <= rRow; i++) {
            for (int j = lCol; j <= rCol; j++) {
                if (matrix[i][j] == target) return true;
            }
        }
        return false;
    }
};
```

