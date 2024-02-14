# 滑动窗口

## Intro

- 左右指针维护一个滑动窗口，右指针右移向窗口添加元素，左指针右移移除元素，不断更新结果
- 设计左闭右开的区间[left, right)最方便处理。初始时[0,0)没有元素，right右移一位[0,1)就包含元素0
- 套模板时思考：增大窗口时应更新哪些数据？什么时候开始缩小窗口，缩小时应更新哪些数据？什么时候更新结果？
- 学习资源
  - https://labuladong.gitee.io/algo/1/12/

```cpp
void slidingWindow(string s) {
    unordered_map<char, int> window;
    int left = 0, right = 0;
    while (right < s.size()) {
        char c = s[right]; // c 是将移入窗口的字符
        right++; // 增大窗口
        ... // 进行窗口内数据的一系列更新
        while (window needs shrink) {  // 判断左侧窗口是否要收缩
            char d = s[left];  // d 是将移出窗口的字符
            left++; // 缩小窗口
            ... // 进行窗口内数据的一系列更新
        }
    }
}
```

## 0076. Minimum Window Substring

> :red_circle:

给两个字符串s和t，在s中寻找包含t全部字符（t中有重复字符）的子串，返回长度最短的子串，没有返回“”

### 方法

- 滑动窗口，扩大窗口直至包含t全部字符，然后开始收缩直至不再满足要求，每次收缩时更新结果
- 哈希表need统计t中字符及次数，window统计窗口中字符及次数（只需统计need中出现的）
- 整数valid记录窗口中满足need条件的字符个数，等于need.size()时，窗口满足条件

```cpp
class Solution {
public:
    string minWindow(string s, string t) {
        unordered_map<char, int> need, window;
        for (char c : t) need[c]++;
        int left = 0, right = 0;
        int valid = 0;
        int start = 0, minLen = INT_MAX;
        while (right < s.size()) {
            char c = s[right];
            right++;
            if (need.find(c) != need.end()) {
                window[c]++;
                if (window[c] == need[c]) valid++;
            }
            while (valid == need.size()) {
                if (minLen > right - left) {
                    start = lefts;
                    minLen = right - left;
                }
                char c = s[left];
                left++;
                if (need.find(c) != need.end()) {
                    if (window[c] == need[c]) valid--;
                    window[c]--;
                }
            }
        }
        return minLen == INT_MAX ? "" : s.substr(start, minLen);
    }
};
```

## 0567. Permutation in String

> :orange_circle:

给两个字符串s1和s2，判断s2是否有一个子串，是s1的一种排列

### 方法

- 滑动窗口，要求窗口包含s1的全部字符（可能有重复字符），需要哈希表need, window以及valid。T: O(L1+L2), S: O(1)
- 因为是s1的排列，窗口不能多字符，所有窗口大小固定为s1.size()，维护的是**定长窗口**，按照窗口大小进行滑动
- 维护定长窗口时，窗口滑动每次移入一个字符，移出一个字符，收缩的while条件可以改为if
- valid用于统计两个哈希表中已经满足数量要求的字符数量

```cpp
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        unordered_map<char, int> need, window;
        for (char c : s1) need[c]++;
        int left = 0, right = 0;
        int valid = 0;
        while (right < s2.size()) {
            char c = s2[right];
            right++;
            if (need.find(c) != need.end()) {
                window[c]++;
                if (window[c] == need[c]) valid++;
            }
            // if (valid == need.size()) return true;	//因为维护的是定长窗口，此处也可以判断是否满足要求
            if (right - left == s1.size()) {
                if (valid == need.size()) return true; // 满足要求时，立即返回true
                char c = s2[left];
                left++;
                if (need.find(c) != need.end()) {
                    if (window[c] == need[c]) valid--;
                    window[c]--;
                }
            }
        }
        return false;
    }
};
```

## 0438. Find All Anagrams in a String

> :orange_circle:

给两个字符串s和p，找到s中p的所有字母异位词的下标

### 方法一

