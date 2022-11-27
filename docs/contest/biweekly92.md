# Biweekly Contest 92
> 20221126

## 2481. Minimum Cuts to Divide a Circle
> :green_circle:

将一个圆分成n等份需要的最少的切割线数量，切割线必须是直径或半径

### 方法

- 当n为1时，不切割，数量为0；当n为偶数时，切割线为直径，数量为n / 2；当n为奇数时，切割线为半径，数量为n

```cpp
class Solution {
public:
    int numberOfCuts(int n) {
        if (n == 1) return 0;
        if (n % 2 == 0) return n / 2;
        else return n;
    }
};
```

## 2482. Difference Between Ones and Zeros in Row and Column
> :orange_circle:

给一个充满0和1的二维矩阵，根据公式计算出其对应的差异矩阵：`diff[i][j] = onesRowi + onesColj - zerosRowi - zerosColj`，其中`onesRowi`代表第i行1的个数，其余同理

### 方法

- 先统计出每行1的个数，每列1的个数，0的个数=总数-1的个数，然后根据公式计算差异矩阵即可。T: O(mn), S: O(mn)

```cpp
class Solution {
public:
    vector<vector<int>> onesMinusZeros(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> diff(m, vector<int>(n));
        vector<int> onesRow(m);
        vector<int> onesCol(n);
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == 1) {
                    onesRow[i]++;
                    onesCol[j]++;
                }
            }
        }
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                diff[i][j] = onesRow[i] + onesCol[j] - (m - onesRow[i]) - (n - onesCol[j]);
            }
        }
        return diff;
    }
};
```

## 2483. Minimum Penalty for a Shop
> :orange_circle:

给一个字符串，记录每个时间点商店是否有顾客来访。对每个时间点，店开着没顾客惩罚+1，店关着有顾客惩罚+1，选择一个惩罚最小的时间点关店

### 方法

- 选择在一个时间点关店时，惩罚=之前（不包括自身）没有顾客的数量+之后（包括自身）有顾客的数量
- 先统计出顾客来访的总次数，然后依次在每个时间点关店，根据公式计算惩罚，并保留最小值。T: O(n), S: O(1)

```cpp
class Solution {
public:
    int bestClosingTime(string customers) {
        int n = customers.size();
        int Y = 0;
        for (int i = 0; i < n; i++) {
            if (customers[i] == 'Y') Y++;
        }
        int minIndex = 0;
        int minPenalty = INT_MAX;
        int yNum = 0;
        for (int i = 0; i <= n; i++) {
            int penalty = (i - yNum) + (Y - yNum);
            if (penalty < minPenalty) {
                minPenalty = penalty;
                minIndex = i;
            }
            if (customers[i] == 'Y') yNum++;
        }
        return minIndex;
    }
};
```

## 2484. Count Palindromic Subsequences

> :red_circle:

给一个由数字组成的字符串s，返回s中长度为5的回文子序列的数量。由于答案较大，结果对`10^9 + 7`取余

### 方法一

- 回溯寻找子序列，在DFS搜索时保证回文。（TLE）

```cpp
class Solution {
public:
    int countPalindromes(string s) {
        if (s.size() < 5) return 0;
        path = vector<char>(5);
        backtracking(s, 0, 0);
        return res;
    }
private:
    int res = 0;
    int MOD = 1e9 + 7;
    vector<char> path;
    void backtracking(string s, int start, int size) {
        if (size == 5) {
            res += 1;
            if (res > MOD) res %= MOD;
            return;
        }
        for (int i = start; i < s.size(); i++) {
            if (s.size() - i < 5 - size) return;
            if (size == 3 && s[i] != path[1]) continue;
            else if (size == 4 && s[i] != path[0]) continue;
            path[size] = s[i];
            backtracking(s, i + 1, size + 1);
        }
    }
};
```

### 方法二