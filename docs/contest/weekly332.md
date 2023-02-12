# Weekly Contest 332
> 2023.02.12

## 2562. Find the Array Concatenation Value
> :green_circle:

两个数的拼接是连接其组成数字。给一个数组，拼接值初始为0。重复操作直至数组为空：依次将数组两端的元素拼接，将值加入拼接值，删除这两个元素；最后剩一个元素时，直接加入拼接值。返回数组的拼接值。

### 方法一

- 从两端遍历数组，将两个数转成字符串，拼接后转为数值，相加得到结果。注意处理数组长度为奇数和偶数。

```cpp
class Solution {
public:
    long long findTheArrayConcVal(vector<int>& nums) {
        long long ans = 0;
        int n = nums.size();
        for (int i = 0; i < n / 2; i++) {
            string str = to_string(nums[i]) + to_string(nums[n - 1 - i]);
            ans += stoi(str);
        }
        if (n % 2) ans += nums[n / 2];
        return ans;
    }
};
```

### 方法二

- 从两端遍历数组，利用pow和log10得到拼接后的数字，避免字符串操作
- 其它方法：将第二个数字转为字符串得到位数，利用pow计算数值，相加；将第二个数组转为字符串，读入每一位相加

```cpp
class Solution {
public:
    long long findTheArrayConcVal(vector<int>& nums) {
        long long ans = 0;
        int n = nums.size();
        for (int i = 0, j = n - 1; i <= j; i++, j--) {
            if (i < j) ans += nums[i] * pow(10, (int)log10(nums[j]) + 1) + nums[j];
            else ans += nums[i];
        }
        return ans;
    }
};
```

## 2563. Count the Number of Fair Pairs

> :orange_circle:

给一个整数数组和两个值`lower`与`upper`，返回**公平对**的数量。**公平对**`(i, j)`满足`0 <= i < j < n,lower <= nums[i] + nums[j] <= upper `

### 方法

- `nums[i] + nums[j] == nums[j] + nums[i]`，因此`i < j`可以视为`i != j`，但要避免既选择`i, j` 又选择 `j, i`。
- 将数组排序，对于每个数`nums[i]`，通过二分查找在下标`[i+1, n]`中找到满足要求的下标范围`lower-nums[i] =< nums[j] <= upper-nums[i]`。T: O(nlogn), S: O(1)

```cpp
class Solution {
public:
    long long countFairPairs(vector<int>& nums, int lower, int upper) {
        long long ans = 0;
        sort(nums.begin(), nums.end());
        for (int i = 0; i < nums.size(); i++) {
            auto high = upper_bound(nums.begin() + i + 1, nums.end(), upper - nums[i]);
            auto low = lower_bound(nums.begin() + i + 1, nums.end(), lower - nums[i]);
            ans += high - low;
        }
        return ans;
    }
};
```

## 2564. Substring XOR Queries
> :orange_circle:

给一个二进制字符串s，二维数组queries。对`queries[i] = [firsti, secondi]`，找到s中最短的子串，其数值为val，满足`val ^ firsti == secondi`，返回子串的端点`ans[i] = [lefti, righti]`，没有这样的子串返回`[-1, -1]`，有多个子串返回`lefti`最小的。`0 <= firsti, secondi <= 10^9`

### 方法

- `val == firsti ^ secondi`，`2^30 = 1073741824 > 10^9`，因此子串最多有三十位。
- 将s中所有三十位以内的子串的值以及最好的端点记录到map中，对每个query直接查找即可。T: O(30n), S: O(30n)
- 子串中`0`开头只需记录第一个`0`的端点即可，`1`开头的，第一个数值即为最好的端点，之后遇见不再记录

```cpp
class Solution {
public:
    vector<vector<int>> substringXorQueries(string s, vector<vector<int>>& queries) {
        unordered_map<int, vector<int>> map;
        int n = s.size();
        for (int i = 0; i < s.size(); i++) {
            int val = 0;
            for (int j = i; j < min(n, i + 30); j++) {
                val = (val << 1) + (s[j] - '0');
                if (!map.count(val)) map[val] = {i, j};
                if (val == 0) break;
            }
        }
        vector<vector<int>> ans;
        for (auto& q : queries) {
            int val = q[0] ^ q[1];
            if (map.count(val)) ans.push_back(map[val]);
            else ans.push_back({-1, -1});
        }
        return ans;
    }
};
```

## 2565. Subsequence With the Minimum Score
> :red_circle:

