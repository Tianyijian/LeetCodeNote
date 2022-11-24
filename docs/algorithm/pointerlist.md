# 双指针解决链表问题


## 0021. Merge Two Sorted Lists

> :green_circle:

合并两个有序链表

### 方法一

*  定义新的链表节点，将list1和list2依次加上去。T: O(n), S: O(1)

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* dummy = new ListNode(-1);
        ListNode* p = dummy;
        while (list1 && list2) {
            if (list1->val < list2->val) {
                p->next = list1;
                list1 = list1->next;
            } else {
                p->next = list2;
                list2 = list2->next;
            }
            p = p->next;
        }
        if (list1) p->next = list1;
        if (list2) p->next = list2;
        return dummy->next;
    }
};
```
### 方法二

*  将list1直接加到list2上，较为麻烦

```cpp
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* cur1 = new ListNode(0, list1);
        ListNode* cur2 = new ListNode(0, list2);
        ListNode* head = cur2;
        while (cur1->next && cur2->next) {
            if (cur1->next->val < cur2->next->val) {
                ListNode* temp = cur2->next;
                cur2->next = cur1->next;
                cur1->next = cur1->next->next;
                cur2->next->next = temp;
            } 
            cur2 = cur2->next;
        }
        if (cur1->next) cur2->next = cur1->next;
        return head->next;
    }
};
```

## 0086. Partition List

> :orange_circle:

给一个链表以及值x，分割链表，使小于x的节点出现在大于等于x的节点之前

### 方法

- 按要求将一个链表分解为两个小链表，然后拼接。T: O(n), S: O(1)

```cpp
class Solution {
public:
    ListNode* partition(ListNode* head, int x) {
        ListNode* dummy1 = new ListNode(0);
        ListNode* dummy2 = new ListNode(0);
        ListNode* p1 = dummy1;
        ListNode* p2 = dummy2;
        ListNode* cur = head;
        while (cur) {
            if (cur->val < x) {
                p1->next = cur;
                p1 = p1->next;
            } else {
                p2->next = cur;
                p2 = p2->next;
            }
            cur = cur->next;
        }
        p2->next = NULL;
        p1->next = dummy2->next;
        return dummy1->next;
    }
};
```

## 0023. Merge k Sorted Lists

> :red_circle: 

将k个升序链表合并为一个有序链表

### 方法

- 方法一：链表的归并排序，两两合并。T: O(Nlogk) S: O(1)
* 方法二：从K个节点中选择最小的，维护一个包含k个元素的小顶堆。T: O(Nlogk) S:O(k)

```cpp
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        return mergeSort(lists, 0, lists.size());
    }

