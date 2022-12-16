# 栈与队列

## 0232. Implement Queue using Stacks

> :green_circle:

使用两个栈模拟队列的先进先出。(`push peek pop empty`)

### 方法

- 一个栈直接装入元素，需要弹出或查看队首时，将第一个栈全部弹出并装入第二个栈。T: O(1), S: O(n)
- void stack.pop()，无返回值；stack.top()返回栈顶值

```cpp
class MyQueue {
public:
    MyQueue() {
        
    }
    
    void push(int x) {
        inSt.push(x);
    }
    
    int pop() {
        if (outSt.empty()) {
            while (!inSt.empty()) {
                outSt.push(inSt.top());
                inSt.pop();
            }
        }
        int top = outSt.top();
        outSt.pop();
        return top;
    }
    
    int peek() {
        if (outSt.empty()) {
            while (!inSt.empty()) {
                outSt.push(inSt.top());
                inSt.pop();
            }
        }
        return outSt.top();
    }
    
    bool empty() {
        return inSt.empty() && outSt.empty();
    }
private:
    stack<int> inSt;
    stack<int> outSt;
};
```

