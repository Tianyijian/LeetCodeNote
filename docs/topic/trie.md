# 前缀树

## 0212. Word Search II

> :red_circle:

给一个`mxn`的字符网格以及词典，返回字符网格能得到的词典中的词语。`1 <= m, n <= 12`，`1 <= words.length <= 3 * 10^4`，`1 <= words[i].length <= 10`

### 方法一

- 回溯法（DFS）遍历网格得到每个词语，使用前缀树（Trie）及时判断当前字符是否在任何词语中，从而进行剪枝
- T: O(M * N * 4 * 3^(L-2) + S)，`M * N`是网格数量，然后可搜索4个方向，接着都有3个方向搜索。`L <= 10`是单词的最大长度，`S <= 3 * 10^5`是所有单词的长度总和。S: O(S)，建立前缀树耗费空间，最坏情况下所有单词没有共同前缀

```cpp
struct TrieNode {
    TrieNode* children[26] = {};
    string* word;
    void addWord(string& word) {
        TrieNode* cur = this;
        for (char c : word) {
            c -= 'a';
            if (cur->children[c] == nullptr) cur->children[c] = new TrieNode();
            cur = cur->children[c];
        }
        cur->word = &word;
    }
};

class Solution {
public:
    int m, n;
    int DIR[5] = {0, 1, 0, -1, 0};
    vector<string> ans;
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        m = board.size(); n = board[0].size();
        TrieNode trieNode;
        for (string& word : words) trieNode.addWord(word);
        
        for (int r = 0; r < m; ++r)
            for (int c = 0; c < n; ++c)
                dfs(board, r, c, &trieNode);
        return ans;
    }
    void dfs(vector<vector<char>>& board, int r, int c, TrieNode* cur) {
        if (r < 0 || r == m || c < 0 || c == n || board[r][c] == '#' || cur->children[board[r][c]-'a'] == nullptr) return;
        char orgChar = board[r][c];
        cur = cur->children[orgChar - 'a'];
        if (cur->word != nullptr) {
            ans.push_back(*cur->word);
            cur->word = nullptr; // Avoid duplication!
        }
        board[r][c] = '#'; // mark as visited!
        for (int i = 0; i < 4; ++i) dfs(board, r + DIR[i], c + DIR[i+1], cur);
        board[r][c] = orgChar; // restore org state
    }
};
```

### 方法二

- 回溯法进行搜索（TLE），对词典中的每个词，使用map找到网格中的位置，开始DFS搜索

```cpp
class Solution {
public:
    vector<string> findWords(vector<vector<char>>& board, vector<string>& words) {
        int m = board.size(), n = board[0].size();
        unordered_map<char, vector<int>> map;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                map[board[i][j]].push_back(i * n + j);
            }
        }
        vector<string> res;
        for (string word : words) {
            if (map.find(word[0]) != map.end()) {
                for (int index : map[word[0]]) {
                    // cout << index << "-" << index / n << "-" << index % n << endl; 
                    visited = vector<vector<bool>>(m, vector<bool>(n, false));
                    if (dfs(board, word, 0, index / n, index % n)) {
                        res.push_back(word);
                        break;
                    }
                }
            }
        }
        return res;
    }

private:
    vector<vector<int>> diretion = {{1, 0}, {-1, 0}, {0, 1}, {0, -1}};
    vector<vector<bool>> visited;
    bool dfs(vector<vector<char>>& board, string word, int id, int row, int col) {
        if (row < 0 || col < 0 || row >= board.size() || col >= board[0].size()) return false;
        // cout << word << " " << word[id] << " " << board[row][col] << endl;
        if (visited[row][col]) return false;
        if (word[id] != board[row][col]) {
            return false;
        } else if (id == word.size() - 1) {
            return true;
        }
        visited[row][col] = true;
        for (auto d : diretion) {
            if (dfs(board, word, id + 1, row + d[0], col + d[1])) return true;
        }
        visited[row][col] = false;
        return false;
    }
};
```