private:
    ListNode* mergeSort(vector<ListNode*> lists, int left, int right) {
        if (left >= right) return nullptr;
        else if (left == right - 1) return lists[left];
        int mid = left + (right - left) / 2;
        ListNode* leftList = mergeSort(lists, left, mid);
        ListNode* rightList = mergeSort(lists, mid, right);
        return merge(leftList, rightList);
    }

    ListNode* merge(ListNode* list1, ListNode* list2) {
        ListNode* dummy = new ListNode(0);
        ListNode* p = dummy;
        while (list1 && list2) {
            if (list1->val < list2->val) {
                p->next = list1;
                list1 = list1->next;
            } else {
                p->next = list2;
                list2 = list2->next;
            }
            p = p->next;
        }
        if (list1) p->next = list1;
        if (list2) p->next = list2;
        p = dummy->next;
        delete dummy;
        return p;
    }
};
```

## 0019. Remove Nth Node From End of List

> :orange_circle:

删除链表的倒数第n个节点

### 方法

- 两遍扫描：先获得size，再根据index删除节点
- 一遍扫描：双指针：快指针领先慢指针n个，同时移动直至快指针到末尾。T: O(n), S: O(1)

```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dummy = new ListNode(0, head);
        ListNode* fast = dummy;
        ListNode* slow = dummy;
        while (n--) fast = fast->next;
        while (fast->next) {
            fast = fast->next;
            slow = slow->next;
        }
        ListNode* temp = slow->next;
        slow->next = slow->next->next;
        delete temp;
        return dummy->next;
    }
};
```

## 0876. Middle of the Linked List

> :green_circle:

返回链表的中间节点，如果中间有两个，返回第二个节点

### 方法

- 双指针法：一遍扫描，快指针每次移动两步，慢指针每次移动一步。T: O(n), S: O(1)

```cpp
class Solution {
public:
    ListNode* middleNode(ListNode* head) {
        ListNode* last = head;
        ListNode* middle = head;
        while (last && last->next) {
            last = last->next->next;
            middle = middle->next;
        }
        return middle;
    }
};
```

## 0141. Linked List Cycle

> :green_circle:

判断一个链表是否有环

### 方法

- 快慢指针，快指针每次走两步，满指针每次走一步，快指针追上慢指针，有环。T: O(n), S: O(1)

```cpp
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode *slow = head;
        ListNode *fast = head;
        while (fast && fast->next) {
            fast = fast->next->next;
            slow = slow->next;
            if (fast == slow) return true;
        }
        return false;
    }
};
```

## 0142. Linked List Cycle II

> :orange_circle:

给定一个链表，如果有环返回环的入口，没有环则返回null

### 方法

- 快指针以恒定速度快于慢指针，相遇即代表有环。
- `2(x+y) = x+y+n(y+z), x=(n-1)(y+z)+z `，相遇点与头节点同时设置指针进行移动，相遇即为环入口

```cpp
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* fast = head;
        ListNode* slow = head;
        while (1) {
            if (fast == NULL || fast -> next == NULL) {
                return NULL;
            }
            fast = fast -> next -> next;
            slow = slow -> next;
            if (fast == slow) {
                ListNode* cur = head;
                while (cur != slow) {
                    cur = cur -> next;
                    slow = slow -> next;
                }
                return cur;
            }
        }
    }
};
```

## 0160. Intersection of Two Linked Lists

> :green_circle:

给定两个可能相交的链表，若相交返回相交的节点，不相交则返回null

### 方法一

- 双指针，找到两个列表长度差k，先移动快指针k步，然后同时移动

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* curA = headA;
        ListNode* curB = headB;
        // get size 
        int sizeA = 0, sizeB = 0;
        while (curA != NULL) {
            sizeA++;
            curA = curA -> next;
        }
        while (curB != NULL) {
            sizeB++;
            curB = curB -> next;
        }
        // move one by size distance
        curA = headA;
        curB = headB;
        int dis = sizeA - sizeB;
        if (dis > 0) {
            while (dis--) {
                curA = curA -> next;
            }
        } else {
            dis = sizeB - sizeA;
            while (dis--) {
                curB = curB -> next;
            }
        }
        // find intersected node
        while (curB != NULL) {
            if (curA == curB) {
                return curA;
            }
            curA = curA -> next;
            curB = curB -> next;
        }
        return NULL;
    }
};
```

### 方法二

- 双指针，P1遍历完A接着遍历B，P2遍历完B接着遍历A（逻辑连接A和B）
  	* 若两者不相交，最后都为NULL，停止循环

```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* p1 = headA;
        ListNode* p2 = headB;
        while (p1 != p2) {
            if (p1 == NULL) p1 = headB;
            else p1 = p1->next;
            if (p2 == NULL) p2 = headA;
            else p2 = p2->next;
        }
        return p1;
    }
};
```

## 0206. Reverse Linked List

> :green_circle:

给定单链表的头结点，返回反转后的链表

### 方法一

