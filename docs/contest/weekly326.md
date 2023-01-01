# Weekly Contest 326
> 2023.01.01

## 2520. Count the Digits That Divide a Number

> :green_circle:

给一个整数`num`，返回能够整除`num`的`num`中的数字个数

### 方法

- 取出整数中的每个数字，判断是否能整除该整数，计数

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

