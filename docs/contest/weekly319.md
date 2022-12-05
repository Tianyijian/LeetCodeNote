# Weekly Contest 319
> 2022.11.13

## 2469. Convert the Temperature

> :green_circle:

将浮点数celsius温度转换为Kelvin和Fahrenheit温度，`Kelvin = Celsius + 273.15`，`Fahrenheit = Celsius * 1.80 + 32.00` 

### 方法

```cpp
class Solution {
public:
    vector<double> convertTemperature(double celsius) {
        vector<double> ans(2);
        ans[0] = celsius + 273.15;
        ans[1] = celsius * 1.80 + 32.00;
        return ans;
    }
};
```

## 2470. Number of Subarrays With LCM Equal to K

> :orange_circle:

给一个整数数组和整数k，返回子数组的数量，其中每个子数组的最小公倍数是k

### 方法

- 暴力搜索每个子串，保存之前的LCM。T: O(n^2), S: O(1)

```cpp
class Solution {
public:
    int subarrayLCM(vector<int>& nums, int k) {
        int cnt = 0;
        for (int i = 0; i < nums.size(); i++) {
            int curLcm = nums[i];
            for (int j = i; j < nums.size(); j++) {
                curLcm = lcm(curLcm, nums[j]);
                if (curLcm == k) cnt++;
                else if (curLcm > k) break;
            }
        }
        return cnt;
    }
};
```

### 拓展

- 最大公约数（Greatest Common Divisor, GCD）
  - a, b的最大公约数同时也是b, a mod b的最大公约数
  - 欧几里得算法：a, b全为0，GCD不存在；a, b之一为0，GCD为非零数；a, b都不为0，新a=b, 新b = a mod b
- 最小公倍数（Least Common Multiple，LCM）
  - 最小公倍数为两数乘积除以最大公约数：LCM(a, b) = a * b / GCD(a, b)

```cpp
int gcd(int a, int b) {
    while (b != 0) {
        int temp = a;
        a = b;
        b = temp % b;
    }
    return a;
}

int lcm(int a, int b) {
    return a * b / gcd(a, b);
}

int lcm(int a, int b) {
    int res = a * b;
    while (b != 0) {
        int temp = a;
        a = b;
        b = temp % b;
    }
    return res / a;
}
```

## 2471. Minimum Number of Operations to Sort a Binary Tree by Level

> :orange_circle:

给定含有唯一值的二叉树，同层之间交换节点使每层数值为严格递增，返回最小的交换次数

### 方法

- 使用队列层序遍历，得到每一层的数值数组。关键在于如何使用最小的交换次数将该数组变有序
- 方法一：(TLE) 选择排序思想，每轮在未排序序列中找到最小值，放在已排序序列的末尾。O(n^2)
- 方法二：map记录下标，对比原数组与已排序数组，根据已排序数组直接交换到位。O(nlogn)
- 方法三：对原数组的下标排序，直接将下标交换到对应位置即可。O(nlogn)

<!-- tabs: start -->
####**First**

```cpp
class Solution {
public:
    int minimumOperations(TreeNode* root) {
        queue<TreeNode*> que;
        que.push(root);
        int res = 0;
        while (!que.empty()) {
            int size = que.size();
            vector<int> nums;
            for (int i = 0; i < size; i++) {
                TreeNode* cur = que.front();
                que.pop();
                nums.push_back(cur->val);
                if (cur->left) que.push(cur->left);
                if (cur->right) que.push(cur->right);
            }
            res += swapTimes(nums);
        }
        return res;
    }
private:
    int swapTimes(vector<int>& nums) {
        if (nums.size() <= 1) return 0;
        int times = 0;
        for (int i = 0; i < nums.size(); i++) {
            int minId = i;
            for (int j = i + 1; j < nums.size(); j++) {
                if (nums[j] < nums[minId]) minId = j;
            }
            if (minId != i) {
                // swap(nums[i], nums[minId]); 
                nums[minId] = nums[i];
                times += 1; 
            }
        }
        return times;
    }
};
```
####**Second**

```cpp
class Solution {
public:
    int minimumOperations(TreeNode* root) {
        queue<TreeNode*> que;
        que.push(root);
        int res = 0;
        while (!que.empty()) {
            int size = que.size();
            vector<int> nums;
            for (int i = 0; i < size; i++) {
                TreeNode* cur = que.front();
                que.pop();
                nums.push_back(cur->val);
                if (cur->left) que.push(cur->left);
                if (cur->right) que.push(cur->right);
            }
            res += countSwap(nums);
        }
        return res;
    }
private:
    int countSwap(vector<int>& t){
        int count = 0;
        map<int,int> mp;
        vector<int> u;
        for(int i = 0; i < t.size(); ++i){
            mp[t[i]] = i;
            u.push_back(t[i]);
        }
        sort(u.begin(), u.end());
        for(int i = 0; i < t.size(); ++i){
            if(t[i] != u[i] ){
                t[mp[u[i]]] = t[i];
                mp[t[i]] = mp[u[i]];
                mp[u[i]] = i;
                t[i] = u[i];
                count++;
            }
        }
        return count;
    }
};
```

####**Third**

```cpp
class Solution {
public:
    int minimumOperations(TreeNode* root) {
        queue<TreeNode*> que;
        que.push(root);
        int res = 0;
        while (!que.empty()) {
            int size = que.size();
            vector<int> nums;
            for (int i = 0; i < size; i++) {
                TreeNode* cur = que.front();
                que.pop();
                nums.push_back(cur->val);
                if (cur->left) que.push(cur->left);
                if (cur->right) que.push(cur->right);
            }
            res += minOps(nums);
        }
        return res;
    }
private:
    int minOps(vector<int>& arr) {
        int n = arr.size(), ans = 0;
        vector<pair<int, int>> curr(n);
        for (int i = 0; i < n; i++) {
            curr[i].first = arr[i];
            curr[i].second = i;
        }
        sort(curr.begin(), curr.end());
        vector<bool> vis(n, false);
        for (int i = 0; i < n; i++) {
            if (vis[i] || curr[i].second == i) continue;
            int cnt = 0, j = i;
            while (!vis[j]) {
                vis[j] = 1;
                j = curr[j].second;
                cnt++;
            }
            if (cnt > 0) ans += (cnt - 1);
        }
        return ans;
    }
};
```


<!-- tabs: end -->
## 2472. Maximum Number of Non-overlapping Palindrome Substrings

> :red_circle:

给定字符串s和正整数k，从s中选择非重叠的子串集合，每个子串是长度至少为k的回文子串，返回最大的子串数量

### 方法

```cpp
class Solution {
public:
    int maxPalindromes(string s, int k) {
        int n = s.size(), ans = 0, start = 0;
        for (int center = 0; center < 2 * n; center++) {
            int left = center / 2;
            int right = left + center % 2;
            while (left >= start && right < n && s[left] == s[right]) {
                if (right + 1 - left >= k) {
                    ans++;
                    start = right + 1;
                    break;
                }
                left--; right++;
            }
        }
        return ans;
    }
};
```

