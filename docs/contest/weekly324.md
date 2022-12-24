# Weekly Contest 324
> 2022.12.18

## 2506. Count Pairs Of Similar Strings

> :green_circle:

由相同字符组成的两个字符串是相似的。给字符串数组，返回相似字符串的对数。数组中的字符串仅包含小写字母

### 方法

- 找出每个字符串包含的字母，拼接字母得到新字符串，使用map记录新字符串出现次数

```cpp
class Solution {
public:
    int similarPairs(vector<string>& words) {
        unordered_map<string, int> map;
        int ans = 0;
        for (string w : words) {
            vector<int> hash(26, 0);
            for (char c : w) hash[c - 'a'] = 1;
                
            string s;
            for (int i = 0; i < 26; i++) {
                if (hash[i] == 1) s.push_back(i + 'a');
            }
            
            if (map.find(s) != map.end()) {
                ans += map[s];
                map[s]++;
            } else map[s] = 1;
        }
        return ans;
    }
};
```

## 2507. Smallest Value After Replacing With Sum of Prime Factors

> :orange_circle:

给正整数n，持续将n替换为质因数的和，返回得到的最小的n

### 方法

- 遍历计算得到n的质因数的和，替换n，直至不能替换。

```cpp
class Solution {
public:
    int smallestValue(int n) {
        while (true) {
            int sum = 0;
            for (int i = 2; i < n / 2; i++) {
                if (n % i == 0) {
                    sum += i;
                    n = n / i;
                    i -= 1;
                }
            }
            if (sum == 0) break;
            sum += n;
            n = sum;
        }
        return n;
    }
};
```

## 2508. Add Edges to Make Degrees of All Nodes Even

> :red_circle:

n个节点的无向图，图可能不连通。添加最多两条边，判断是否能让图中每个节点的度为偶数

### 方法

- 找出图中度为奇数的节点，这些节点数不能大于4，不能为奇数，因此只能是2或者4
- 有2个奇数度的节点时，若不相连可直接相连，若相连可寻找第三个都不相连的节点
- 有4个奇数度的节点时，由于最多添加两条边，只能是找到两组不相连的将其相连

```cpp
class Solution {
public:
    bool isPossible(int n, vector<vector<int>>& edges) {
        map<pair<int, int>, bool> hasEdge;
        vector<int> degree(n + 1);
        for (auto& e : edges) {
            degree[e[0]]++;
            degree[e[1]]++;
            hasEdge[{e[0], e[1]}] = 1;
            hasEdge[{e[1], e[0]}] = 1;
        }
        vector<int> odd;
        for (int i = 1; i <= n; i++) {
            if (degree[i] % 2 != 0) {
                odd.push_back(i);
            }
        }
        if (odd.size() == 0) return true;
        if (odd.size() > 4 || odd.size() % 2 != 0) return false;
        if (odd.size() == 2) {
            if (!hasEdge[{odd[0], odd[1]}]) return true;
            else {
                for (int i = 1; i <= n; i++) {
                    if (i == odd[0] || i == odd[1]) continue;
                    if (!hasEdge[{odd[0], i}] && !hasEdge[{odd[1], i}]) return true;
                }
                return false;
            }
        } else if (odd.size() == 4){
            int a = odd[0], b = odd[1], c = odd[2], d = odd[3];
            if (!hasEdge[{a, b}] && !hasEdge[{c, d}]) return true;
            if (!hasEdge[{a, c}] && !hasEdge[{b, d}]) return true;
            if (!hasEdge[{a, d}] && !hasEdge[{b, c}]) return true;
            return false;
        }
        return false;
    }
};
```

## 2509. Cycle Length Queries in a Tree

> :red_circle:

完全二叉树，有`2^n - 1`个节点。节点取值范围是`[1, 2^(n - 1) - 1]`，每个节点的左子节点：`2 * val`，右子节点：`2 * val + 1`。找到任意两个节点添加一条边后形成的环的长度

### 方法

- 完全二叉树：每个节点的左子节点：`2 * val`，右子节点：`2 * val + 1`。因此父节点是 `val / 2`
- 找到两个节点的最近公共祖先，记录长度，最终+1即为环的长度。对每个查询，T: O(loga + logb), S: O(1)

```cpp
class Solution {
public:
    vector<int> cycleLengthQueries(int n, vector<vector<int>>& queries) {
        vector<int> ans(queries.size(), 1);
        for (int i = 0; i < queries.size(); i++) {
            int a = queries[i][0], b = queries[i][1];
            while (a != b) {
                if (a > b) a /= 2; 
                else b /= 2;
                ans[i]++;
            }
        }
        return ans;
    }
};
```

