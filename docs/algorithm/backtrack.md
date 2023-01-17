# 回溯算法

## 0093. Restore IP Addresses

> :orange_circle:

给一个只包含数字的字符串，返回所有有效的IP地址。IP地址由四个[0, 255]之间的整数组成，数不能由0开头

### 方法

- 注意判断合法性：子串长度小于4，数值不大于255，不能“0”开头
* 需要记录已经分割的段数，path可以使用数组也可以使用字符串

<!-- tabs:start -->
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

<!-- tabs:end -->

## 0039. Combination Sum

> :orange_circle:

给定一个数值唯一的候选数组以及整数target，从数组中选择一些数字构成组合，其和为target，每个数字可以被选多次。返回所有的不重复的组合

### 方法

- 回溯算法，记录路径。数组排序，从前往后遍历，通过起始位置start控制选取位置，防止`[2,3] [3,2]`的出现
- 下一层递归依旧从当前位置开始，可使当前元素重复使用多次。因为数组有序，当前和大于target时，停止搜索作为剪枝

```cpp
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        path.clear();
        res.clear();
        sort(candidates.begin(), candidates.end());
        backtracking(candidates, target, 0);
        return res;
    }
private:
    vector<vector<int>> res;
    vector<int> path;
    int sum = 0;
    void backtracking(vector<int>& candidates, int target, int start) {
        if (sum == target) {
            res.push_back(path);
            return;
        }
        for (int i = start; i < candidates.size(); i++) {
            if (sum + candidates[i] > target) return;
            sum += candidates[i];
            path.push_back(candidates[i]);
            backtracking(candidates, target, i);
            sum -= candidates[i];
            path.pop_back();
        }
    }
};
```

## 0047. Permutations II

> :orange_circle:

给定一维数组，可能含有重复元素，返回所有独特的排列。`1 <= nums.length <= 8, -10 <= nums[i] <= 10`

### 方法

- 树枝去重+树层去重

- 排列问题，树层去重和树枝去重都可以，树层去重效率更高

* 树层去重方法：
	* 1. 数组排序，if (i > 0 && nums[i] == nums[i-1]) continue;
	* 2. 直接哈希表, int used[21] = {0}; if (used[nums[i] + 10] == 1) continue;
		* 可以不排序（视题目而定），如子集问题需要排序，排列问题不需要
		* 数组哈希效率要比set，map表效率高
	
- 回溯法。每个元素只能使用一次，但可以每次都从头选取元素，因此采用used数组记录使用情况

- 每到一层，该位置不能使用重复元素，即树层去重

- 写法一：数组排序，在每一层时，使用指针移动去重

- 写法二：不用排序，数组取值范围有限，可以在每一层采用数组哈希，避免重复选取

<!-- tabs:start-->

####**First**

```cpp
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        path.clear();
        res.clear();
        sort(nums.begin(), nums.end());
        vector<bool> used(nums.size(), false);
        backtracking(nums, used);
        return res;
    }

private:
    vector<vector<int>> res;
    vector<int> path;
    void backtracking(vector<int>& nums, vector<bool>& used) {
        if (path.size() == nums.size()) {
            res.push_back(path);
            return;
        }
        for (int i = 0; i < nums.size(); i++) {
            if (!used[i]) {
                path.push_back(nums[i]);
                used[i] = true;
                backtracking(nums, used);
                used[i] = false;
                path.pop_back();
                while (i + 1 < nums.size() && nums[i + 1] == nums[i]) i++;
            }
        }
    }
};
```

####**Second**

```cpp
class Solution {
public:
    vector<vector<int>> permuteUnique(vector<int>& nums) {
        res.clear();
        path.clear();
        vector<bool> used(nums.size(), false);
        // sort(nums.begin(), nums.end());
        backtracking(nums, used);
        return res;
    }

private:
    vector<vector<int>> res;
    vector<int> path;
    void backtracking(vector<int>& nums, vector<bool>& used) {
        if (nums.size() == path.size()) {
            res.push_back(path);
            return;
        }
        vector<bool> wideUsed(21, false);
        for (int i = 0; i < nums.size(); i++) {
            if (used[i] || wideUsed[nums[i] + 10]) continue;
            wideUsed[nums[i] + 10] = true;
            used[i] = true;
            path.push_back(nums[i]);
            backtracking(nums, used);
            used[i] = false;
            path.pop_back();
        }
    }
};
```

<!-- tabs:end-->
