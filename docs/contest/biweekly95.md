# Biweekly Contest 94
> 2023.01.07

## 2525. Categorize Box According to Criteria

> :green_circle:

给一个盒子的长宽高三个维度和质量，返回盒子的类别。盒子是"Bulky"：如果任一维度大于等于`10^4`，或者体积大于等于`10^9`。质量大于等于`100`盒子是"Heavy"。都是返回"Both"，都不是返回"Neither"。`1 <= length, width, height <= 10^5; 1 <= mass <= 10^3`

### 方法

- 根据条件判断即可，注意计算体积可能溢出。不能三连乘，或者直接转换再乘

```cpp
class Solution {
public:
    string categorizeBox(int length, int width, int height, int mass) {
        bool b = false, h = false;
        if (length >= 1e4 || width >= 1e4 || height >= 1e4) b = true;
        else {
            long v = length * width; // long v = (long)length * (long)width * (long)height;
            v *= height;
            if (v >= 1e9) b = true;
        }
        if (mass >= 100) h = true;
        if (b && h) return "Both";
        else if (b & !h) return "Bulky";
        else if (!b && h) return "Heavy";
        else return "Neither";
    }
};
```

### 拓展

- C++: int, long, long long 

```cpp
int : 4 bytes, 32 bits, [INT_MIN, INT_MAX], [-2^31, 2^31-1], [-2147483648, 2147483647], [-2e9, 2e9];
long long : 8 bytes, 64 bits, [LLong_MIN, LLong_MAX], [-2^63, 2^63-1], [-9223372036854775808, 9223372036854775807], [-9e18, 9e18];
long:  = long long, [Long_MIN, Long_MAX] = [LLong_MIN, LLong_MAX]
```

## 2526. Find Consecutive Integers from a Data Stream

> :orange_circle:

整数数据流，检查是否最后k个数等于`value`。实现`DataStream`类，`DataStream(int value, int k)`：初始化类，`boolean consec(int num)`：数据流中添加num，如果最后k个数等于value返回true，否则返回false，数据不足k个时，返回false

### 方法

- 记录k和value，记录数据流中最新的等于value的数的数量
- 注意私有变量不能再命名为value和k，没法初始化

```cpp
class DataStream {
public:
    DataStream(int value, int k) {
        v = value;
        k_val = k;
        cnt = 0;
    }
    
    bool consec(int num) {
        if (num == v)  cnt++;
        else cnt = 0;
        return cnt >= k_val;
    }
private:
    int v, k_val, cnt;
};
```

## 2527. Find Xor-Beauty of Array

> :orange_circle:

给一个整数数组，**有效值**是`((nums[i] | nums[j]) & nums[k])`，返回数组中所有三元组的**有效值**的异或结果

### 方法

- 暴力遍历所有三元组，计算有效值及异或结果，TLE
- 进行公式简化

```cpp
class Solution {
public:
    int xorBeauty(vector<int>& nums) {
        int ans = 0;
        for (int& n : nums) ans ^= n;
        return ans;
    }
};
```

## 2528. Maximize the Minimum Powered City

> :red_circle: