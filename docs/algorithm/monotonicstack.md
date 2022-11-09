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

