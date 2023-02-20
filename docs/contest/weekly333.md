# Weekly Contest 333
> 2023.02.19

## 2570. Merge Two 2D Arrays by Summing Values
> :green_circle:

两个二维整数数组，每个元素有id和值。同一数组所含的id不相同，元素以id升序排列。将两个数组合并，相同id的值相加。返回结果数组，同样根据id升序

### 方法

- 双指针，从左至右分别遍历两个数组，将最小id对应的数值加和存入结果。T: O(m+n), S: O(1)
- 也可以采用有序map，存储每个id及其值，然后导入结果数组

```cpp
class Solution {
public:
    vector<vector<int>> mergeArrays(vector<vector<int>>& nums1, vector<vector<int>>& nums2) {
        vector<vector<int>> ans;
        int m = nums1.size(), n = nums2.size();
        int i = 0, j = 0;
        while (i < m || j < n) {
            int id = i < m ? nums1[i][0] : INT_MAX;
            if (j < n) id = min(id, nums2[j][0]);
            int sum = 0;
            while (i < m && nums1[i][0] == id) {
                sum += nums1[i++][1];
            }
            while (j < n && nums2[j][0] == id) {
                sum += nums2[j++][1];
            }
            ans.push_back({id, sum});
        }
        return ans;
    }
};
```

## 2571. Minimum Operations to Reduce an Integer to 0

> :orange_circle:

给一个正整数n，可以添加或者减去一个2的幂。返回将其变为0的最少的操作次数。`1 <= n <= 10^5`

### 方法

- 找到距其最近的2的幂，其差值即为新的n。可以先把需要的2的幂存储下来，然后利用二分查找。

```cpp
class Solution {
public:
    int minOperations(int n) {
        vector<int> nums;
        int i = 0;
        for (i = 1; i <= n; i *= 2) {
            nums.push_back(i);
        }
        nums.push_back(i);
        int ans = 0;
        while (n != 0) {
            int id = upper_bound(nums.begin(), nums.end(), n) - nums.begin();
            n = min(nums[id] - n, n - nums[id - 1]);
            ans++;
        }
        return ans;
    }
};
```

## 2572. Count the Number of Square-Free Subsets

> :orange_circle:

## 2573. Find the String with LCP
> :red_circle: