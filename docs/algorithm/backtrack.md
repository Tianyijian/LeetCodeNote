# 回溯算法

## 0093. Restore IP Addresses

> :orange_circle:

给一个只包含数字的字符串，返回所有有效的IP地址。IP地址由四个[0, 255]之间的整数组成，数不能由0开头

### 方法

- 注意判断合法性：子串长度小于4，数值不大于255，不能“0”开头
* 需要记录已经分割的段数，path可以使用数组也可以使用字符串

<!-- tabs:start-->
####**First**

```cpp
class Solution {
public:
    vector<string> restoreIpAddresses(string s) {
        res.clear();
        path.clear();
        if (s.size() < 4 || s.size() > 12) return res;
        backtracking(s, 0);
        return res;
    }
private:
    vector<string> res;
    vector<string> path;
    void backtracking(const string& s, int startIndex) {
        if (path.size() > 4) return;
        if (startIndex >= s.size() && path.size() == 4) {
            string item = path[0] + "." + path[1] + "." + path[2] + "." + path[3];
            res.push_back(item);
            return;
        }
        for (int i = startIndex; i < s.size() && i < startIndex + 3; i++) {
            string str = s.substr(startIndex, i - startIndex + 1);
            if (stoi(str) > 255) continue;
            path.push_back(str);
            backtracking(s, i + 1);
            path.pop_back();
            if (str == "0") break;
        }
    }
};
```
####**Second**
```cpp
class Solution {
public:
    vector<string> restoreIpAddresses(string s) {
        if (s.size() < 4 || s.size() > 12) return {};
        backtracking(s, "", 0, 0);
        return res;
    }
private:
    vector<string> res;
    void backtracking(string s, string path, int start, int k) {
        if (k > 4) return;
        if (start >= s.size() && k == 4) {
            path.pop_back();
            res.push_back(path);
            return;
        }
        for (int i = start; i <= start + 2 && i < s.size(); i++) {
            string str = s.substr(start, i - start + 1);
            if (stoi(str) > 255) continue;
            backtracking(s, path + str + ".", i + 1, k + 1);
            if (str == "0") break;
        }
    }
};
```

<!-- tabs:end-->

## 0039. Combination Sum

> :orange_circle:

### 方法
