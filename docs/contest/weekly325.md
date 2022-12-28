# Weekly Contest 325
> 2022.12.25

## 2515. Shortest Distance to Target String in a Circular Array
> :green_circle:

给一个环形字符串数组`words`和字符串`target`，`words[i]`的下一个元素是`words[(i+1)%n]`，上一个元素是`words[(i-1+n)%n]`。从`startIndex`出发，返回到达`target`的最短距离，若`target`不存在返回-1

### 方法

- 遍历走的步数，如果到达`target`，记录移动的距离，以及反向移动的距离。T: O(n), S: O(1)

```cpp
class Solution {
public:
    int closetTarget(vector<string>& words, string target, int startIndex) {
        int n = words.size();
        int ans = INT_MAX;
        for (int i = 0; i < n; i++) {
            int j = (i + startIndex) % n;
            if (words[j] == target) {
                ans = min(ans, i);
                ans = min(ans, n - i);
            }
        }
        return ans == INT_MAX ? -1 : ans;
    }
};
```

## 2516. Take K of Each Character From Left and Right

> :orange_circle:

字符串s由小写字母`a, b, c`组成。每分钟可以带走s的最左侧字符或最右侧字符。返回每个字符至少带走k个的最少时间，不可能则返回-1

### 方法

- 滑动窗口，`[j, i]`之间维护不需要带走的窗口，该窗口最大时，带走的最少。T: O(n), S: O(1)

```cpp
class Solution {
public:
    int takeCharacters(string s, int k) {
        int n = s.size(), cnt[3] = {0};
        for (char& c : s) cnt[c - 'a']++;
        if (cnt[0] < k || cnt[1] < k || cnt[2] < k) return -1;
        
        int i = 0, j = 0, ans = n;
        while (i < n) {
            cnt[s[i] - 'a']--;
            while (cnt[s[i] - 'a'] < k) {
                cnt[s[j] - 'a']++;
                j++;
            }
            ans = min(ans, n - (i - j + 1));
            i++;
        }
        return ans;
    }
};
```

## 2517. Maximum Tastiness of Candy Basket

> :orange_circle:

正整数数组代表每个糖果的价格。商店组合k个糖果打包成礼盒，礼盒的**甜蜜度**是礼盒中任两种糖果价格绝对差的最小值。返回礼盒的最大甜蜜度

### 方法

- 排序，对甜蜜度进行二分搜索，统计每种甜蜜度是否有k个以上糖果。T: O(n logm), S: O(1)

```cpp
class Solution {
public:
    int maximumTastiness(vector<int>& price, int k) {
        sort(price.begin(), price.end());
        int n = price.size();
        int l = 0, r = price[n - 1] - price[0];
        int ans = 0;
        while (l <= r) {
            int m = l + (r - l) / 2;
            int cnt = 1;
            for (int i = 1, j = 0; i < n; i++) {
                if (price[i] - price[j] >= m) {
                    cnt++;
                    j = i;
                }
            }
            if (cnt >= k) {
                ans = m;
                l = m + 1;
            } else r = m - 1;
        }
        return ans; // return l - 1;
    }
};
```

## 2518. Number of Great Partitions

> :red_circle:

给一个正整数数组和整数k，将数组分为两组，若每组的元素和大于等于k，则为好分区。返回不同的好分区数目，对`10^9 + 7`取余

### 方法