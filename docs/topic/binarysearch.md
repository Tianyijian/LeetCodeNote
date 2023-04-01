# 二分查找

## 0035. Search Insert Position

> :green_circle:

给一个值不相同的有序数组，和整数target。如果有target，返回index，如果没有，找到它应该被插入的位置。要求O(logn)

### 方法

- 二分查找，找到直接返回，没找到返回第一个大于target的下标。T: O(logn), S: O(1)

```cpp
class Solution {
public:
    int searchInsert(vector<int>& nums, int target) {
        int left = 0, right = nums.size() - 1;
        while (left <= right) {
            int mid = left + (right - left) / 2;
            if (nums[mid] > target) right = mid - 1;
            else if (nums[mid] < target) left = mid + 1;
            else return mid;
        }
        return left;
    }
};
```

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

## 0704. Binary Search

> :green_circle:

给一个升序排序的整数数组，从中查找是否包含整数target，包含返回其下标，不包含返回-1。时间要求O(logn)

### 方法

- 二分查找

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0, r = nums.size() - 1;
        while (l <= r) {
            int m = l + (r - l) / 2;
            if (nums[m] < target) l = m + 1;
            else if (nums[m] > target) r = m - 1;
            else return m;
        }
        return -1;
    }
};
```

