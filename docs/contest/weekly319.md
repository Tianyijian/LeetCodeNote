# Weekly Contest 319
> 2022.11.13

## 2469. Convert the Temperature

> :green_circle:

将浮点数celsius温度转换为Kelvin和Fahrenheit温度，`Kelvin = Celsius + 273.15`，`Fahrenheit = Celsius * 1.80 + 32.00` 

### 方法

```cpp
class Solution {
public:
    vector<double> convertTemperature(double celsius) {
        vector<double> ans(2);
        ans[0] = celsius + 273.15;
        ans[1] = celsius * 1.80 + 32.00;
        return ans;
    }
};
```

## 2470. Number of Subarrays With LCM Equal to K

> :orange_circle:

## 2471. Minimum Number of Operations to Sort a Binary Tree by Level

> :orange_circle:

## 2472. Maximum Number of Non-overlapping Palindrome Substrings

> :red_circle:
