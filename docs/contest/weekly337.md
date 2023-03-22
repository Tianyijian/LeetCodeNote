# Weekly Contest 337
> 2023.03.19

## 2595. Number of Even and Odd Bits
> :green_circle:

给一个正整数n，其二进制表示中，偶数位置为1的个数为`even`，奇数位置为1的个数为`odd`，返回`[even, odd]`

### 方法

- 通过计算依次得到正整数的二进制位（对2取余得到最低位的值，然后不断除以2），依次统计even和odd的值

```cpp
class Solution {
public:
    vector<int> evenOddBit(int n) {
        vector<int> ans(2, 0);
        int i = 0;
        while (n) {
            if (n % 2 == 1) ans[i % 2]++;
            i++;
            n /= 2;
        }
        return ans;
    }
};
```

## 2596. Check Knight Tour Configuration

> :orange_circle:

nxn的棋盘上有一匹马，在有效的配置中，马从左上角开始，能够跳到棋盘的每个格并且只跳一次。给一个nxn的网格数组，其值为`[0, n*n-1]`，代表马移动的步数。判断该网格是否是有效配置。马走日，有八个选择方向

### 方法

- DFS进行遍历，从八个位置中判断是否有下一步应该跳的位置，有则继续跳，无则返回失败。注意判断起始位置为0。

```cpp
class Solution {
public:
    bool checkValidGrid(vector<vector<int>>& grid) {
        if (grid[0][0] != 0) return false;
        return dfs(grid, 0, 0);
    }
private:
    int dir[9] = {1, 2, -1, -2, 1, -2, -1, 2, 1};
    bool dfs(vector<vector<int>>& grid, int r, int c) {
        int n = grid.size();
        if (grid[r][c] == n * n - 1) return true; 
        for (int i = 0; i < 8; i++) {
            int newR = r + dir[i];
            int newC = c + dir[i + 1];
            if (newR < 0 || newC < 0 || newR >= n || newC >= n) continue;
            if (grid[newR][newC] == grid[r][c] + 1) {
                if (dfs(grid, newR, newC)) return true;
            }
        }
        return false;
    }
};
```

## 2597. The Number of Beautiful Subsets
> :orange_circle:

## 2598. Smallest Missing Non-negative Integer After Operations
> :orange_circle: