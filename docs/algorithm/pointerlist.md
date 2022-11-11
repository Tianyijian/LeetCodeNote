# 双指针解决链表问题

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

