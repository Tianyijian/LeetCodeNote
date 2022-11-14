# 并查集

## 介绍

* 树形数据结构，处理不相交集合的合并及查询问题
* 通常有两个操作：查询（查询两个元素是否在同一个集合中），合并（把两个不相交的集合合并为一个集合）
* 初始化：每个节点是其本身
* 查找：递归查找父节点，直至根节点（根节点标志就是根节点本身）
* 合并：将一个节点的根节点设为另一个节点的根节点
* 判断两个元素是否属于同一集合，看他们的根节点是否相同即可
* 路径压缩：提高并查集的效率，直接把每个元素的父节点改为集合的根节点，方法为查找过程中，把沿途的每个父节点设为根节点
* 按秩合并：把深度小的树合并到深度大的树中
* 学习资源
  * https://labuladong.gitee.io/algo/2/22/53/

## 0684. Redundant Connection

> :orange_circle:

树是连通的无向无环图，给定往一棵 n 个节点 (节点值 1～n) 的树中添加一条边后的图，删去一条边使其为树，多个答案时返回最后添加的边

### 方法

- 并查集，从前向后遍历边，边的节点不在同一集合，加入集合；已经在同一集合，再加入边就出现环了，返回该边即可

```cpp
class Solution {
public:
    vector<int> findRedundantConnection(vector<vector<int>>& edges) {
        init();
        for (int i = 0; i < edges.size(); i++) {
            if (same(edges[i][0], edges[i][1])) return edges[i];
            else join(edges[i][0], edges[i][1]);
        }
        return {};
    }
private:
    int n = 1005;
    int father[1005];
    
    void init() {
        for (int i = 0; i < n; i++) father[i] = i;
    }
    
    int find(int u) {
        return u == father[u] ? u : (father[u] = find(father[u]));
    }
    
    void join(int u, int v) {
        father[find(v)] = find(u);
    }
    
    bool same(int u, int v) {
        return find(u) == find(v);
    }
};
```

## 0685. Redundant Connection II

> :red_circle:

有根树是有向图：只有一个根节点，其没有父节点，其它节点都是根节点的后继，且有且只有一个父节点。

给定往一颗n个节点的有根树中添加一条边后的图，删去一条边使其为有根树，多个答案时返回最后添加的边

### 方法

- 有根树添加一条边后，有三种情况：前两种情况出现入度为2的边，第三种出现有向环
- 前两种情况：计算节点的入度，从后往前找到入度为2的边，如果删除之后图成了树，则该边就是答案
- 第三种情况：图中出现了有向环，找到构成环的边就是要删除的边
- 并查集：可以判断一个图是不是树，待添加边的两个点在并查集中有相同的根，则该边添加之后图一定不是树
- https://programmercarl.com/0685.%E5%86%97%E4%BD%99%E8%BF%9E%E6%8E%A5II.html

