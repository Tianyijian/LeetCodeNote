# Weekly Contest 336
> 2023.03.12

## 2586. Count the Number of Vowel Strings in Range
> :green_circle:

给一个字符串数组，两个整数left和right。**元音字符串**的首字母和尾字母是元音"a, e, i, o, u"。返回下标[left, right]间的**元音字符串**个数

### 方法

- 遍历下标范围，用集合存储元音字母，判断每个字符串的首尾字母是否是元音

```cpp
class Solution {
public:
    int vowelStrings(vector<string>& words, int left, int right) {
        unordered_set<char> set = {'a', 'e', 'i', 'o', 'u'}; 
        int ans = 0;
        for (int i = left; i <= right; i++) {
            int n = words[i].size() - 1;
            if (set.count(words[i][0]) && set.count(words[i][n])) ans++;
        }
        return ans;
    }
};
```


## 2587. Rearrange Array to Maximize Prefix Score
> :orange_circle:

给一个整数数组，将其元素重排为任意顺序，使得数组的前缀和数组中，正数的个数最多。返回最多的正数数量

### 方法

- 将数组按照降序排列，前缀和的正数最多。依次计算前缀和，出现非正数停止

```cpp
class Solution {
public:
    int maxScore(vector<int>& nums) {
        sort(nums.begin(), nums.end(), greater<int>());
        long long pSum = 0;
        for (int i = 0; i < nums.size(); i++) {
            pSum += (long long)nums[i];
            if (pSum <= 0) return i;
        }
        return nums.size();
    }
};
```


## 2588. Count the Number of Beautiful Subarrays
> :orange_circle:

给一个整数数组，一次操作如下：选两个不同下标，选一个整数k，这两个数的二进制表示中第k位都为1，将这两个数减去$2^k$。**优美子数组**是应用操作任意次后，数组元素全为0。返回**优美子数组**的个数

### 方法

- 

```cpp

```


## 2589. Minimum Time to Complete All Tasks
> :red_circle:

计算机同时可进行无数任务。给一个任务列表，`tasks[i] = [starti, endi, durationi]`，代表任务完成时间为duration（可不连续），需要在[start, end]范围内完成。计算机需要完成任务时就可以打开，空闲时可以关闭。返回计算机完成所有任务应该被打开的最小时间