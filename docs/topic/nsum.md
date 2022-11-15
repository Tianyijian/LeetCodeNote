# nSum问题

## 介绍

- 给定一个数组nums和一个目标和target，从nums数组中选择n个数，使数字和为target
- 返回结果为数值的情况下，可以对数组进行排序，采用左右指针的搜索方法
- 返回结果为下标的情况下，若数组无序，则不能排序加双指针，考虑哈希表降低复杂度
- 解唯一时可以用哈希表，当有多个解且需要去重时，哈希表去重较麻烦，双指针更易去重
- 学习资源
  - https://mp.weixin.qq.com/s/fSyJVvggxHq28a0SdmZm6Q


## 0001. Two Sum

> :green_circle:

给定数组nums和目标和target，找到和为target的两个数，返回其下标。设结果唯一，元素不能重复使用`-10^9 <= nums[i] <= 10^9`

### 方法

- 数组无序，且有正负数，所求结果为下标，不宜采用排序加双指针的方法
- 暴力求解需要O(n^2)，采用哈希表记录数据及其下标，寻找是否存在num2 = target - num1，并返回下标即可。T: O(n), S: O(n)
- 判断出现map.count(key)，map.find(key) != map.end() ; 取值map.at(key), map[key]
- 添加值 map.insert(pair<int, int>(key, value))，map[key] = value

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> map;
        for (int i = 0; i < nums.size(); i++) {
            int num2 = target - nums[i];
            if (map.find(num2) != map.end()) {
                return {i, map[num2]};
            }
            map[nums[i]] = i;
        }
        return {};
    }
};
```

## 0167. Two Sum II - Input Array Is Sorted

> :orange_circle:

给定下标从1开始的非减数组，找到和为target的两个值，返回这两个值的下标，解唯一。`-1000 <= numbers[i] <= 1000`

### 方法一

- 采用哈希表减少一层循环，降低复杂度。T: O(n), S: O(n)

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        unordered_map<int, int> map;
        for (int i = 0; i < numbers.size(); i++) {
            int num2 = target - numbers[i];
            if (map.find(num2) != map.end()) return {map[num2], i + 1};
            map[numbers[i]] = i + 1;
        }
        return {};
    }
};
```

### 方法二

- 所求结果为下标，但数组本身有序，可以采用双指针进行搜索
- 左右指针相向而行，因为数组升序，所以当前和大时，右指针左移，当前和小时，左指针右移。T: O(n), S: O(1)

```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& numbers, int target) {
        int left = 0, right = numbers.size() - 1;
        while (left <= right) {
            int sum = numbers[left] + numbers[right];
            if (sum > target) right--;
            else if (sum < target) left++;
            else return {left + 1, right + 1};
        }
        return {-1, -1};
    }
};
```

## 2Sum

给定一维数组nums和目标和target，返回所有二元组使得`nums[i] + nums[j] = target`，`i != j`且结果没有重复元组

### 方法

- 对数组进行排序，采用双指针的思想，并去重。双指针遍历为O(n)，但排序为O(nlogn)，T: O(nlogn), S: O(1)

```cpp
vector<vector<int>> twoSumTarget(vector<int>& nums, int target) {
    sort(nums.begin(), nums.end());
    int lo = 0, hi = nums.size() - 1;
    vector<vector<int>> res;
    while (lo < hi) {
        int sum = nums[lo] + nums[hi];
        int left = nums[lo], right = nums[hi];
        if (sum < target) {
            while (lo < hi && nums[lo] == left) lo++;
        } else if (sum > target) {
            while (lo < hi && nums[hi] == right) hi--;
        } else {
            res.push_back({left, right});
            while (lo < hi && nums[lo] == left) lo++;
            while (lo < hi && nums[hi] == right) hi--;
        }
    }
    return res;
}
```

## 0015. 3Sum

> :orange_circle:

给定一维整数数组nums，返回所有和为0的三元组`[nums[i], nums[j], nums[k]]`，`i != j != k`且结果没有重复元组

### 方法一

