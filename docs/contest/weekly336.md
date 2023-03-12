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


## 2588. Count the Number of Beautiful Subarrays
> :orange_circle:


## 2589. Minimum Time to Complete All Tasks
> :red_circle: