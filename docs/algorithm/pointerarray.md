# 双指针解决数组问题

## 介绍

- 双指针技巧主要分为两类：**左右指针**和**快慢指针**，数组的索引当作指针
- 左右指针，是两个指针相向而行或者相背而行；快慢指针，是两个指针同向而行，一快一慢
- 快慢指针一般要求原地操作且不使用额外内存，可以有序数组去重，删除元素，移动元素，构建滑动窗口
- 左右指针一般用来二分查找，反转数组，判断回文子串
- 学习资源
  - https://labuladong.gitee.io/algo/1/5/

## 0026. Remove Duplicates from Sorted Array

> :green_circle:

一维非减数组，移除重复的元素，使每个元素出现一次并且顺序不变，返回移除后的数组个数k。要求in-place, O(1) extra memory

### 方法

双指针，`nums[0-slow]`为去重后元素，快指针与慢指针的数不同时，快指针的数复制到慢指针的下一位。T: O(n), S: O(1)

```cpp
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int slow = 0;
        for (int fast = 0; fast < nums.size(); fast++) {
            if (nums[fast] != nums[slow]) {
                nums[++slow] = nums[fast];
            }
        }
        return slow + 1;
    }
};
```

## 0027. Remove Element

> :green_circle:

给定一维数组和一个值，移除数组中等于该值的元素，顺序可以改变，返回新数组的长度，要求原地移除并且不使用额外空间

### 方法

- 双指针，快指针寻找符合条件的元素，慢指针记录新数组位置，快指针的值不等于移除的值时，将其赋值给慢指针。

* 先判断快指针是否符合条件，再赋值，更新慢指针。T: O(n), S: O(1)

```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slow = 0;
        for (int fast = 0; fast < nums.size(); fast++) {
            if (nums[fast] != val) {
                nums[slow++] = nums[fast];
            }
        }
        return slow;
    }
};
```

## 0283. Move Zeroes

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

## 0167. Two Sum II - Input Array Is Sorted

> :orange_circle:

给定下标从1开始的非减数组，找到和为target的两个值，返回这两个值的下标，解唯一

### 方法

左右指针相向而行，因为数组升序，所以当前和大时，右指针左移，当前和小时，左指针右移。T: O(n), S: O(1)

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int left = 0, right = numbers.size() - 1;
        while (left <= right) {
            int sum = numbers[left] + numbers[right];
            if (sum > target) right--;
            else if (sum < target) left++;
            else return {left + 1, right + 1};
        }
        return {-1, -1};
    }
};
```

## 0344. Reverse String

>  :green_circle:

给定一个字符数组，将其反转，要求in-place并且O(1)额外内存

### 方法

左右指针，同步向中间移动，并交换值

```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        int left =  0, right = s.size() - 1;
        while (left < right) {
            swap(s[left], s[right]);
            left++;
            right--;
        }
    }
};
```

## 0005. Longest Palindromic Substring

> :orange_circle:

返回字符串s中的最长回文子串

### 方法一

- 双指针，左右指针从中心相背而行，以字符i为中心点，向两边扩散，判断是否回文

* 回文串的长度可能是奇数或偶数，从中心向两端扩散，中心点元素个数可能为1个或2个。T(n) = O(n^2), S(n) = O(1)

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        for (int i = 0; i < s.size(); i++) {
            extend(s, i, i);
            extend(s, i, i + 1);
        }
        return s.substr(left, right - left + 1);
    }
private:
    int left = 0;
    int right = 0;
    void extend(const string& s, int i, int j) {
        while (i >= 0 && j < s.size() && s[i] == s[j]) {
            if (j - i + 1 > right - left + 1) {
                left = i;
                right = j;
            }
            i--;
            j++;
        }
    }
};
```

### 方法二

* 动态规划判断回文子串
	* `dp[i][j]` i~j是否为回文子串，最后记录最长的子串
	* `s[i] != s[j]: fasle; s[i] == s[j]: i == j, true; i == j - 1, true; dp[i + 1][j - 1]`
	* T(n) = O(n^2), S(n) = O(n^2)

```cpp
class Solution {
public:
    string longestPalindrome(string s) {
        int n = s.size();
        vector<vector<bool>> dp(n, vector<bool>(n, false));
        string res = "";
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (s[i] == s[j]) {
                    if (i == j || i == j - 1 || dp[i + 1][j - 1]) dp[i][j] = true;
                    if (dp[i][j] && j - i + 1 > res.size()) res = s.substr(i, j - i + 1);
                }
            }
        }
        return res;
    }
};
```
