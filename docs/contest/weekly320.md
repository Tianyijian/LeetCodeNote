# Weekly Contest 320
> 2022.11.20

## 2475. Number of Unequal Triplets in Array

> :green_circle:

给一个正整数数组`nums`，从中找出三元组，满足`0 <= i < j < k < nums.length`，`nums[i] != nums[j]`, `nums[i] != nums[k]`,  `nums[j] != nums[k]`。返回满足条件的三元组数量。`3 <= nums.length <= 100`，`1 <= nums[i] <= 1000`

### 方法一

- 暴力搜索。T: O(n^3), S: O(1)

```cpp
class Solution {
public:
    int unequalTriplets(vector<int>& nums) {
        int n = nums.size();
        int res = 0;
        for (int i = 0; i < n - 2; i++) {
            for (int j = i + 1; j < n - 1; j++) {
                if (nums[i] == nums[j]) continue;
                for (int k = j + 1; k < n; k++) {
                    if (nums[j] != nums[k] && nums[i] != nums[k]) {
                        res++;
                    }
                }
            }
        }
        return res;
    }
};
```

### 方法二

- 哈希表`m`统计不同元素的个数，则元素a,b,c能形成的三元组个数为`m[a] * m[b] * m[c]`
- 设有`a~z`26个元素，则`n`在中间时，形成的三元组个数为`sum(m[a]...m[m]) * m[n] * sum(m[o]...m[z])`
- 因此可以在遍历map的过程中，得到`m[n]`，并依次追踪left，right的和。T: O(n), S: O(n)

```cpp
class Solution {
public:
    int unequalTriplets(vector<int>& nums) {
        unordered_map<int, int> map;
        for (int n : nums) map[n]++;
        int left = 0, right = nums.size();
        int res = 0;
        for (auto [n, cnt] : map) {
            right -= cnt;
            res += left * cnt * right;
            left += cnt;
        }
        return res;
    }
};
```

### 方法三

- 数学组合数，从n个数中任选三个数，共有 $C_n^3$ = n(n-1)(n-2) / 6种选法，移除两种情况：三个数都一样，以及两个数一样
- 哈希表m统计不同元素的个数，对个数为x的某元素来说，三个都一样：$C_x^3$，两个一样：(n - x) $C_x^2$。T: O(n), S: O(n)

```cpp
class Solution {
public:
    int unequalTriplets(vector<int>& nums) {
        unordered_map<int, int> map;
        for (int num : nums) map[num]++;
        int n = nums.size();
        int res = n * (n - 1) * (n - 2) / 6;
        for (auto [num, cnt] : map) {
            res -= cnt * (cnt - 1) * (cnt - 2) / 6;
            res -= (n - cnt) * cnt * (cnt - 1) / 2;
        }
        return res;
    }
};
```

## 2476. Closest Nodes Queries in a Binary Search Tree

> :orange_circle:

给一个二叉搜索树，以及数组queries。对queries中的每个数，找到左右两侧最相邻的数。如果一侧最相邻的数与其相等，返回该数，如果一侧的数不存在，返回-1

### 方法

- BST的中序遍历为升序数组，先通过遍历将节点值保存到数组中
- 在一维升序数组中，查找某个数左右相邻的两个数，可通过二分查找完成（可利用内置函数lower_bound）
- 也可以不保存到数组，对每个query使用BST的迭代式遍历。但BST不平衡时代价为O(nm)，n为queries个数，m为BST节点数
- BST搜索代价为O(h)，平衡时为O(logm)，不平衡时为O(m)，因此先保存为数组更合适。T: O(nlogm), S: O(m)

```cpp
class Solution {
public:
    vector<vector<int>> closestNodes(TreeNode* root, vector<int>& queries) {
        traversal(root);
        vector<vector<int>> res;
        for (int i = 0; i < queries.size(); i++) {
            res.push_back(search(vals, queries[i], 0, vals.size() - 1));
        }
        return res;
    }
    
private:
    vector<int> vals;
    void traversal(TreeNode* node) {
        if (node == NULL) return;
        traversal(node->left);
        vals.push_back(node->val);
        traversal(node->right);
    }
    
    vector<int> search(vector<int>& vals, int target, int left, int right) {
        if (vals[left] > target) return {-1, vals[left]};
        if (vals[right] < target) return {vals[right], -1};
        while (left <= right) {
            int mid = left + (right - left) / 2; 
            if (vals[mid] < target) {
                left = mid + 1;
            } else if (vals[mid] > target) {
                right = mid - 1;
            } else return {target, target};
        }
        return {vals[right], vals[left]};
    }
};
```

## 2477. Minimum Fuel Cost to Report to the Capital

> :orange_circle:

由n个城市节点构成树结构网络（无向无环图），每个城市都有一名代表，要在首都节点0开会。每个城市都有一辆固定座位数的车，代表们可共同乘坐，车在两个城市间行驶需要1升汽油。求最少需要的汽油数量

### 方法

- DFS，后序遍历，得到到达每个节点的人数，然后根据车载量分配汽车，到达下一节点重新计算即可
- 每个节点有num个人，每辆车座位为seats，则车的个数为`(num + seats - 1) / seats`
- 因为无环，且是双向边，用visited数组防止回头，也可以在dfs参数中记录上个节点防止回头

```cpp
class Solution {
public:
    long long minimumFuelCost(vector<vector<int>>& roads, int seats) {
        int n = roads.size() + 1;
        vector<vector<int>> graph(n);
        for (int i = 0; i < roads.size(); i++) {
            graph[roads[i][0]].push_back(roads[i][1]);
            graph[roads[i][1]].push_back(roads[i][0]);
        }
        visited = vector<bool>(n, false);
        dfs(graph, 0, seats);
        return res;
    }

private:
    long long res; 
    vector<bool> visited;
    int dfs(vector<vector<int>>& graph, int node, int seats) {
        if (visited[node]) return 0;
        visited[node] = true;
        int num = 1;
        for (int v : graph[node]) {
            num += dfs(graph, v, seats);
        }
        if (node != 0) res += (num + seats - 1) / seats;
        return num;
    }
};
```

## 2478. Number of Beautiful Partitions

> :red_circle:
