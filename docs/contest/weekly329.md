# Weekly Contest 329
> 2023.01.22

## 2544. Alternating Digit Sum

> :green_circle:

给一个正整数，每位数字都有正负号：最高位的数字符号为正，其它数字的符号和它相邻的符号相反。返回所有数字的和

### 方法一

- 将数字转为字符串，从高位到低位依次遍历，符号依次为正和负。T: O(logn), S: O(logn)，logn代表数字n的位数

```cpp
class Solution {
public:
    int alternateDigitSum(int n) {
        string s = to_string(n);
        int ans = 0, sign = 1;
        for (int i = 0; i < s.size(); i++) {
            ans += (s[i] - '0') * sign;
            sign *= -1;
        }
        return ans;
    }
};
```

### 方法二

- 从右向左，依次取出位数进行加和。T: O(logn), S: O(1)
- 设立符号位，从负号开始，数字为偶数位时首位为正，数字为奇数位时首位为负，因此最后再乘以符号即可。

```cpp
class Solution {
public:
    int alternateDigitSum(int n) {
        int ans = 0, sign = 1;
        while (n > 0) {
            sign *= -1;
            ans += sign * (n % 10);
            n /= 10;
        }
        return sign * ans;
    }
};
```

## 2545. Sort the Students by Their Kth Score

> :orange_circle:

m个学生n场考试，`mxn`矩阵代表学生的分数，其中分数都不同。将矩阵按照第K场考试从高分到低分排列

### 方法

- 利用sort函数，自定义排序接口即可

```cpp
class Solution {
public:
    vector<vector<int>> sortTheStudents(vector<vector<int>>& score, int k) {
        sort(score.begin(), score.end(), [&](auto const &a, auto const &b) {
            return a[k] > b[k];
        });
        return score;
    }
};
```

## 2546. Apply Bitwise Operations to Make Strings Equal

> :orange_circle:

## 2547. Minimum Cost to Split an Array

> :red_circle: