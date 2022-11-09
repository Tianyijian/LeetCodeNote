# 图

## 介绍

节点和边构成，多叉树延伸，邻接表和邻接矩阵存储，有向，无向（视为双向），有环无环，入度和出度，加权图

邻接表占用空间少，但无法快速判断节点是否相邻

DFS遍历：onPath数组记录当前路径上的节点，有环图需要visited数组辅助，防止递归重复遍历同一节点而死循环

BFS遍历：利用队列遍历，indegree数组记录节点的入度，可实现环检测以及拓扑排序

拓扑排序（Topological Sorting）：把一幅图拉平，所有箭头方向一致。有向无环图一定可以拓扑排序，有环图不可以

## 0797. All Paths From Source to Target

> :orange_circle:

有向无环图，输入邻接表，输出所有路径

### 方法

DFS遍历图即可

```cpp
class Solution {
public:
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        path.clear();
        res.clear();
        traversal(graph, 0);
        return res;
    }
private:
    vector<vector<int>> res;
    vector<int> path;
    void traversal(vector<vector<int>>& graph, int node) {
        if (node == graph.size() - 1) {
            path.push_back(node);
            res.push_back(path);
            path.pop_back();
            return;
        }
        path.push_back(node);
        for (int v : graph[node]) {
            traversal(graph, v);
        }
        path.pop_back();
    }
};
```

## 0207. Course Schedule

> :orange_circle:

给定课程数，以及先决条件，如[0,1]表示必须先学课程1，才能学习课程0，判断能否学完全部课程

### 方法一

- 有向图是否有环问题：课程间存在循环依赖，即有环，这些课程都不能学完

- DFS遍历利用onPath数组判断是否有环
- 将题目的输入转换为邻接表，[0, 1] => 1 -> 0
- visited数组记录历史遍历过的节点，遍历到已经访问过的节点时不再访问，减少重复访问
- onPath数组记录当前遍历路径，进入节点为true，离开为false，如果已经被标记，说明有环
- 图中并不是每个节点都相连，需要每个节点都调用一次DFS

```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> graph(numCourses);
        for (auto course : prerequisites) {
            graph[course[1]].push_back(course[0]);
        }
        visited = vector<bool>(numCourses, false);
        onPath = vector<bool>(numCourses, false);
        for (int i = 0; i < graph.size(); i++) {	//遍历所有节点
            traversal(graph, i);
        }
        return !hasCycle;
    }
private:
    vector<bool> visited;
    vector<bool> onPath;
    bool hasCycle = false;
    void traversal(vector<vector<int>>& graph, int node) {
        if (onPath[node])  hasCycle = true;		//出现环
        if (visited[node] || hasCycle) return;	//遍历过或有环时，不再遍历
        visited[node] = true;
        onPath[node] = true;
        for (int v : graph[node]) {
            traversal(graph, v);
        }
        onPath[node] = false;
    }
};
```

### 方法二

- BFS方法判断是否有环，利用indegree数组记录每个节点的入度
- 使用队列进行BFS，先将入度为0的节点装入队列，然后开始循环
  - 不断弹出节点，并将相邻节点的入度减一
  - 将入度为0的节点加入队列
- 如果最终所有节点都被遍历过（即进过队列），则不存在环

```cpp
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> graph(numCourses);
        vector<int> indegree(numCourses, 0);
        for (auto course : prerequisites) {
            graph[course[1]].push_back(course[0]);
            indegree[course[0]]++;
        }
        queue<int> que;
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) que.push(i);
        }
        int count = 0;	//记录进过队列的节点个数
        while (!que.empty()) {
            int node = que.front();
            que.pop();
            count++;
            for (int v : graph[node]) {
                indegree[v]--;
                if (indegree[v] ==  0) que.push(v);
            }
        }
        return count == numCourses;
    }
};
```



## 0210. Course Schedule II

> :orange_circle:

给定课程数以及先决条件，判断是否能学完所有课程，并输出有效的学习顺序

### 方法一

- DFS环检测算法，后续遍历的结果进行翻转即为拓扑排序
- 后续遍历是因为一个任务必须等到它依赖的所有任务都完成之后才能开始执行

```cpp
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> graph(numCourses);
        for (auto course : prerequisites) {
            graph[course[1]].push_back(course[0]);
        }
        visited = vector<bool>(numCourses, false);
        onPath = vector<bool>(numCourses, false);
        for (int i = 0; i < graph.size(); i++) {	//遍历所有节点
            traversal(graph, i);
        }
        if (hasCycle) return {};        //有环输出空节点
        reverse(postOrder.begin(), postOrder.end());    //后序遍历逆序为拓扑排序
        return postOrder;
    }

private:
    vector<int> postOrder;
    vector<bool> visited;
    vector<bool> onPath;
    bool hasCycle = false;
    void traversal(vector<vector<int>>& graph, int node) {
        if (onPath[node])  hasCycle = true;		//出现环
        if (visited[node] || hasCycle) return;	//遍历过或有环时，不再遍历
        visited[node] = true;
        onPath[node] = true;
        for (int v : graph[node]) {
            traversal(graph, v);
        }
        postOrder.push_back(node);
        onPath[node] = false;
    }
};
```

### 方法二

- BFS的环检测算法中，队列的节点遍历顺序即为拓扑排序

```cpp
class Solution {
public:
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> graph(numCourses);
        vector<int> indegree(numCourses, 0);
        for (auto course : prerequisites) {
            graph[course[1]].push_back(course[0]);
            indegree[course[0]]++;
        }
        queue<int> que;
        for (int i = 0; i < numCourses; i++) {
            if (indegree[i] == 0) que.push(i);
        }
        int count = 0;
        vector<int> res(numCourses, 0);
        while (!que.empty()) {
            int node = que.front();
            que.pop();
            res[count] = node;
            count++;
            for (int v : graph[node]) {
                indegree[v]--;
                if (indegree[v] ==  0) que.push(v);
            }
        }
        if (count != numCourses) return {};
        return res; 
    }
};
```