- 暴力搜索是O(n^3)，利用哈希表记录数值可以去掉一层循环，变为O(n^2)，但去重较为麻烦
- 通过构建map，value记录个数去重，利用key的顺序，得到两数加和的所有情况，再根据顺序判断第三个数是否符合要求

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        // construct map, get key array, key_to_id
        unordered_map<int, vector<int>> map;
        vector<int> key;
        unordered_map<int, int> key_to_id;
        for (int i = 0; i < nums.size(); i++) {
            map[nums[i]].push_back(i);
        }
        for (auto kv:map) {
            key.push_back(kv.first);
        }
        for (int i = 0; i < key.size(); i++) {
            key_to_id[key[i]] = i;
        }
        // get two sum, 
        vector<vector<int>> res;
        int num_i, num_j, num_k;    // [a,b,c]
        for (int i = 0; i < key.size(); i++) {
            for (int j = i; j < key.size(); j++) {      // j from i, reserve [a=b], remove duplication [a,b] = [b,a]
                if (i == j && map[key[i]].size() < 2) {
                    continue;
                }
                num_i = key[i];
                num_j = key[j];
                num_k = 0 - num_i - num_j;
                if (key_to_id.find(num_k) != key_to_id.end()) {  // c in key
                    // when a != b, index_c must > index_a and index_b;
                    // when a == b, index_c arbitrary
                    if (num_i != num_j && (key_to_id[num_k] <= key_to_id[num_i] || key_to_id[num_k] <= key_to_id[num_j])) {
                        continue;
                    }
                    if (num_i == num_j && num_j == num_k && map[num_i].size() < 3) {   //a = b = c, [0, 0, 0]
                        continue;
                    }
                    res.push_back({num_i, num_j, num_k});
                }
            }
        }
        return res;
    }
};
```

### 方法二

- 采用数组排序，双指针的方法进行元素查找，并且双指针移动进行去重。T: O(n^2), S: O(1)

```cpp
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        int left, right, sum; //[a = nums[i], b = nums[left], c = nums[right]], sum = a + b + c 
        for (int i = 0; i < nums.size(); i++) { 
            if (nums[i] > 0) return res; // when a>0, return 
            if (i > 0 && nums[i] == nums[i - 1]) continue; //deduplicate a
            left = i + 1;
            right = nums.size() - 1;
            while (left < right) {
                sum = nums[i] + nums[left] + nums[right];
                if (sum > 0) right--;
                else if (sum < 0) left++;
                else {
                    res.push_back({nums[i], nums[left], nums[right]});
                    while (left < right && nums[left] == nums[left + 1]) left++; //deduplicate b
                    while (left < right && nums[right] == nums[right - 1]) right--; //deduplicate c
                    left++;
                    right--;
                }
            }
        }
        return res;
    }
};
```

## 0018. 4Sum

> :orange_circle:

给定一维数组nums和目标和target，返回所有四元组使得`nums[a] + nums[b] + nums[c] + nums[d] == target`，abcd互不相同且结果没有重复元组

### 方法

- 在3Sum的基础上增加了一个维度，且target没有固定为0。T: O(n^3), S: O(1)

```cpp
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        vector<vector<int>> res;
        sort(nums.begin(), nums.end());
        for (int a = 0; a < nums.size(); a++) {
            if (nums[a] > target && nums[a] >= 0) break; //target < 0
            if (a > 0 && nums[a] == nums[a - 1]) continue;
            for (int b = a + 1; b < nums.size(); b++) {
                if (nums[b] + nums[a] > target && nums[b] + nums[a] >= 0) break;
                if (b > a + 1 && nums[b] == nums[b - 1]) continue;
                int c = b + 1;
                int d = nums.size() - 1;
                while (c < d) {
                    long sum = nums[a] + nums[b]; // overflow
                    sum += nums[c] + nums[d];
                    if (sum > target) d--;
                    else if (sum < target) c++;
                    else {
                        res.push_back(vector<int>{nums[a], nums[b], nums[c], nums[d]});
                        while (c < d && nums[c] == nums[c + 1]) c++;
                        while (c < d && nums[d] == nums[d - 1]) d--;
                        c++;
                        d--;
                    }
                }
            }
        }
        return res;
    }
};
```

## nSum

给定一维数组nums和目标和target，返回所有和为target的n元组，元组中每个元素的下标互不相同且结果没有重复元组

### 方法

```cpp
/* 注意：调用这个函数之前一定要先给 nums 排序 */
vector<vector<int>> nSumTarget(
    vector<int>& nums, int n, int start, int target) {

    int sz = nums.size();
    vector<vector<int>> res;
    // 至少是 2Sum，且数组大小不应该小于 n
    if (n < 2 || sz < n) return res;
    // 2Sum 是 base case
    if (n == 2) {
        // 双指针那一套操作
        int lo = start, hi = sz - 1;
        while (lo < hi) {
            int sum = nums[lo] + nums[hi];
            int left = nums[lo], right = nums[hi];
            if (sum < target) {
                while (lo < hi && nums[lo] == left) lo++;
            } else if (sum > target) {
                while (lo < hi && nums[hi] == right) hi--;
            } else {
                res.push_back({left, right});
                while (lo < hi && nums[lo] == left) lo++;
                while (lo < hi && nums[hi] == right) hi--;
            }
        }
    } else {
        // n > 2 时，递归计算 (n-1)Sum 的结果
        for (int i = start; i < sz; i++) {
            vector<vector<int>> 
                sub = nSumTarget(nums, n - 1, i + 1, target - nums[i]);
            for (vector<int>& arr : sub) {
                // (n-1)Sum 加上 nums[i] 就是 nSum
                arr.push_back(nums[i]);
                res.push_back(arr);
            }
            while (i < sz - 1 && nums[i] == nums[i + 1]) i++;
        }
    }
    return res;
}
```

## 454. 4Sum II
> :orange_circle:

