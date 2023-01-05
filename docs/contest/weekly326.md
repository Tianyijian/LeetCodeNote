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

### 方法一

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

### 方法二

- `nums[i]`最大为1000，可以先找出sqrt(1000)以内的质数，作为因子。或者直接找出1000以内的质数，统计是因数的数量
- Sieve of Eratosthenes：找出n以内的所有质数。从2开始，迭代标记每个质数的倍数，最终剩余的未标记数即为质数

```cpp
class Solution {
public:
    int distinctPrimeFactors(vector<int>& nums) {
        vector<int> primes = sieveOfEratosthenes(sqrt(1000)); // {2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31}
        unordered_set<int> ans;
        for (int n : nums) {
            for (int p : primes) {
                if (n % p == 0) {
                    ans.insert(p);
                    while (n % p == 0) n /= p;
                }
            }
            if (n > 1) ans.insert(n);
        }
        return ans.size();
    }
private: 
    vector<int> sieveOfEratosthenes(int n) { 	// T: O(n*log(log(n))), S: O(n)
        vector<bool> prime(n + 1, 1);
        for (int p = 2; p * p <= n; p++) {
            if (prime[p]) {
                for (int i = p * p; i <= n; i += p) {
                    prime[i] = 0;
                }
            }
        }
        vector<int> ans;
        for (int p = 2; p <= n; p++) {
            if (prime[p]) ans.push_back(p);
        }
        return ans;
    }
};
```

## 2522. Partition String Into Substrings With Values at Most K

> :orange_circle:

给一个由数字组成的字符串，其划分称为好划分：每个数字只属于一个子串，每个子串的值小于等于k。返回好划分中最少的子串数目，没有好划分返回-1

### 方法

- 贪心思想，在子串值小于等于k的前提下，每个子串尽可能的长，从而子串数目最少。T: O(n), S: O(1)
- 不断记录当前的值，大于k时开始一个新划分。单独一位数就比k大时，不能划分返回-1

```cpp
class Solution {
public:
    int minimumPartition(string s, int k) {
        long n = 0, ans = 1;
        for (int i = 0; i < s.size(); i++) {
            n = n * 10 + (s[i] - '0');
            if (n > k) {
                ans++;
                n = s[i] - '0';
            }
            if (n > k) return -1;
        }
        return ans;
    }
};
```

## 2523. Closest Prime Numbers in Range

> :orange_circle:

给两个正整数`left`和`right`，找到两个整数满足：`left <= nums1 < nums2 <= right `，都是质数，`nums2 - nums1`最小。返回`ans = [nums1, nums2]`，如果有多个返回`nums1`最小的，不存在则返回`[-1,-1]`。`1 <= left <= right <= 10^6`

### 方法

- Sieve of Eratosthenes方法找到[left, right]之间的所有质数，然后找出差值最小的结果

```cpp
class Solution {
public:
    vector<int> closestPrimes(int left, int right) {
        vector<bool> prime(right + 1, 1);
        prime[1] = 0;
        for (int p = 2; p * p <= right; p++) {
            if (prime[p]) {
                for (int i = p * p; i <= right; i += p) prime[i] = 0;
            }
        }
        int nums1 = 0, diff = INT_MAX;
        vector<int> ans = {-1, -1};
        for (int i = left; i <= right; i++) {
            if (prime[i]) {
                if (nums1 && diff > i - nums1) {
                    diff = i - nums1;
                    ans = {nums1, i};
                }
                nums1 = i;
            }
        }
        return ans;
    }
};
```

