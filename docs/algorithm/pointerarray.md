# 双指针解决数组问题

## 283. Move Zeroes

> :green_circle:

一维数组，将其中的零移动到数组最后，其它元素保持顺序不变。要求in-place操作，不复制数组，尽可能操作次数最小。

### 方法一

* 新建数组，将非零元素依次复制到新数组，再赋值给原数组。T: O(2n), S: O(n), 操作次数2n
```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        vector<int> newNums(nums.size(), 0);
        int index = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != 0) newNums[index++] = nums[i];
        }
        for (int i = 0; i < nums.size(); i++) {
            nums[i] = newNums[i];
        }
    }
};
```
### 方法二

* 快慢指针移动元素，最后将慢指针之后的元素赋值为0。T: O(n), S: O(1), 操作次数n

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int slow = 0;
        for (int fast = 0; fast < nums.size(); fast++) {
            if (nums[fast] != 0) {
                nums[slow++] = nums[fast];
            }
        }
        for (int i = slow; i < nums.size(); i++) {
            nums[i] = 0;
        }
    }
};
```
### 方法三

* 统计出每个元素前面出现的0的个数，即为其向前移动的位数。T: O(n), S: O(1), 非零元素数< 操作次数< n

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int count = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] == 0) count++;
            else if (count > 0) nums[i - count] = nums[i];
        }
        for (int i = nums.size() - count; i < nums.size(); i++) {
            if (nums[i] != 0) nums[i] = 0;
        }
    }
};
```
### 方法四

* 双指针，直接交换元素。T: O(n), S: O(1), 操作次数=非零元素数

```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int slowIndex = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] != 0) {
                swap(nums[slowIndex], nums[i]);
                slowIndex++;
            }
        }
    }
};
```

