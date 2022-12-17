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

## 0150. Evaluate Reverse Polish Notation

> :orange_circle:

计算逆波兰表达式的值，有效操作符是 `+ - * /`。表达式有效，没有除以0的操作。`["2","1","+","3","*"] => ((2 + 1) * 3) = 9`

### 方法

- 使用栈存储数值。遇到运算符时，取出栈顶的两个数值进行运算，然后加入栈中。遇到数值直接入栈
- 逆波兰表达式：算式的后缀表达式。stoi(s): string转换为int

```cpp
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
        stack<long> st;
        for (string s : tokens) {
            if (s == "+" || s == "-" || s == "*" || s == "/") {
                long val2 = st.top();
                st.pop();
                long val1 = st.top();
                st.pop();
                if (s == "+") val1 += val2;
                else if (s == "-") val1 -= val2;
                else if (s == "*") val1 *= val2;
                else if (s == "/") val1 /= val2;
                st.push(val1);
            } else {
                st.push(stoi(s));
            }
        }
        return (int)st.top();
    }
};
```

