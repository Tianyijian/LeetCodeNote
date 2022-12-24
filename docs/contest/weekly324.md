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

### 方法

## 2509. Cycle Length Queries in a Tree

> :red_circle:

### 方法
