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

整数数组。一个整数可以被表达，当其是数组的子序列的或值。返回最小的非零正整数，没有被该数组所表达。

### 方法

-  从1,2,3,4...依次向上找，第一个不能由数组子序列的或值得到，即为所求。
- 对一个数(b1010...01)，与其他数的或值一定大于等于该数，与所有小于该数的或值，能得到的最大数为(b1111...11)
- 因此只要数组中包含2的幂，(b1, b10, b100...)，数组能表达的数可以一直增大，其不能表达的数也一定是下一个幂
- 可以将数组排序，从小到大寻找不能表达的2的幂。也可以将数组转换为集合，进行查找

```cpp
class Solution {
public:
    int minImpossibleOR(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int r = 1;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] == r) r *= 2;
            else if (nums[i] > r) return r;
        }
        return r;
    }
};
```

## 2569. Handling Sum Queries After Update

> :red_circle:

