# Weekly Contest 323
> 2022.12.11

## 2500. Delete Greatest Value in Each Row

> :green_circle:

`mxn`二维正数矩阵，进行如下操作直至矩阵为空：删除每行中最大的值，将删除的最大值加入答案。每次操作后矩阵减少一列，返回得到的答案

### 方法

- 将每一行排好序，然后按照列取最大值，加入答案。T: O(M*NlogN), S: O(1)
- 从最大往最小加和，与从最小往最大加和，结果一样，因此升序与降序排列均可

```cpp
class Solution {
public:
    int deleteGreatestValue(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        for (auto& row : grid) sort(begin(row), end(row));
        int ans = 0;
        for (int j = 0; j < n; j++) {
            int maxRow = 0;
            for (int i = 0; i < m; i++) maxRow = max(maxRow, grid[i][j]);
            ans += maxRow;
        }
        return ans;
    }
};
```

## 2501. Longest Square Streak in an Array

> :orange_circle:

给整数数组，其子序列称做**方波**：长度至少为2，排序后后一个元素是前一个元素的平方。返回最长的**方波**的长度，没有则返回-1。`2 <= nums.length <= 10^5; 2 <= nums[i] <= 10^5`

### 方法

- 采用有序的哈希map记录元素及其作为方波结尾时的长度。从小到大遍历，如果一个数的平方数在map中，将其长度加上当前数的长度。T: O(nlogn), S: O(n)
- 原数组中可能有重复元素，map可以去重。同时根据平方数可以尽早终止遍历

```cpp
class Solution {
public:
    int longestSquareStreak(vector<int>& nums) {
        map<int, int> map;
        for (int num : nums) map[num] = 1;
        int maxVal = sqrt(map.rbegin()->first);
        int ans = 1;
        for (auto [k, v] : map) {
            if (k > maxVal) break;
            int n = k * k;
            if (map.find(n) != map.end()) {
                map[n] += v;
                ans = max(ans, map[n]);
            }
        }
        return ans == 1 ? -1 : ans;
    }
};
```

## 2502. Design Memory Allocator

> :orange_circle:

给一个整数`n`代表下标从0开始的内存数组的大小，设计内存分配器：**分配** `size`大小的连续空闲内存单元并赋予id为`mID`，**释放** id为`mID`的内存单元。`1 <= n, size, mID <= 1000`

### 方法一

- 使用有序map记录每个内存块的起始下标及终止下标（左开右闭，`[s, e) e-s=size`），使用无序map记录每个内存块的`mID`及对应的起始下标集合。
- 设内存块个数为M，则分配O(MlogM)，释放O(1)，空间O(M)

```cpp
class Allocator {
private:
    map<int, int> map; // All memory blocks: <startIndex, endIndex>, e.g. [s, e) e-s=size
    unordered_map<int, vector<int>> idMap; // <mID, vector: startIndex>
public:
    Allocator(int n) {
        map[n] = n;
    }
    
    int allocate(int size, int mID) {
        int start = 0;
        for (auto [k, v] : map) {
            if (k - start >= size) {
                map[start] = start + size;
                idMap[mID].push_back(start);
                return start;
            } else {
                start = v;
            }
        }
        return -1;
    }
    
    int free(int mID) {
        if (idMap.find(mID) == idMap.end()) return 0;
        int ans = 0;
        for (int s : idMap[mID]) {
            ans += (map[s] - s);
            map.erase(s);
        }
        idMap.erase(mID);
        return ans;
    }
};
```

### 方法二

- 直接采用一维数组：分配时从左至右遍历，寻找`size`个连续空闲内存，O(n)；释放时遍历一遍查找`mID`，O(n)。S: O(n)

```cpp
class Allocator {
public:
    Allocator(int n) {
        v = vector<int>(n);
    }
    
    int allocate(int size, int mID) {
        int start = -1, count = 0;
        for (int i = 0; i < v.size(); i++) {
            if (v[i] == 0) {
                if (count == 0) start = i;
                count++;
                if (count == size) {
                    for (int j = start; j <= i; j++) {
                        v[j] = mID;
                    }
                    return start;
                }
            } else {
                count = 0;
                start = -1;
            }
        }
        return -1;
    }
    
    int free(int mID) {
        int ans = 0;
        for (int i = 0; i < v.size(); i++) {
            if (v[i] == mID) {
                ans++;
                v[i] = 0;
            }
        }
        return ans;
    }
private:
    vector<int> v;
};
```

## 2503. Maximum Number of Points From Grid Queries

> :red_circle:
