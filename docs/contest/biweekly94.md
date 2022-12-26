# Biweekly Contest 94
> 2022.12.24

## 2511. Maximum Enemy Forts That Can Be Captured

> :green_circle:

长度为n的整数数组代表堡垒的位置，-1空堡垒，0是敌人堡垒，1是我方堡垒。从我方堡垒移动军队到达空堡垒，途中只能路过敌人堡垒，路过的敌人堡垒被攻占。返回可以被攻占的最大敌人堡垒数量

### 方法

- 暴力：遍历我方堡垒，向两侧寻找敌人堡垒，直至遇到空堡垒，记录最大的堡垒数量
- 一遍遍历：计算1和-1之间的0的数量。T: O(n), S: O(1)

```cpp
class Solution {
public:
    int captureForts(vector<int>& forts) {
        int ans = 0, j = 0;
        for (int i = 0; i < forts.size(); i++) {
            if (forts[i] != 0) {
                if (forts[i] == -forts[j]) {
                    ans = max(ans, i - j - 1);
                }
                j = i;
            }
        }
        return ans;
    }
};
```

## 2512. Reward Top K Students

> :orange_circle:

### 方法

## 2513. Minimize the Maximum of Two Arrays

> :orange_circle:

### 方法

## 2514. Count Anagrams

> :red_circle:

### 方法