- 滑动窗口，字母异位词就是p的一种排列，相比于567题，只需要多记录每个满足条件的窗口的起始下标即可。T: O(L1+L2), S: O(1)

```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        unordered_map<char, int> need, window;
        for (char c : p) need[c]++;
        int left = 0, right = 0;
        int valid = 0;
        vector<int> res;
        while (right < s.size()) {
            char c = s[right];
            right++;
            if (need.count(c)) {
                window[c]++;
                if (window[c] == need[c]) valid++;
            }
            if (valid == need.size()) res.push_back(left);
            if (right - left == p.size()) {
                char c = s[left];
                left++;
                if (need.count(c)) {
                    if (window[c] == need[c]) valid--;
                    window[c]--;
                }
            }
        }
        return res;
    }
};
```

### 方法二

- 滑动窗口，使用哈希数组记录字母，vector可以直接比较内容。将窗口中的字母都装入，然后移动比较。T: O(L2), S: O(1) 

```cpp
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        vector<int> ans;
        if (s.size() < p.size()) return ans;
        vector<int> need(26, 0), have(26, 0);
        int ps = p.size();
        for (int i = 0; i < ps; i++) {
            need[p[i] - 'a']++;
            have[s[i] - 'a']++;
        }
        if (need == have) ans.push_back(0);
        for (int i = ps; i < s.size(); i++) {
            have[s[i] - 'a']++;
            have[s[i - ps] - 'a']--;
            if (need == have) ans.push_back(i - ps + 1);
        }
        return ans;
    }
};
```

## 0003. Longest Substring Without Repeating Characters

> :orange_circle:

给一个字符串，返回没有重复字符的最长子串的长度。字符串由English letters, digits, symbols and spaces组成

### 方法一

* 滑动窗口，哈希表记录窗口内字符出现的次数，大于1时有重复字符，开始收缩窗口
* 收缩完成后，窗口一定没有重复字符，更新最大结果。T: O(n), S: O(1)

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int hash[128] = {0};
        int left = 0, right = 0;
        int maxLen = 0;
        while (right < s.size()) {
            char c = s[right];
            right++;
            hash[c]++;
            while (hash[c] > 1) {
                char d = s[left];
                left++;
                hash[d]--;
            }
            maxLen = max(right - left, maxLen);
        }
        return maxLen;
    }
};
```
### 方法二

* 滑动窗口，用哈希表记录每个字符最后出现的位置
* 根据当前字符确定滑动窗口的起始位置，确保窗口内一定没有重复字符（相比方法一，该方法一步收缩到位）
* 记录最大长度，更新当前字符的哈希表。TC: O(n), SC: O(1)
* 数组哈希
  * int[26] for Letters 'a' - 'z' or 'A' - 'Z'
  * int[128] for ASCII
  * int[256] for Extended ASCII

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int hash[128] = {0};
        int maxLen = 0;
        int start = 0;
        for (int end = 0; end < s.size(); end++) {
            char ch = s[end];
            start = max(start, hash[ch]);
            maxLen = max(maxLen, end - start + 1);
            hash[ch] = end + 1;
        }
        return maxLen;
    }
};
```

### 方法三

* 动态规划(TLE)，类似回文子串，记录最大长度，`dp[i][j]` 下标为i~j的子串，是否没有重复字母
* 左下往右上遍历，TC: O(n^2) SC: O(n^2)

```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int n = s.size();
        int maxLen = 0;
        vector<vector<bool>> dp(n, vector<bool>(n, false));
        for (int i = n - 1; i >= 0; i--) {
            for (int j = i; j < n; j++) {
                if (s[i] == s[j] && i == j) dp[i][j] = true;
                else if (s[i] != s[j] && (i = j - 1 || dp[i + 1][j - 1])) dp[i][j] = true;
                if (dp[i][j]) maxLen = max(maxLen, j - i + 1);
            }
        }
        return maxLen;
    }
};
```
## Other Problems
- 2461 Maximum Sum of Distinct Subarrays With Length K