# Weekly Contest 322
> 2022.12.04

## 2490. Circular Sentence
> :green_circle:

句子由一连串单词组成，单词由大小写字母组成，单个空格分割。判断句子是否成环：一个单词的最后字符与下一个单词的首字符相同，最后一个单词的最后字符与第一个单词的首字符相同。

### 方法

- 判断句首与句尾字符是否相同。找到每个空格，判断前一个字符与后一个字符是否相等。T: O(n), S: O(1)

```cpp
class Solution {
public:
    bool isCircularSentence(string sentence) {
        int n = sentence.size();
        if (sentence[0] != sentence[n - 1]) return false;
        for (int i = 0; i < n; i++) {
            if (sentence[i] == ' ') {
                if (sentence[i - 1] != sentence[i + 1]) return false;
            }
        }
        return true;
    }
};
```

## 2491. Divide Players Into Teams of Equal Skill
> :orange_circle:

长度为偶数n的整数数组，分成`n/2`个组，每组两个数，各组和相等。返回每组数乘积的和

### 方法一

- 一遍遍历计算全部和，得到每组的和作为Target。
- 类似Two Sum，从左至右遍历，哈希表记录出现的元素，对当前元素`i`看是否存在`target-i`。T: O(n), S: O(n)

```cpp
class Solution {
public:
    long long dividePlayers(vector<int>& skill) {
        int n = skill.size();
        int sum = 0;
        for (int i : skill) sum += i;
        if (sum % (n / 2) != 0) return -1;
        int target = sum / (n / 2);
        
        unordered_map<int, int> cnt;
        long long res = 0;
        for (int i : skill) {
            if (cnt.find(target - i) != cnt.end()) {
                res += i * (target - i);
                cnt[target - i]--;
                if (cnt[target - i] == 0) cnt.erase(target - i);
            } else cnt[i]++;
        }
        if (cnt.size() != 0) return -1;
        return res;
    }
};
```

### 方法二

- 哈希表先统计所有出现的数及其频率，然后遍历哈希表，配对并进行计算，由于结果重复计算最后除以2。T: O(n), S: O(n)

```cpp
class Solution {
public:
    long long dividePlayers(vector<int>& skill) {
        int n = skill.size();
        int sum = accumulate(skill.begin(), skill.end(), 0);
        if (sum % (n / 2) != 0) return -1;
        int target = sum / (n / 2);

        unordered_map<int, int> cnt;
        for (int i : skill) cnt[i]++;

        long long res = 0;
        for (auto [k, v] : cnt) {
            if (v != cnt[target - k]) return -1;
            else res += (long long) k * (target- k) * v;
        }
        return res / 2;
    }
};
```

### 拓展

- C++ 累积函数`accumulate`

```cpp
template <class InputIterator, class T, class BinaryOperation>
T accumulate (InputIterator first, InputIterator last, T init, BinaryOperation binary_op); 
//起始迭代器，结束迭代器，初始值，自定义操作函数

int sum = accumulate(vec.begin(), vec.end(), 0); // 计算vector<int>数组和
int sum = accumulate(vec.begin(), vec.end(), 1, multiplies<int>()); // 计算vector<int>数组乘积	

string sum = accumulate(v.begin(), v.end(), string("")) //连接vector<string>
string sum = accumulate(v.begin(), v.end(), string(""), [](string str, int n) {return str + to_string(n)}); //连接vector<int> 为string

int nums[] = {10, 20, 30};
int sum = accumulate(nums, nums + 3, 0) //计算int数组和
```

## 2492. Minimum Score of a Path Between Two Cities

> :orange_circle:

有`n`个城市，从 `1` 到 `n` 编号，二维`roads`数组，`roads[i] = [ai, bi, distancei]`。城市构成的图不一定是连通的。找到城市 `1` 和城市 `n` 之间的所有路径的最小分数（路径中道路的最小距离）。路径可以多次包含同一道路，城市 `1` 和城市`n` 之间 **至少** 有一条路径

### 方法一

- 并查集，构建城市`1`和`n`所在的连通分量，找到其中道路的最小值即可。T: O(n + e), S: (n)

```cpp
class Solution {
public:
    int minScore(int n, vector<vector<int>>& roads) {
        init(n + 1);
        for (auto r : roads) {
            if (!same(r[0], r[1])) join(r[0], r[1]);
        }
        int ans = INT_MAX;
        for (auto r : roads) {
            if (same(r[0], n)) ans = min(ans, r[2]);
        }
        return ans;
    }
private:
    vector<int> father;
    void init (int n) {
        father = vector<int>(n);
        for (int i = 0; i < n; i++) father[i] = i;
    }
    int find (int u) {
        return father[u] == u ? u : father[u] = find(father[u]);
    }
    void join(int u, int v) {
        father[find(u)] = find(v);
    }
    bool same(int u, int v) {
        return find(u) == find(v);
    }
};
```

### 方法二

- DFS或BFS进行遍历，从1出发，遍历所有能走的路径，记录其中的最小值即可。T: O(n + e), S: (n + e)

```cpp
class Solution {
public:
    int minScore(int n, vector<vector<int>>& roads) {
        vector<vector<vector<int>>> adj(n + 1);
        for (auto& r : roads) {
            adj[r[0]].push_back({r[1], r[2]});
            adj[r[1]].push_back({r[0], r[2]});
        }
        vector<bool> vis(n + 1);
        int ans = INT_MAX;
        dfs(1, adj, vis, ans);
        return ans;
    }
private:
    void dfs(int cur, vector<vector<vector<int>>>& adj, vector<bool>& vis, int& ans) {
        vis[cur] = true;
        for (auto& r : adj[cur]) {
            ans = min(ans, r[1]);
            if (!vis[r[0]]) {
                dfs(r[0], adj, vis, ans);
            }
        }
    }
};
```

### 拓展

- c++ 函数`iota`，用于生成连续的数字序列

```cpp
void iota (ForwardIterator first, ForwardIterator last, T val);

vector<int> nums(5);
itoa(nums.begin(), nums.end(), 1); //vector<int>  1,2,3,4,5
    
int nums[5];
itoa(nums, nums + 5, 0) // int[]  0,1,2,3,4
```

## 2493. Divide Nodes Into the Maximum Number of Groups

> :red_circle:

有`n`个节点的无向图，编号`1`到`n`，边为 `edges[i] = [ai, bi]` ，图可能是不连通的。将图划分为`m`组（编号从`1 ` 开始）：每个节点只属于一组，每条边的两个节点的组编号相差`1`。返回最大的`m`，如果不能分组返回-1

### 方法

- 

```cpp

```

