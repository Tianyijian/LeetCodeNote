# Weekly Contest 337
> 2023.03.19

## 2595. Number of Even and Odd Bits
> :green_circle:

给一个正整数n，其二进制表示中，偶数位置为1的个数为`even`，奇数位置为1的个数为`odd`，返回`[even, odd]`

### 方法

- 通过计算依次得到正整数的二进制位，依次统计even和odd的值

```cpp
class Solution {
public:
    vector<int> evenOddBit(int n) {
        vector<int> ans(2, 0);
        int i = 0;
        while (n) {
            if (n % 2 == 1) ans[i % 2]++;
            i++;
            n /= 2;
        }
        return ans;
    }
};
```

## 2596. Check Knight Tour Configuration

> :orange_circle:

## 2597. The Number of Beautiful Subsets
> :orange_circle:

## 2598. Smallest Missing Non-negative Integer After Operations
> :orange_circle: