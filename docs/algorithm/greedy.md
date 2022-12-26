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

