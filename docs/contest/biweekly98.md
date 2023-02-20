# Biweekly Contest 98
> 2023.02.18

## 2566. Maximum Difference by Remapping a Digit

> :green_circle:

给一个整数，可以将0-9中的一个数字改为另一个数字。通过改变一个数字，返回最大值与最小值的差值。改变的是整数中所有该数字，改后的整数可以左侧有0

### 方法

- 改为最大：从最高位开始，将第一个不是9的数字改为9。改为最小：从最高位开始，第一个不是0的数字改为0。

```cpp
class Solution {
public:
    int minMaxDifference(int num) {
        string max = to_string(num), min = to_string(num);
        for (char c : max) {
            if (c != '9') {
                replace(max.begin(), max.end(), c, '9');
                break;
            }
        }
        for (char c : min) {
            if (c != '0') {
                replace(min.begin(), min.end(), c, '0');
                break;
            }
        }
        return stoi(max) - stoi(min);
    }
};
```

## 2567. Minimum Score by Changing Two Elements

> :orange_circle:

整数数组，low是数组中任两个元素的最小差值，high是最大差值，两者和称为数组的score。可以最多改变数组的两个元素，返回最小的可能score。

### 方法

- 将数组排序，便于找到最大与最小值，high的值为数组两端的值的差值。改变数组两端的值，有重复元素可以使low为0。
- 让high变小有三种情况：改变最小的两个，改变最大的两个，改变最小与最大各一个。找到三种情况的最小值即可。

```cpp
class Solution {
public:
    int minimizeSum(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int n = nums.size();
        return min({nums[n - 1] - nums[2], nums[n - 2] - nums[1], nums[n - 3] - nums[0]});
    }
};
```

## 2568. Minimum Impossible OR

> :orange_circle:

## 2569. Handling Sum Queries After Update

> :red_circle:

