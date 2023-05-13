# Biweekly Contest 91
> 2022.11.12

## 2465. Number of Distinct Averages

> :green_circle:

给定整数数组，长度为偶数，每次移除最大值和最小值，并记录两者的平均值，重复移除直至数组为空，返回独特的平均数个数

### 方法

- 数组排序后，左右指针依次向中间移动，使用集合记录两者的平均值，最后返回集合大小。T: O(nlogn), S: O(n)
- 注意平均值为double，要除以2.0。简便写法：直接添加两者和即可，不用计算平均值

```cpp
class Solution {
public:
    int distinctAverages(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int left = 0, right = nums.size() - 1;
        unordered_set<double> set; // set<int> set;
        while (left < right) {
            double num = (nums[left] + nums[right]) / 2.0; // set.insert(nums[left] + nums[right]);
            set.insert(num);
            left++;
            right--;
        }
        return set.size();
    }
};
```

## 2466. Count Ways To Build Good Strings

> :orange_circle:

从空串开始构建字符串，每次可以选择添加zero个'0'，或者one个'1'，最终长度在[low, high]之间的字符串为好串，返回不同好串的个数。由于数据很大，结果对10^9 + 7取余

### 方法

- 动态规划，dp[i]代表构建长度为i的子串的个数，其有两个来源，dp[i] = dp[i-zero] + dp[i-one]，初始化dp[0]=1
- 多个数据相加之后取余==每个数据取余之后再加和，因此计算过程中的每个结果都取余，防止数值溢出
- 注意`10^9+7`写法为`1e9+7`，`INT_MAX = 2^31-1 = 2147483647 > 2x1e9`

```cpp
class Solution {
public:
    int countGoodStrings(int low, int high, int zero, int one) {
        vector<int> dp(high + 1);
        dp[0] = 1;
        int res = 0, mod = 1e9 + 7;
        for (int i = 1; i <= high; ++i) {
            if (i >= zero) dp[i] = (dp[i] + dp[i - zero]) % mod;
            if (i >= one) dp[i] = (dp[i] + dp[i - one]) % mod;
            if (i >= low) res = (res + dp[i]) % mod;
        }
        return res;
    }
};
```

## 2467. Most Profitable Path in a Tree

> :orange_circle:

有n个节点的无向树，每个节点都有奖励。Alice从根节点0出发到达某一叶子节点，Bob从bob节点出发到达根节点0，两人同时移动，到达某个节点时，获得该节点奖励（奖励变为0）。两者同时到达某个节点，则平分奖励。求Alice能获得的最大奖励

### 方法

- Bob的路径是固定的，先DFS找出Bob的路径。然后DFS模拟Alice前进的路径，每走一步Bob也同时移动一步
- 同时到达某个节点奖励平分，更新得到的奖励，将到达过的节点奖励设为0。Alice达到叶结点时，更新最大的奖励

```cpp
class Solution {
public:
    int mostProfitablePath(vector<vector<int>>& edges, int bob, vector<int>& amount) {
        int n = amount.size();
        vector<vector<int>> graph(n);
        for (auto edge : edges) {
            graph[edge[0]].push_back(edge[1]);
            graph[edge[1]].push_back(edge[0]);
        }
        // DFS: find the fixed path to node 0 of Bob
        visited = vector<bool>(n, false);
        dfsBob(graph, bob, 0);
        // DFS: find the max income of Alice
        visited = vector<bool>(n, false);
        dfsAlice(graph, 0, 0, amount);
        return maxIncome;
        
    }
private:
    vector<int> bobPath;
    vector<bool> visited;
    bool dfsBob(vector<vector<int>>& graph, int cur, int target) {
        if (cur == target) {
            bobPath.push_back(cur);
            return true;
        }
        visited[cur] = true;
        bobPath.push_back(cur);
        for (int v : graph[cur]) {
            if (!visited[v] && dfsBob(graph, v, target)) return true;
        }
        visited[cur] = false;
        bobPath.pop_back();
        return false;
    }
    
    vector<int> res;
    int maxIncome = INT_MIN;
    int pathIncome = 0;
    void dfsAlice(vector<vector<int>>& graph, int cur, int step, vector<int>& amount) {
        // Detemine if cur is a leaf
        bool leaf = true;
        for (int i = 0; i < graph[cur].size(); i++) {
            if (!visited[graph[cur][i]]) leaf = false;
        }
        if (leaf) {
            pathIncome += amount[cur];
            maxIncome = max(pathIncome, maxIncome);
            pathIncome -= amount[cur];
            return;
        }
        // Update income and amount by location of Alice and Bob
        visited[cur] = true;
        int curIncome = amount[cur];
        int curOldAmount = amount[cur];
        int bob, bobOldAmount;
        if (step < bobPath.size()) {
            bob = bobPath[step];
            if (bob == cur) curIncome /= 2;
            bobOldAmount = amount[bob];
            amount[bob] = 0;
        }
        pathIncome += curIncome;
        amount[cur] = 0;
        // Move to next step
        for (int v : graph[cur]) {
            if (!visited[v]) dfsAlice(graph, v, step + 1, amount);
        }
        visited[cur] = false;
        pathIncome -= curIncome;
        amount[cur] = curOldAmount;
        if (step < bobPath.size()) amount[bob] = bobOldAmount;
    }  
};
```

## 2468. Split Message Based on Limit

> :red_circle: