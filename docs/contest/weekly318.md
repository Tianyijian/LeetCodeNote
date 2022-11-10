# Weekly Contest 318
> 2022.11.06

## 2460. Apply Operations to an Array

> :green_circle:

非负数组，先进行n-1个操作，`nums[i]==nums[i+1]时，nums[i]*2，nums[i+1]=0`，之后将所有的零移动到数组最后

相似题目：283. Move Zeroes

### 方法

- 第一步的n-1个操作直接遍历数组即可，第二步为移动数组的零元素，要注意保持非负元素的顺序不变
- 可以直接移动非零元素到新数组，或者双指针移动零元素。T: O(n), S: O(1)

```cpp
class Solution {
public:
    vector<int> applyOperations(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                nums[i] *= 2;
                nums[i + 1] = 0;
            }
        }
        int slow = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] != 0) swap(nums[i], nums[slow++]);
        }
        return nums;
    }
};
```

## 2461. Maximum Sum of Distinct Subarrays With Length K

> :orange_circle:

给一维数组及整数k，找到长度为k的子数组，且元素各不相同，求最大的子数组元素和，没有满足条件的子数组返回0

### 方法

- 滑动窗口，维护固定大小为K的窗口，当满足元素各不相同时，记录最大元素和

## 2462. Total Cost to Hire K Workers

>  :orange_circle:

## 2463. Minimum Total Distance Traveled

> :red_circle:
