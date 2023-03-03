# 字符串

## 0151. Reverse Words in a String

> :orange_circle:

给定一个字符串，反转字符串中的单词。字符串中可能包含多个空格，将额外的空格删掉	

### 方法一

- 先把单词分割出来，然后倒序相加，较为简单。T: O(n), S: O(n)
- substr(pos, len)：从下标pos开始，长度len个元素。substr(i, j - i): [i, j)

```cpp
class Solution {
public:
    string reverseWords(string s) {
        vector<string> words;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] != ' ') {
                int j = i + 1;
                while (j < s.size() && s[j] != ' ') j++;
                words.push_back(s.substr(i, j - i));
                i = j;
            }
        }
        string res;
        for (int i = words.size() - 1; i > 0; i--) {
            res += words[i] + " ";
        }
        res += words[0];
        return res;
    }
};
```

### 方法二

- 原地操作并且不使用额外空间。双指针移除多余空格，整体反转+单词反转
- reverse(s.begin() + i, s.begin() + j): [i, j)

```cpp
class Solution {
public:
    string reverseWords(string s) {
        int slow = 0;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] != ' ') s[slow++] = s[i];
            else if (slow > 0 && s[slow - 1] != ' ') s[slow++] = s[i];
        }
        while (s[slow - 1] == ' ') slow--;
        s.resize(slow);
        reverse(s.begin(), s.end());
        for (int i = 0; i < s.size(); i++) {
            int j = i + 1;
            while (j < s.size() && s[j] != ' ') j++;
            reverse(s.begin() + i, s.begin() + j);
            i = j;
        } 
        return s;
    }
};
```

## 0028. Find the Index of the First Occurrence in a String

> :orange_circle:

给两个字符串`needle`和`haystack`，返回`needle`在`haystack`中的第一次出现位置，没有则返回-1。`1 <= haystack.length, needle.length <= 10^4`

### 方法一

* 暴力解法：对每个起始位置进行匹配，T: O(m*n), S: O(1)
* 注意`size()`返回size_t，是unsigned int，直接size()相减遇到0-1就会变为大正数而溢出，事先定义为int就可以

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        int m = haystack.size(), n = needle.size();
        for (int i = 0; i <= m - n; i++) {
            int j = 0, k = i;
            while (haystack[k] == needle[j]) {
                k++;
                j++;
                if (j == n) return i;
            }
        }
        return -1;
    }
};
```

### 方法二

* KMP字符串匹配算法：T: O(m+n), S: O(n)
  * 当字符串不匹配时，利用已经匹配的内容信息，避免从头匹配。[KMP讲解](https://programmercarl.com/0028.%E5%AE%9E%E7%8E%B0strStr.html)
  * 构建前缀表（前缀，后缀，最长相同前后缀），统一减一作为next数组，供匹配使用
  * 前缀是指不包含最后一个字符的所有以第一个字符开头的连续子串，后缀是指不包含第一个字符的所有以最后一个字符结尾的连续子串，前缀表就是下标i之前（包括i）的字符串中，有多大长度的相同前缀后缀。
  * 匹配失败的位置是后缀子串的后面，找到与其相同的最长的前缀的后面重新匹配即可。

```cpp
class Solution {
public:
    int strStr(string haystack, string needle) {
        int m = haystack.size(), n = needle.size();
        int next[n];
        getNext(next, needle);
        int j = -1;
        for (int i = 0; i < m; i++) {
            while (j >= 0 && haystack[i] != needle[j + 1]) {
                j = next[j];
            }
            if (haystack[i] == needle[j + 1]) {
                j++;
            }
            if (j == n - 1) return i - n + 1;
        }
        return -1;
    }
private:
    void getNext(int* next, const string& s) {
        int j = -1;
        next[0] = j;
        for (int i = 1; i < s.size(); i++) {
            while (j >= 0 && s[i] != s[j + 1]) {
                j = next[j];
            }
            if (s[i] == s[j + 1]) {
                j++;
            }
            next[i] = j;
        }
    }
};
```

