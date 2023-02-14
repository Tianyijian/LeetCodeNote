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

## 0045. Jump Game II

> :orange_circle:

给一个整数数组，数组的每个元素代表你在那个位置能跳跃的最大距离。初始时位于数组第一个位置，一定能跳到数组最后一个位置。返回所需的最小步数

### 方法

- 贪心算法，关注新增加的跳跃范围，而不在已覆盖的跳跃范围中增加跳跃次数。T: O(n), S: O(1)
- 当前能跳跃的范围是[curBegin, curEnd]，下一次能跳跃的最远位置是curFar。不断遍历更新curFar，当遍历达到curEnd时，增加跳跃次数，更新curEnd为curFar。不用显式指定跳跃位置。

```cpp
class Solution {
public:
    int jump(vector<int>& nums) {
        int step = 0, n = nums.size();
        int curEnd = 0, curFar = 0;
        for (int i = 0; i < n - 1; i++) {
            curFar = max(curFar, i + nums[i]);
            if (i == curEnd) {
                step++;
                curEnd = curFar;
            }
        }
        return step;
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

