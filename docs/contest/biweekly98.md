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

## 2568. Minimum Impossible OR

> :orange_circle:

## 2569. Handling Sum Queries After Update

> :red_circle:

