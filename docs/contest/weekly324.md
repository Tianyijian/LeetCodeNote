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

### 方法

## 2508. Add Edges to Make Degrees of All Nodes Even

> :red_circle:

### 方法

## 2509. Cycle Length Queries in a Tree

> :red_circle:

### 方法
