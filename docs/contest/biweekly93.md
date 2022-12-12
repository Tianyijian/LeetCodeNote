# Biweekly Contest 93
> 2022.12.10

## 2496. Maximum Value of a String in an Array
> :green_circle:

含有小写字母和数字的字符串的值定义为：全为数字，则值为数；否则值为字符串的长度。给一个字符串数组，返回最大的值

### 方法

- 遍历每个字符串，若含有小写字母，返回字符串长度，否则转换为数字。记录最大值即可。T: O(n), S: O(1)

```cpp
class Solution {
public:
    int maximumValue(vector<string>& strs) {
        int ans = 0;
        for (string s : strs) {
            ans = max(ans, value(s));
        }
        return ans;
    }
private:
    int value(string s) {
        for (char c : s) {
            if (c >= 'a' && c <= 'z') return s.size();
        }
        return stoi(s);
    }
};
```

## 2497. Maximum Star Sum of a Graph

> :orange_circle:

n个节点的无向图，每个节点都有值。星图包含一个中心节点及0个或多个邻居，其和为所有节点的值的和。给整数k，返回最大的星图和，每个星图最多k条边。`-10^4 <= vals[i] <= 10^4; 0 <= k <= n - 1`

### 方法

- 每个节点作为星图的中心节点，找到其相邻的k个值最大（正数）的节点即可计算星图和，记录最大星图和。
- 只有正值的节点对星图和有贡献，因此构图时可以只添加正值的相邻节点。
- 当每个节点的相邻正值节点数小于等于k时，全都加入星图和。大于k时，只需要K个最大的，可采用小顶堆维护。

```cpp
class Solution {
public:
    int maxStarSum(vector<int>& vals, vector<vector<int>>& edges, int k) {
        int n = vals.size();
        vector<vector<int>> graph(n);
        for (auto e : edges) {
            if (vals[e[1]] > 0) graph[e[0]].push_back(e[1]);
            if (vals[e[0]] > 0) graph[e[1]].push_back(e[0]);
        }
        int ans = INT_MIN;
        for (int i = 0; i < n; i++) {
            int starSum = vals[i];
            if (graph[i].size() <= k) {
                for (int v : graph[i]) starSum += vals[v];
            } else {
                priority_queue<int, vector<int>, greater<int>> minHeap;
                for (int v : graph[i]) {
                    minHeap.push(vals[v]);
                    if (minHeap.size() > k) minHeap.pop();
                }
                while (!minHeap.empty()) {
                    starSum += minHeap.top();
                    minHeap.pop();
                }
            }
            ans = max(ans, starSum);
        }
        return ans;
    }
};
```

## 2498. Frog Jump II

> :orange_circle:

给一个整数数组stones，代表河中石头的位置，严格递增。青蛙从第一个石头跳到最后一个石头，并返回第一个石头，每个石头只能跳一次。每次跳跃的长度是两块石头的绝对位置差，一个来回的代价是最长的跳跃长度。找到青蛙跳跃一个来回的最小代价。

### 方法

- 贪心思想，可以视为两个青蛙从起始跳到末尾，不能选择同一块石头。则必定两个青蛙间隔跳，跳跃长度最小。T: O(n), S: O(1)

```cpp
class Solution {
public:
    int maxJump(vector<int>& stones) {
        int ans = stones[1];
        for (int i = 2; i < stones.size(); i++) {
            ans = max(ans, stones[i] - stones[i - 2]);
        }
        return ans;
    }
};
```

## 2499. Minimum Total Cost to Make Arrays Unequal

> :red_circle:

给两个长度为n且下标从0开始的整数数组 `nums1` 和 `nums2`。每次操作交换`nums1`的两个值，操作代价为两个值的下标和。经过任意次操作后，使得`0 <= i <= n - 1`都有`nums1[i] != nums2[i]`。找到最小的操作代价总和，如果不可能返回-1