- 迭代逆序链表，采用双指针法。临时记录当前节点的下一节点，当前节点指向前一节点。
- 初始时，可以直接将前一节点设为NULL，从而也可逆转head节点。T: O(n), S: O(1)

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* pre = NULL;
        ListNode* cur = head;
        ListNode* temp;
        while (cur) {
            temp = cur->next;
            cur->next = pre;
            pre = cur;
            cur = temp;
        }
        return pre;
    }
};
```

### 方法二

- 递归逆序链表，从前往后反转指针，类似双指针解法。T: O(n), S: O(1)

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        return reverse(NULL, head);
    }
private:
    ListNode* reverse(ListNode* pre, ListNode* cur) {
        if (cur == NULL) return pre;
        ListNode* temp = cur->next;
        cur->next = pre;
        return reverse(cur, temp);
    }
};
```
### 方法三

* 递归逆序链表，从后往前反转指针。T: O(n), S: O(1)
* 注意始终返回最后一个节点，且直接可通过head->next->next=head进行翻转

<!-- tabs:start -->
####**First**

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !head->next) return head;
        ListNode* last = reverseList(head->next);
        head->next->next = head;
        head->next = NULL;
        return last;
    }
};
```
####**Second**
```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (!head || !head->next) return head;
        ListNode* node = reverseList(head->next);
        ListNode* temp = node;
        while (temp->next) temp = temp->next;
        temp->next = head;
        head->next = NULL;
        return node;
    }
};
```
<!-- tabs:end -->

### 方法四

- 设立新数组，将数据取出，逆序构建链表，浪费时间和内存。T: O(n), S: O(n)

```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (head == NULL) return head;
        // Get data to a array
        vector<int> data;
        ListNode* cur = head;
        while (cur != nullptr) {
            data.push_back(cur -> val);
            cur = cur -> next;
        }
        // Create reverse list
        int size = data.size();
        ListNode* res = new ListNode(data[size - 1]);
        cur = res;
        for (int i = size - 2; i >= 0; i--) {
            ListNode* tmp = new ListNode(data[i]);
            cur -> next = tmp;
            cur = tmp;
        }
        return res;
    }
};
```

## 0025. Reverse Nodes in k-Group

> :red_circle:

K个一组反转链表，多余的节点保持不变

### 方法一

- 双指针反转链表，先遍历得到链表的长度，然后K个一组进行反转
- reverseList() 函数从当前节点开始反转K个节点，并返回这K个节点的头节点和尾节点

```cpp
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        if (k == 1 || head->next == NULL) return head;
        ListNode* cur = head;
        int size = 0;
        while (cur) {
            cur = cur->next;
            size++;
        }
        ListNode* dummy = new ListNode(0, head);
        cur = dummy;
        for (int i = k; i <= size; i += k) {
            vector<ListNode*> nodes = reverseList(cur->next, k);
            cur->next = nodes[0];
            cur = nodes[1];
        }
        return dummy->next;
    }
private:
    vector<ListNode*> reverseList(ListNode* head, int k) {
        ListNode* prev = head;
        ListNode* cur = prev->next;
        while (--k) {
            ListNode* temp = cur->next;
            cur->next = prev;
            prev = cur;
            cur = temp;
        }
        head->next = cur;
        return {prev, head};
    }
};
```

### 方法二

- 双指针反转链表，先找k个节点之后的尾结点，没有则直接返回，反转这K个节点，反转之后的节点
- reverseList() 函数反转tail节点之前的链表，[head, tail)，返回新链表的头结点

```cpp
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        // 1. find tail node for the current group
        ListNode* cur = head;
        for (int i = 0; i < k; i++) {
            if (!cur) return head;
            cur = cur->next;
        }
        // 2. reverse the node for the current group
        ListNode* newHead = reverseList(head, cur);
        // 3. reverse the nodes for the next group
        head->next = reverseKGroup(cur, k);
        return newHead;
    }
private:
    // reverse nodes from head to tail. [head, tail)
    ListNode* reverseList(ListNode* head, ListNode* tail) {
        ListNode *prev = tail, *cur = head;
        while (cur != tail) {
            ListNode* temp = cur->next;
            cur->next = prev;
            prev = cur;
            cur = temp;
        }
        return prev;
    }
};
```

