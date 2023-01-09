# Weekly Contest 327
> 2023.01.08

## 2529. Maximum Count of Positive Integer and Negative Integer

> :green_circle:

给一个非递减的数组，返回正数和负数数量的最大值。`1 <= nums.length <= 2000; -2000 <= nums[i] <= 2000`

### 方法

- 遍历统计负数的个数和正数的个数，或者统计负数和0的个数，总数相减得到正数个数。T: O(n), S: O(1)
- 二分搜索：数组有序，采用二分搜索，直接查找正数和负数的下标位置，进而得到数量。T: O(logn), S: O(1)

```cpp
class Solution {
public:
    int maximumCount(vector<int>& nums) {
        int pos = nums.end() - upper_bound(nums.begin(), nums.end(), 0);
        int neg = lower_bound(nums.begin(), nums.end(), 0) - nums.begin();
        return max(pos, neg);
    }
};
```

## 2530. Maximal Score After Applying K Operations

> :orange_circle:

给一个整数数组，起始分为0。一次操作如下：选择一个下标i，分数加上`nums[i]`，将`nums[i]`替换为`ceil(nums[i] / 3)`。返回k次操作后最大可能的分数

### 方法

- 贪心思想，每次选择数组中最大的值加入分数即可。采用大顶堆动态维持。C++的堆在初始化时是线性时间O(n)，T: O(n+ klogn), S: O(n)

```cpp
class Solution {
public:
    long long maxKelements(vector<int>& nums, int k) {
        long long ans = 0;
        priority_queue<int> q(begin(nums), end(nums));
        while (k--) {
            int a = q.top();
            ans += a;
            q.pop();
            q.push((a + 2) / 3);
        }
        return ans;
    }
};
```

## 2531. Make Number of Distinct Characters Equal

> :orange_circle:

给两个字符串，一次移动交换两个字符串某个位置的字符。判断是否能通过一次移动使得两个字符串的不同字符数相等

### 方法

- 利用数组哈希统计两个字符串的各个字符数量，以及不同字符数。遍历每个已有字符，判断移动后不同字符数是否相等。主要判断清楚移动后是否添加或者减少了原有的不同字符数。T: O(m+n+26*26), S: O(26)

```cpp
class Solution {
public:
    bool isItPossible(string word1, string word2) {
        vector<int> hash1(26, 0), hash2(26, 0);
        for (char& c : word1) hash1[c - 'a']++;
        for (char& c : word2) hash2[c - 'a']++;
        int cnt1 = 0, cnt2 = 0;
        for (int i = 0; i < 26; i++) {
            if (hash1[i]) cnt1++;
            if (hash2[i]) cnt2++;
        }
        if (abs(cnt1 - cnt2) > 2) return false;
        for (int i = 0; i < 26; i++) {
            if (hash1[i] == 0) continue;
            for (int j = 0; j < 26; j++) {
                if (hash2[j] == 0) continue;
                int a = cnt1, b = cnt2;
                if (i != j) {
                    if (hash1[j] == 0) a++;
                    if (hash1[i] == 1) a--;
                    if (hash2[j] == 1) b--;
                    if (hash2[i] == 0) b++;
                }
                if (a == b) return true;
            }
        }
        return false;
    }
};
```

## 2532. Time to Cross a Bridge

> :red_circle:
