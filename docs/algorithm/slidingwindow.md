# 滑动窗口

## 3. Longest Substring Without Repeating Characters

> :orange_circle:

给一个字符串，返回没有重复字符的最长子串的长度。字符串由English letters, digits, symbols and spaces组成

### 方法一

* 滑动窗口，哈希表记录窗口内字符出现的次数，大于1时有重复字符，开始收缩窗口
* 收缩完成后更新最大结果。T: O(n), S: O(1)

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
* 根据当前字符确定滑动窗口的起始位置，记录最大长度，更新当前字符的哈希表。TC: O(n), SC: O(1)
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