```cpp
class Solution {
private:
    static const int N = 1010; // 如题：二维数组大小的在3到1000范围内
    int father[N];
    int n; // 边的数量
    // 并查集初始化
    void init() {
        for (int i = 1; i <= n; ++i) {
            father[i] = i;
        }
    }
    // 并查集里寻根的过程
    int find(int u) {
        return u == father[u] ? u : father[u] = find(father[u]);
    }
    // 将v->u 这条边加入并查集
    void join(int u, int v) {
        u = find(u);
        v = find(v);
        if (u == v) return ;
        father[v] = u;
    }
    // 判断 u 和 v是否找到同一个根
    bool same(int u, int v) {
        u = find(u);
        v = find(v);
        return u == v;
    }
    // 在有向图里找到删除的那条边，使其变成树
    vector<int> getRemoveEdge(const vector<vector<int>>& edges) {
        init(); // 初始化并查集
        for (int i = 0; i < n; i++) { // 遍历所有的边
            if (same(edges[i][0], edges[i][1])) { // 构成有向环了，就是要删除的边
                return edges[i];
            }
            join(edges[i][0], edges[i][1]);
        }
        return {};
    }
    // 删一条边之后判断是不是树
    bool isTreeAfterRemoveEdge(const vector<vector<int>>& edges, int deleteEdge) {
        init(); // 初始化并查集
        for (int i = 0; i < n; i++) {
            if (i == deleteEdge) continue;
            if (same(edges[i][0], edges[i][1])) { // 构成有向环了，一定不是树
                return false;
            }
            join(edges[i][0], edges[i][1]);
        }
        return true;
    }

public:
    vector<int> findRedundantDirectedConnection(vector<vector<int>>& edges) {
        int inDegree[N] = {0}; // 记录节点入度
        n = edges.size(); // 边的数量
        for (int i = 0; i < n; i++) {
            inDegree[edges[i][1]]++; // 统计入度
        }
        vector<int> vec; // 记录入度为2的边（如果有的话就两条边）
        // 找入度为2的节点所对应的边，注意要倒序，因为优先返回最后出现在二维数组中的答案
        for (int i = n - 1; i >= 0; i--) {
            if (inDegree[edges[i][1]] == 2) {
                vec.push_back(i);
            }
        }
        // 处理图中情况1 和 情况2
        // 如果有入度为2的节点，那么一定是两条边里删一个，看删哪个可以构成树
        if (vec.size() > 0) {
            if (isTreeAfterRemoveEdge(edges, vec[0])) {
                return edges[vec[0]];
            } else {
                return edges[vec[1]];
            }
        }
        // 处理图中情况3
        // 明确没有入度为2的情况，那么一定有有向环，找到构成环的边返回就可以了
        return getRemoveEdge(edges);
    }
};
```

## 0130. Surrounded Regions

> :orange_circle:

给`m x n`的矩阵，包含`X`和`O`，将四周都被`X`包围的`O`翻转为`X`。边界上的`O`以及与边界`O`相连的`O`不会被翻转

### 方法

- 二维坐标转换为一维坐标： `(x,y)` 可以转换成 `x * n + y` （`m` 是行数，`n` 是列数），对应`[0, m*n-1]`
- 并查集：不需被替换的`O`设置共同的根节点dummy，可以占据索引`m*n`
- 将四边的O与dummy连通，利用方向数组将与边界相相连的`O`连通，剩余的`O`替换掉

## 0947. Most Stones Removed with Same Row or Column

> :orange_circle:

在2维平面，根据给定的坐标放置n个石头，一个石头与其它石头同行或同列，可以将该石头移除，返回可以移除的最大数量的石头

### 方法一

- 并查集，将同行或同列的石头连接到一起，每个连通分量剩余一个石头，最大移除数=总石头数-连通分量数。
- T: O(n^2logn), S: O(n)

```cpp
class Solution {
public:
    int removeStones(vector<vector<int>>& stones) {
        int n = stones.size();
        UF* uf = new UF(n);
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if (stones[i][0] == stones[j][0] || stones[i][1] == stones[j][1]) {
                    uf->join(i, j);
                }
            }
        }
        return n - uf->countNum();
    }

    class UF {
    private:
        int count;
        int *father;
    public:
        UF(int n) {
            count = n;
            father = new int[n];
            for (int i = 0; i < n; i++) father[i] = i;
        }
        void join(int u, int v) {
            u = find(u);
            v = find(v);
            if (u == v) return;
            father[v] = u;
            count--;
        }

        int find(int u) {
            return father[u] == u ? u : (father[u] = find(father[u])); 
        }

        int countNum() {
            return count;
        }
    };
};
```
### 方法二

- 并查集，以空间换时间，横纵坐标单独设为点，加入到并查集中，最后按石头统计连通分量个数
- 因为横纵坐标取值范围为[0, 10000]，所以可以加上10001区分横纵坐标。T: O(nlogn), S: O(n)

```cpp
class Solution {
public:
    vector<int> p;

    int removeStones(vector<vector<int>>& stones) {
        int n = 10001;
        p.resize(n << 1);
        for (int i = 0; i < p.size(); ++i) p[i] = i;
        for (auto& stone : stones) p[find(stone[0])] = find(stone[1] + n);
        unordered_set<int> s;
        for (auto& stone : stones) s.insert(find(stone[0]));
        return stones.size() - s.size();
    }

    int find(int x) {
        if (p[x] != x) p[x] = find(p[x]);
        return p[x];
    }
};
```

