# Weekly Contest 326
> 2023.01.01

## 2520. Count the Digits That Divide a Number

> :green_circle:

给一个整数`num`，返回能够整除`num`的`num`中的数字个数。`num`中不含0

### 方法一

- 通过除以10取出整数中的每个数字，判断是否能整除该整数，计数

```cpp
class Solution {
public:
    int countDigits(int num) {
        int ans = 0;
        int a = num;
        while (a) {
            int b = a % 10;
            a /= 10;
            if (num % b == 0) ans++;
        }
        return ans;
    }
};
```

### 方法二

- 将其转化为字符串，遍历取出每一位

```cpp
class Solution {
public:
    int countDigits(int num) {
        int ans = 0;
        for (char& c : to_string(num)) {
            if (num % (c - '0') == 0) ans++;
        }
        return ans;
    }
};
```

## 2521. Distinct Prime Factors of Product of Array

> :orange_circle:

正整数数组，返回数组中所有元素乘积的**不同质因数**的个数。`1 <= nums.length <= 10^4; 2 <= nums[i] <= 1000`

### 方法

- 直接计算所有元素乘积会溢出，找出数组中每个元素的质因数，使用集合对质因数去重，最后返回集合大小。
- 找元素n的质因数时，从2开始到sqrt(n)结束

```cpp
class Solution {
public:
    int distinctPrimeFactors(vector<int>& nums) {
        unordered_set<int> ans;
        for (int n : nums) {
            if (n % 2 == 0) {
                ans.insert(2);
                while (n % 2 == 0) n /= 2;
            }
            for (int i = 3; i * i <= n; i += 2) {
                if (n % i == 0) {
                    ans.insert(i);
                    while (n % i == 0) n /= i;
                }
            }
            if (n > 1) ans.insert(n);
        }
        return ans.size();
    }
};
```

## 2522. Partition String Into Substrings With Values at Most K

> :orange_circle:

给一个由数字组成的字符串，其划分称为好划分：每个数字只属于一个子串，每个子串的值小于等于k。返回好划分中最少的子串数目，没有好划分返回-1

### 方法

## 2523. Closest Prime Numbers in Range

> :orange_circle:

给两个正整数`left`和`right`，找到两个整数满足：`left <= nums1 < nums2 <= right `，都是质数，`nums2 - nums1`最小。返回`ans = [nums1, nums2]`，如果有多个返回`nums1`最小的，不存在则返回`[-1,-1]`。`1 <= left <= right <= 10^6`

### 方法
