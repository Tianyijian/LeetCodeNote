# Weekly Contest 318
> 2022.11.06

## 2460. Apply Operations to an Array

> :green_circle:

非负数组，先进行n-1个操作，`nums[i]==nums[i+1]时，nums[i]*2，nums[i+1]=0`，之后将所有的零移动到数组最后

相似题目：0283. Move Zeroes

### 方法

- 第一步的n-1个操作直接遍历数组即可，第二步为移动数组的零元素，要注意保持非负元素的顺序不变
- 可以直接移动非零元素到新数组，或者双指针移动零元素。T: O(n), S: O(1)

```cpp
class Solution {
public:
    vector<int> applyOperations(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n - 1; i++) {
            if (nums[i] == nums[i + 1]) {
                nums[i] *= 2;
                nums[i + 1] = 0;
            }
        }
        int slow = 0;
        for (int i = 0; i < n; i++) {
            if (nums[i] != 0) swap(nums[i], nums[slow++]);
        }
        return nums;
    }
};
```

## 2461. Maximum Sum of Distinct Subarrays With Length K

> :orange_circle:

给一维数组及整数k，找到长度为k的子数组，且元素各不相同，求最大的子数组元素和，没有满足条件的子数组返回0。数组元素取值[1, 100000]

相似题目：0003. Longest Substring Without Repeating Characters
### 方法一

- 滑动窗口，哈希表记录每个字符出现的最后位置+1（start从此处开始，窗口内没有重复元素）
- 根据当前字符移动滑动窗口的起始位置，确保窗口内一定没有重复字符
- 同时维护固定大小为K的窗口，左开右闭区间[start, end)，记录最大元素和。T: O(n), S: O(n)

```cpp
class Solution {
public:
    long long maximumSubarraySum(vector<int>& nums, int k) {
        long long sum = 0, maxSum = 0;
        int start = 0, end = 0;
        int hash[100001] = {0};
        while (end < nums.size()) {
            int num = nums[end];
            end++;
            sum += num;
            while (start < hash[num]) sum -= nums[start++];
            if (end - start == k) {
                maxSum = max(maxSum, sum);
                sum -= nums[start++];
            }
            hash[num] = end;
        }
        return maxSum;
    }
};
```

### 方法二

- 滑动窗口，维护固定大小为K的窗口，左开右闭区间(end-k, end]
- 哈希表记录没有重复元素的起始位置，即每个元素的下一个位置，初始化为0。维护窗户的没有重复元素的起始位置dupStart
- dupStart小于等于窗口的起始位置时（dupStart<=end-k+1），说明没有重复元素，可以记录结果
- 注意end=k-1时，此时窗口元素数为k，窗口区间为(-1, k-1]，dupStart初始化为0没问题，不会遗漏。如例子`[1,2,2] k=2`
- 与方法一的性能一样，思路不同，省略了根据dupStart缩减窗口的过程，代码更简洁。T: O(n), S: O(n)

```cpp
class Solution {
public:
    long long maximumSubarraySum(vector<int>& nums, int k) {
        long long sum = 0, maxSum = 0;
        int dupStart = 0;
        int hash[100001] = {0};
        for (int end = 0; end < nums.size(); end++) {
            sum += nums[end];
            if (end >= k) sum -= nums[end - k];
            dupStart = max(dupStart, hash[nums[end]]);
            if (dupStart <= end - k + 1) maxSum = max(maxSum, sum);
            hash[nums[end]] = end + 1;
        }
        return maxSum;
    }
};
```

## 2462. Total Cost to Hire K Workers

>  :orange_circle:

整数数组`costs`，`costs[i]`代表雇佣第`i`个员工的代价。两个整数 `k` 和 `candidates`，进行 `k` 轮雇佣，每一轮恰好雇佣一位工人。每一轮雇佣中，从最前面 `candidates` 和最后面 `candidates` 人中选出代价最小的一位工人（代价相同时选择下标更小的）。返回总代价

### 方法

- 维护两个小顶堆，双指针记录前后的位置，每次雇佣时先填充堆，然后选择两个堆顶的最小值

```cpp
class Solution {
public:
    long long totalCost(vector<int>& costs, int k, int candidates) {
        priority_queue<int, vector<int>, greater<int>> p1, p2;
        int i = 0, j = costs.size() - 1;
        long long ans = 0;
        while (k--) {
            while (p1.size() < candidates && i <= j) p1.push(costs[i++]);
            while (p2.size() < candidates && j >= i) p2.push(costs[j--]);
            int a = p1.size() ? p1.top() : INT_MAX; 
            int b = p2.size() ? p2.top() : INT_MAX;
            if (a <= b) {
                p1.pop();
                ans += a;
            } else {
                p2.pop();
                ans += b;
            }
        }
        return ans;
    }
};
```

## 2463. Minimum Total Distance Traveled

> :red_circle:
