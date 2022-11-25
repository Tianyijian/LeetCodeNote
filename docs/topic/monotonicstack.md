# 单调栈

## 介绍

- 使用场景：**通常是一维数组，要寻找任一个元素的右边或者左边第一个比自己大或者小的元素的位置**
- 以空间换时间，利用栈记录元素的大小顺序，时间O(n)，空间O(n)
- 从栈底到栈顶可以递减或递增，根据当前元素与栈顶元素的大小情况做出选择

## 0739. Daily Temperatures

> :orange_circle:

一维数组，寻找右边第一个比自己大的元素

### 方法
- 单调栈，寻找右边第一个比自己大的元素
- 维护单调递减栈（栈底到栈顶），当前元素较大时，栈顶较小的元素即可得到结果，弹出。当前元素入栈等待未来的更大元素
- 为了便于计算相隔天数（下标距离），栈中可以直接保存元素下标。T: O(n), S: O(n)

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        vector<int> res(temperatures.size(), 0);
        stack<int> st;
        for (int i = 0; i < temperatures.size(); i++) {
            while (!st.empty() && temperatures[i] > temperatures[st.top()]) {
                res[st.top()] = i - st.top();
                st.pop();
            }
            st.push(i);
        }
        return res;
    }
};
```

## 0496. Next Greater Element I

> :green_circle:

nums1是nums2的子数组，根据nums2寻找右边比自己大的第一个元素，按照nums1输出

### 方法
- 单调栈，寻找右侧第一个大于自己的元素
- 维护单调递减栈（栈底到栈顶），栈中为元素值，栈顶较小的元素可以得到结果，弹出，当前元素加入栈顶
- 由于元素是唯一的，将nums2的结果存入map中，赋值给nums1。为了节省空间，可以只记录在nums1中出现的值，因此先将nums1的（值，下标）存入map

```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> map;
        for (int i = 0; i < nums1.size(); i++) {
            map[nums1[i]] = i;
        }
        stack<int> st;
        vector<int> res(nums1.size(), -1);
        for (int i = 0; i < nums2.size(); i++) {
            while (!st.empty() && nums2[i] > st.top()) {
                if (map.find(st.top()) != map.end()) {
                    res[map[st.top()]] = nums2[i];
                }
                st.pop();
            }
            st.push(nums2[i]);
        }
        return res;
    }
};
```

## 0084. Largest Rectangle in Histogram

> :red_circle:

给定一个整数数组，代表柱状图中每个柱子的高度，其宽度都为1，找到柱状图中最大的矩形的面积

### 方法

- 单调递增栈（从栈底到栈顶），对每个柱子，找到左右两侧最近的更小的柱子位置，计算出构成矩阵的面积，保存最大值。T: O(n), S: O(n)
- **栈顶和栈顶的下一个元素以及要入栈的三个元素组成了我们要求最大面积的高度和宽度**

<!-- tabs:start -->

####**First**

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int maxArea = 0;
        stack<int> st;
        for (int i = 0; i <= heights.size(); i++) {
            while (!st.empty() && (i == heights.size() || heights[i] <= heights[st.top()])) {
                int mid = st.top();
                st.pop();
                int prevSmall = st.empty()? -1 : st.top();
                int area = (i - prevSmall - 1) * heights[mid];
                maxArea = max(maxArea, area);
            }
            st.push(i);
        }
        return maxArea;
    }
};
```

####**Second**

```cpp
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        int maxArea = 0;
        stack<int> st; 
        heights.insert(heights.begin(), 0); // 数组头部加入元素0
        heights.push_back(0); // 数组尾部加入元素0
        st.push(0);
        for (int i = 1; i < heights.size(); i++) {
            while (heights[i] < heights[st.top()]) {
                int mid = st.top();
                st.pop();
                int w = i - st.top() - 1;
                int area = heights[mid] * w;
                maxArea = max(maxArea, area);                    
            }
            st.push(i);
        }
        return maxArea;
    }
};
```

<!-- tabs:end -->

## 0907. Sum of Subarray Minimums

> :orange_circle:

给一个整数数组，计算其所有的子数组的最小值的和。因为答案较大，返回答案除以 `10^9 + 7`的余数

### 方法

- 暴力搜索：两层循环得到所有子数组，然后得到每个的最小值，计算总和
- 动态规划：存储`[i,j]`范围内的子串的最小值（MLE）。T: O(n^2), S: O(n^2)
- 对于每个元素，找到左侧最近的比其小的元素，右侧最近的比其小的元素，该范围内的所有子数组的最小值均为该元素
  - 下标为`[i, k, j]`，则该范围内包含元素k的子数组个数为`(k-i)*(j-k)`
- 单调递增栈（从栈底到栈顶为递增）可方便找到两侧最近的更小的元素。T: O(n), S:O(n)
  - 待加入栈的元素j小于等于栈顶元素k时，弹出栈顶元素k，则该元素的右侧最小为待加入元素j，左侧最小为此时的栈顶i（栈为空则-1）。注意为左开右闭，即`arr[i] < arr[k] <= arr[j]`，防止子数组重复计数
  - 最后栈中剩余的元素均没有右侧更小，其右侧更小下标为数组个数。

```cpp
class Solution {
public:
    int sumSubarrayMins(vector<int>& arr) {
        int n = arr.size();
        long res = 0;
        int MOD = 1e9 + 7;
        stack<int> st;
        for (int i = 0; i <= n; i++) {
            while (!st.empty() && (i == n || arr[i] <= arr[st.top()])) {
                int mid = st.top();
                st.pop();
                int prevSmall = st.empty()? -1 : st.top();
                long count = (mid - prevSmall) * (i - mid) % MOD;
                res += arr[mid] * count % MOD;
                res %= MOD;
            }
            st.push(i);
        }
        return res;
    }
};
```
