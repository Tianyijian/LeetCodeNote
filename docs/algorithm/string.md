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
