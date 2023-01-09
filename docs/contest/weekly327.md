# Weekly Contest 327
> 2023.01.08

## 2529. Maximum Count of Positive Integer and Negative Integer

> :green_circle:

给一个非递减的数组，返回正数和负数数量的最大值。`1 <= nums.length <= 2000; -2000 <= nums[i] <= 2000`

### 方法

- 遍历统计负数的个数和正数的个数，或者统计负数和0的个数，总数相减得到正数个数。T: O(n), S: O(1)
- 二分搜索：数组有序，采用二分搜索，直接查找正数和负数的下标位置，进而得到数量。T: O(logn), S: O(1)

```cpp
class Solution {
public:
    int maximumCount(vector<int>& nums) {
        int pos = nums.end() - upper_bound(nums.begin(), nums.end(), 0);
        int neg = lower_bound(nums.begin(), nums.end(), 0) - nums.begin();
        return max(pos, neg);
    }
};
```

## 2530. Maximal Score After Applying K Operations

> :orange_circle:

给一个整数数组，起始分为0。一次操作如下：选择一个下标i，分数加上`nums[i]`，将`nums[i]`替换为`ceil(nums[i] / 3)`。返回k次操作后最大可能的分数

### 方法

- 贪心思想，每次选择数组中最大的值加入分数即可。采用大顶堆动态维持。C++的堆在初始化时是线性时间O(n)，T: O(n+ klogn), S: O(n)

```cpp
class Solution {
public:
    long long maxKelements(vector<int>& nums, int k) {
        long long ans = 0;
        priority_queue<int> q(begin(nums), end(nums));
        while (k--) {
            int a = q.top();
            ans += a;
            q.pop();
            q.push((a + 2) / 3);
        }
        return ans;
    }
};
```

## 2531. Make Number of Distinct Characters Equal

> :orange_circle:

## 2532. Time to Cross a Bridge

> :red_circle:
