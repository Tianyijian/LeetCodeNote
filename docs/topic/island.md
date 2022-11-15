# 岛屿问题

## 介绍

- 核心在于DFS/BFS遍历二维数组
- 可定义方向数组，方便遍历上下左右
- 学习资源：
  - https://labuladong.gitee.io/algo/4/31/107/


## 0200. Number of Islands

> :orange_circle:

给定`mxn`二维网格，`0`代表海水，`1`代表陆地，计算岛屿的数量。岛屿四周由海水包围，且假设网格的四边都由海水包围

### 方法

- 每次遇到一个没有访问过的岛屿，岛屿数+1，DFS将该岛屿四周相连的岛屿全部访问
- 写法一：使用visited数组记录岛屿的访问情况
- 写法二：直接将四周相连的岛屿淹没为海水，DFS遇到海水时直接返回

<!-- tabs:start -->
####**First**

```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        visited = vector<vector<bool>>(m, vector<bool>(n, false));
        int res = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1' && !visited[i][j]) {
                    res += 1;
                    dfs(grid, i, j);
                }
            }
        }
        return res;
    }
private:
    vector<vector<bool>> visited;
    void dfs(vector<vector<char>>& grid, int row, int col) {
        int m = grid.size(), n = grid[0].size();
        if (row < 0 || col < 0 || row >= m || col >= n) return;
        if (visited[row][col]) return;
        if (grid[row][col] == '0') return; 
        visited[row][col] = true;
        dfs(grid, row - 1, col);
        dfs(grid, row + 1, col);
        dfs(grid, row, col - 1);
        dfs(grid, row, col + 1);
    }
};
```

####**Second**

```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int res = 0;
        for (int i = 0; i < grid.size(); i++) {
            for (int j = 0; j < grid[0].size(); j++) {
                if (grid[i][j] == '1') {
                    res += 1;
                    dfs(grid, i, j);
                }
            }
        }
        return res;
    }
private:
    void dfs(vector<vector<char>>& grid, int row, int col) {
        int m = grid.size(), n = grid[0].size();
        if (row < 0 || col < 0 || row >= m || col >= n) return;
        if (grid[row][col] == '0') return; 
        grid[row][col] = '0';
        dfs(grid, row - 1, col);
        dfs(grid, row + 1, col);
        dfs(grid, row, col - 1);
        dfs(grid, row, col + 1);
    }
};
```

<!-- tabs:end -->

## 1254. Number of Closed Islands

> :orange_circle:

给定二维网格，`0`代表陆地，`1`代表海水，返回四周都被海水包围的封闭岛屿的数量

### 方法一

- 对于一块没有访问过的陆地，DFS向四周探寻，四周都返回海水时，该陆地为封闭岛屿

```cpp
class Solution {
public:
    int closedIsland(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        visited = vector<vector<bool>>(m, vector<bool>(n, false));
        int res = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0 && !visited[i][j]) {
                    if (dfs(grid, i, j)) res++;
                }
            }
        }
        return res;
    }
private:
    vector<vector<bool>> visited;
    vector<vector<int>> direction = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    bool dfs(vector<vector<int>>& grid, int i, int j) {
        int m = grid.size(), n = grid[0].size();
        if (i < 0 || j < 0 || i >= m || j >= n) return false;
        if (grid[i][j] == 1) return true;
        if (visited[i][j]) return true;
        visited[i][j] = true;
        bool res = true;
        for (auto d : direction) {
            res = res & dfs(grid, i + d[0], j + d[1]);
        }
        return res;
    }
};
```

### 方法二

- 先淹没四边上的岛屿，则剩下的岛屿就是封闭岛屿。DFS直接将四周相连的岛屿淹没为海水

```cpp
class Solution {
public:
    int closedIsland(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        for (int i = 0; i < m; i++) {
            dfs(grid, i, 0);
            dfs(grid, i, n - 1);
        }
        for (int j = 0; j < n; j++) {
            dfs(grid, 0, j);
            dfs(grid, m - 1, j);
        }
        int res = 0;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 0) {
                    res += 1;
                    dfs(grid, i, j);
                }
            }
        }
        return res;
    }
private:
    void dfs(vector<vector<int>>& grid, int row, int col) {
        int m = grid.size(), n = grid[0].size();
        if (row < 0 || col < 0 || row >= m || col >= n) return;
        if (grid[row][col] == 1) return; 
        grid[row][col] = 1;
        dfs(grid, row - 1, col);
        dfs(grid, row + 1, col);
        dfs(grid, row, col - 1);
        dfs(grid, row, col + 1);
    }
};
```

