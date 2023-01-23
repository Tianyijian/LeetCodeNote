# Weekly Contest 328
> 2023.01.15

## 2535. Difference Between Element Sum and Digit Sum of an Array
> :green_circle:

正整数数组，元素和是数组中所有元素的和，数字和是数组中所有数字的和。返回两者的绝对差值

### 方法

- 对每个元素，累加元素和，取出每个数字累加数字和，最后相减求绝对值
- 简化：元素和可以直接减去每个数字；元素和始终大于数字和，最后可以不用绝对值

```cpp
class Solution {
public:
    int differenceOfSum(vector<int>& nums) {
        int eSum = 0, dSum = 0;
        for (int& i : nums) {
            eSum += i;
            while (i > 0) {
                dSum += i % 10;
                i /= 10;
            } 
        }
        return abs(eSum - dSum);
    }
};
```

## 2536. Increment Submatrices by One

> :orange_circle:

## 2537. Count the Number of Good Subarrays
> :orange_circle:

给一个整数数组和整数k，返回**好子数组**的数量。**好子数组**`arr`至少有k对下标`(i, j)`满足`i < j and arr[i] < arr[j]`

### 方法

- 子数组是连续的，采用滑动窗口。记录窗口内每个值的数量，扩大或缩小窗口时增加或减去相等值的个数

```cpp
class Solution {
public:
    long long countGood(vector<int>& nums, int k) {
        int n = nums.size();
        long long ans = 0;
        int cnt = 0;
        unordered_map<int, int> map;
        for (int i = 0, j = 0; j < n; j++) {
            cnt += map[nums[j]];
            map[nums[j]] += 1;
            while (cnt >= k) {
                ans += n - j;
                map[nums[i]] -= 1;
                cnt -= map[nums[i]];
                i++;
            }
        }
        return ans;
    }
};
```

## 2538. Difference Between Maximum and Minimum Price Sum
> :red_circle: