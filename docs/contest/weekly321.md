# Weekly Contest 321
> 2022.11.27

## 2485. Find the Pivot Integer
> :green_circle:

给一个正整数n，找到基准整数x满足：`sum([1, x]) = sum([x, n])`。不存在返回-1，输入保证最多一个基准元素

### 方法一

- 前缀和，计算出总和，从左到右遍历计算前缀和，相减得到后部分和，从而确认基准的位置。T: O(n), S: O(1)

```cpp
class Solution {
public:
    int pivotInteger(int n) {
        int sum = (n + 1) * n / 2;
        int lSum = 0;
        for (int i = 1; i <= n; i++) {
            if (lSum + i == sum - lSum) {
                return i;
            }
            lSum += i;
        }
        return -1;
    }
};
```

### 方法二

- 数学公式，连续整数的和可通过等差数列公式计算：`sum[1, x]=(1+x)*x/2`，直接从左到右遍历找到基准即可。T: O(n), S: O(1)
- 二分搜索：数组有序，可以通过二分搜索查找满足条件的基准。T: O(logn), S: O(1)

```cpp
class Solution {
public:
    int pivotInteger(int n) {
        for (int i = 1; i <= n; i++) {
            if ((1 + i) * i == (i + n) * (n - i + 1)) {
                return i;
            }
        }
        return -1;
    }
};
```

### 方法三

- 数学，`(1+x)*x/2 = (x+n)*(n-x+1)/2`可得，`x*x = (n+1)n/2`，可直接判断是否存在这样的x。T: O(1), S: O(1)

```cpp
class Solution {
public:
    int pivotInteger(int n) {
        int sum = (n + 1) * n / 2, x = sqrt(sum); 
        return x * x == sum ? x : -1;
    }
};
```

## 2486. Append Characters to String to Make Subsequence

> :orange_circle:

字符串s和t都由小写字母组成，返回最少的需要加在s末尾的字符数量，使得t成为s的子序列

### 方法

- 在s中顺序查找t中的字符，t中剩下的字符就是需要添加的字符。T: O(n), S:O(1)

```cpp
class Solution {
public:
    int appendCharacters(string s, string t) {
        int j = 0;
        for (char c : s) {
            if (c == t[j]) j++;
            if (j == t.size()) return 0;
        }
        return t.size() - j;
    }
};
```

## 2487. Remove Nodes From Linked List

> :orange_circle:

给一个链表，如果一个节点右侧有一个严格更大的值，则移除该节点，返回最终的链表

### 方法

- 单调递减栈（栈底到栈顶递减），从左到右遍历，每次遇到较大值时，弹出栈中的小值，栈中剩余节点构成最终链表。T: O(n), S: O(n)
  - 栈也可以顺序遍历，`for(auto& p : st) node = node->next = p;`
- 简洁写法：虚拟头节点入栈，每当有元素入栈时，更改栈顶元素的指向。vector可以当作栈使用
- 方法二：将链表逆序，如果下一个节点值小于当前节点，删除下一节点，然后再逆序链表

<!-- tabs:start -->

####**First**

```cpp
class Solution {
public:
    ListNode* removeNodes(ListNode* head) {
        stack<ListNode*> st;
        ListNode* cur = head;
        while (cur) {
            while (!st.empty() && cur->val > st.top()->val) {
                st.pop();
            }
            st.push(cur);
            cur = cur->next;
        }
        ListNode* dummy = new ListNode(0);
        while (!st.empty()) {
            st.top()->next = dummy->next;
            dummy->next = st.top();
            st.pop();
        }
        return dummy->next;
    }
};
```

####**Second**

```cpp
class Solution {
public:
    ListNode* removeNodes(ListNode* head) {
        ListNode* dummy = new ListNode(INT_MAX);
        vector<ListNode*> st = {dummy};
        for (auto& p = head; p != nullptr; p = p->next) {
            while (st.back()->val < p->val) {
                st.pop_back();
            }
            st.back()->next = p;
            st.push_back(p);
        }
        return dummy->next;
    }
};
```

<!-- tabs:end -->

## 2488. Count Subarrays With Median K

> :red_circle:

