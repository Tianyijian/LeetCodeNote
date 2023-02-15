# Weekly Contest 329
> 2023.01.22

## 2544. Alternating Digit Sum

> :green_circle:

给一个正整数，每位数字都有正负号：最高位的数字符号为正，其它数字的符号和它相邻的符号相反。返回所有数字的和

### 方法

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

## 2545. Sort the Students by Their Kth Score

> :orange_circle:

## 2546. Apply Bitwise Operations to Make Strings Equal

> :orange_circle:

## 2547. Minimum Cost to Split an Array

> :red_circle: