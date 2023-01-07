# 贪心算法

## 0055. Jump Game

> :orange_circle:

给一个整数数组，数组的每个元素代表你在那个位置能跳跃的最大距离。初始时位于数组第一个位置，判断能否跳跃到数组最后一个位置

### 方法

- 贪心思想，记录能够跳跃到的最大位置，超越数组最后一个位置时，即能够跳跃。T: O(n), S: O(1)

```cpp
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int range = 0;
        for (int i = 0; i <= range; i++) {
            range = max(range, nums[i] + i);
            if (range >= nums.size() - 1) return true;
        }
        return false;
    }
};
```

## 0134. Gas Station

> :orange_circle:

n个加油站呈环形，第i个加油站的汽油为`gas[i]`，到达下一个加油站用油`cost[i]`。油箱容量无限，开始油箱为空，从一个加油站出发顺时针转一圈，返回开始加油站的下标，不能则返回-1。若有结果则结果唯一

### 方法

- 贪心思想，计算每个加油站的剩余油量，若总剩余油量为负返回-1。从下标0开始累加剩余油量，若小于0，则[j, i] 之间均不能作为起始点，起始点设为i+1，重新累加剩余油量。T: O(n), S: O(1)

```cpp
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int curSum = 0, sum = 0, ans = 0;
        for (int i = 0; i < gas.size(); i++) {
            int rest = gas[i] - cost[i];
            curSum += rest;
            sum += rest;
            if (curSum < 0) {
                curSum = 0;
                ans = i + 1;
            }
        }
        if (sum < 0) return -1;
        return ans;
    }
};
```

