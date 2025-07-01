# Chapter 9: Stacks and Queues - The Order Keepers

## Table of Contents
- [Introduction to Linear Data Structures](#introduction)
- [Stack Implementation and Operations](#stack-implementation)
- [Queue Implementation and Operations](#queue-implementation)
- [Advanced Stack Techniques](#advanced-stack)
- [Advanced Queue Techniques](#advanced-queue)
- [Classic Problems with Three Approaches](#classic-problems)
- [Monotonic Stack/Queue Mastery](#monotonic)
- [Applications and Real-World Usage](#applications)
- [Practice Problems](#practice-problems)

## Introduction to Linear Data Structures {#introduction}

Stacks and Queues are fundamental linear data structures that enforce specific ordering rules:
- **Stack**: Last In, First Out (LIFO) - like a pile of plates
- **Queue**: First In, First Out (FIFO) - like a line at a store

### Why They Matter in DSA
- **Algorithm Building Blocks**: Used in DFS, BFS, expression evaluation
- **Memory Management**: Function call stack, undo operations
- **Real-World Applications**: Browser history, process scheduling
- **Interview Favorites**: Extremely common in coding interviews

## Stack Implementation and Operations {#stack-implementation}

### Array-Based Stack
```cpp
template<typename T>
class ArrayStack {
private:
    T* arr;
    int capacity;
    int topIndex;

public:
    ArrayStack(int size = 100) : capacity(size), topIndex(-1) {
        arr = new T[capacity];
    }
    
    ~ArrayStack() {
        delete[] arr;
    }
    
    // Push element - O(1)
    void push(T element) {
        if (topIndex >= capacity - 1) {
            throw overflow_error("Stack Overflow");
        }
        arr[++topIndex] = element;
    }
    
    // Pop element - O(1)
    T pop() {
        if (isEmpty()) {
            throw underflow_error("Stack Underflow");
        }
        return arr[topIndex--];
    }
    
    // Peek top element - O(1)
    T top() {
        if (isEmpty()) {
            throw underflow_error("Stack is empty");
        }
        return arr[topIndex];
    }
    
    // Check if empty - O(1)
    bool isEmpty() {
        return topIndex == -1;
    }
    
    // Get size - O(1)
    int size() {
        return topIndex + 1;
    }
    
    // Display stack
    void display() {
        for (int i = topIndex; i >= 0; i--) {
            cout << arr[i] << " ";
        }
        cout << "\n";
    }
};
```

### Dynamic Array Stack (Resizable)
```cpp
template<typename T>
class DynamicStack {
private:
    vector<T> arr;

public:
    void push(T element) {
        arr.push_back(element);
    }
    
    T pop() {
        if (isEmpty()) {
            throw underflow_error("Stack Underflow");
        }
        T element = arr.back();
        arr.pop_back();
        return element;
    }
    
    T top() {
        if (isEmpty()) {
            throw underflow_error("Stack is empty");
        }
        return arr.back();
    }
    
    bool isEmpty() {
        return arr.empty();
    }
    
    int size() {
        return arr.size();
    }
};
```

### Linked List-Based Stack
```cpp
template<typename T>
class LinkedStack {
private:
    struct Node {
        T data;
        Node* next;
        Node(T val) : data(val), next(nullptr) {}
    };
    
    Node* head;
    int stackSize;

public:
    LinkedStack() : head(nullptr), stackSize(0) {}
    
    ~LinkedStack() {
        while (!isEmpty()) {
            pop();
        }
    }
    
    void push(T element) {
        Node* newNode = new Node(element);
        newNode->next = head;
        head = newNode;
        stackSize++;
    }
    
    T pop() {
        if (isEmpty()) {
            throw underflow_error("Stack Underflow");
        }
        
        T data = head->data;
        Node* temp = head;
        head = head->next;
        delete temp;
        stackSize--;
        return data;
    }
    
    T top() {
        if (isEmpty()) {
            throw underflow_error("Stack is empty");
        }
        return head->data;
    }
    
    bool isEmpty() {
        return head == nullptr;
    }
    
    int size() {
        return stackSize;
    }
};
```

## Queue Implementation and Operations {#queue-implementation}

### Array-Based Circular Queue
```cpp
template<typename T>
class CircularQueue {
private:
    T* arr;
    int capacity;
    int front, rear, count;

public:
    CircularQueue(int size = 100) : capacity(size), front(0), rear(-1), count(0) {
        arr = new T[capacity];
    }
    
    ~CircularQueue() {
        delete[] arr;
    }
    
    // Enqueue (add to rear) - O(1)
    void enqueue(T element) {
        if (isFull()) {
            throw overflow_error("Queue Overflow");
        }
        rear = (rear + 1) % capacity;
        arr[rear] = element;
        count++;
    }
    
    // Dequeue (remove from front) - O(1)
    T dequeue() {
        if (isEmpty()) {
            throw underflow_error("Queue Underflow");
        }
        T element = arr[front];
        front = (front + 1) % capacity;
        count--;
        return element;
    }
    
    // Peek front element - O(1)
    T frontElement() {
        if (isEmpty()) {
            throw underflow_error("Queue is empty");
        }
        return arr[front];
    }
    
    // Check if empty - O(1)
    bool isEmpty() {
        return count == 0;
    }
    
    // Check if full - O(1)
    bool isFull() {
        return count == capacity;
    }
    
    // Get size - O(1)
    int size() {
        return count;
    }
    
    // Display queue
    void display() {
        if (isEmpty()) {
            cout << "Queue is empty\n";
            return;
        }
        
        int i = front;
        for (int j = 0; j < count; j++) {
            cout << arr[i] << " ";
            i = (i + 1) % capacity;
        }
        cout << "\n";
    }
};
```

### Linked List-Based Queue
```cpp
template<typename T>
class LinkedQueue {
private:
    struct Node {
        T data;
        Node* next;
        Node(T val) : data(val), next(nullptr) {}
    };
    
    Node* front;
    Node* rear;
    int queueSize;

public:
    LinkedQueue() : front(nullptr), rear(nullptr), queueSize(0) {}
    
    ~LinkedQueue() {
        while (!isEmpty()) {
            dequeue();
        }
    }
    
    void enqueue(T element) {
        Node* newNode = new Node(element);
        if (isEmpty()) {
            front = rear = newNode;
        } else {
            rear->next = newNode;
            rear = newNode;
        }
        queueSize++;
    }
    
    T dequeue() {
        if (isEmpty()) {
            throw underflow_error("Queue Underflow");
        }
        
        T data = front->data;
        Node* temp = front;
        front = front->next;
        
        if (front == nullptr) {
            rear = nullptr; // Queue becomes empty
        }
        
        delete temp;
        queueSize--;
        return data;
    }
    
    T frontElement() {
        if (isEmpty()) {
            throw underflow_error("Queue is empty");
        }
        return front->data;
    }
    
    bool isEmpty() {
        return front == nullptr;
    }
    
    int size() {
        return queueSize;
    }
};
```

### Double-Ended Queue (Deque)
```cpp
template<typename T>
class Deque {
private:
    struct Node {
        T data;
        Node* next;
        Node* prev;
        Node(T val) : data(val), next(nullptr), prev(nullptr) {}
    };
    
    Node* front;
    Node* rear;
    int dequeSize;

public:
    Deque() : front(nullptr), rear(nullptr), dequeSize(0) {}
    
    // Add to front - O(1)
    void pushFront(T element) {
        Node* newNode = new Node(element);
        if (isEmpty()) {
            front = rear = newNode;
        } else {
            newNode->next = front;
            front->prev = newNode;
            front = newNode;
        }
        dequeSize++;
    }
    
    // Add to rear - O(1)
    void pushRear(T element) {
        Node* newNode = new Node(element);
        if (isEmpty()) {
            front = rear = newNode;
        } else {
            rear->next = newNode;
            newNode->prev = rear;
            rear = newNode;
        }
        dequeSize++;
    }
    
    // Remove from front - O(1)
    T popFront() {
        if (isEmpty()) {
            throw underflow_error("Deque is empty");
        }
        
        T data = front->data;
        Node* temp = front;
        front = front->next;
        
        if (front) {
            front->prev = nullptr;
        } else {
            rear = nullptr; // Deque becomes empty
        }
        
        delete temp;
        dequeSize--;
        return data;
    }
    
    // Remove from rear - O(1)
    T popRear() {
        if (isEmpty()) {
            throw underflow_error("Deque is empty");
        }
        
        T data = rear->data;
        Node* temp = rear;
        rear = rear->prev;
        
        if (rear) {
            rear->next = nullptr;
        } else {
            front = nullptr; // Deque becomes empty
        }
        
        delete temp;
        dequeSize--;
        return data;
    }
    
    T getFront() {
        if (isEmpty()) throw underflow_error("Deque is empty");
        return front->data;
    }
    
    T getRear() {
        if (isEmpty()) throw underflow_error("Deque is empty");
        return rear->data;
    }
    
    bool isEmpty() {
        return front == nullptr;
    }
    
    int size() {
        return dequeSize;
    }
};
```

## Advanced Stack Techniques {#advanced-stack}

### 1. Stack with Min/Max in O(1)
```cpp
class MinStack {
private:
    stack<int> st;
    stack<int> minSt;

public:
    void push(int val) {
        st.push(val);
        if (minSt.empty() || val <= minSt.top()) {
            minSt.push(val);
        }
    }
    
    void pop() {
        if (st.top() == minSt.top()) {
            minSt.pop();
        }
        st.pop();
    }
    
    int top() {
        return st.top();
    }
    
    int getMin() {
        return minSt.top();
    }
};

// Space optimized version
class MinStackOptimized {
private:
    stack<long long> st;
    long long minElement;

public:
    void push(int val) {
        if (st.empty()) {
            st.push(val);
            minElement = val;
        } else if (val >= minElement) {
            st.push(val);
        } else {
            st.push(2LL * val - minElement);
            minElement = val;
        }
    }
    
    void pop() {
        if (st.top() < minElement) {
            minElement = 2 * minElement - st.top();
        }
        st.pop();
    }
    
    int top() {
        if (st.top() < minElement) {
            return minElement;
        }
        return st.top();
    }
    
    int getMin() {
        return minElement;
    }
};
```

### 2. Two Stacks in One Array
```cpp
class TwoStacks {
private:
    int* arr;
    int size;
    int top1, top2;

public:
    TwoStacks(int n) : size(n), top1(-1), top2(n) {
        arr = new int[n];
    }
    
    ~TwoStacks() {
        delete[] arr;
    }
    
    void push1(int x) {
        if (top1 < top2 - 1) {
            arr[++top1] = x;
        } else {
            throw overflow_error("Stack Overflow");
        }
    }
    
    void push2(int x) {
        if (top1 < top2 - 1) {
            arr[--top2] = x;
        } else {
            throw overflow_error("Stack Overflow");
        }
    }
    
    int pop1() {
        if (top1 >= 0) {
            return arr[top1--];
        } else {
            throw underflow_error("Stack 1 Underflow");
        }
    }
    
    int pop2() {
        if (top2 < size) {
            return arr[top2++];
        } else {
            throw underflow_error("Stack 2 Underflow");
        }
    }
};
```

## Advanced Queue Techniques {#advanced-queue}

### 1. Queue using Two Stacks
```cpp
class QueueUsingStacks {
private:
    stack<int> input;
    stack<int> output;

public:
    void enqueue(int x) {
        input.push(x);
    }
    
    int dequeue() {
        if (output.empty()) {
            while (!input.empty()) {
                output.push(input.top());
                input.pop();
            }
        }
        
        if (output.empty()) {
            throw underflow_error("Queue is empty");
        }
        
        int front = output.top();
        output.pop();
        return front;
    }
    
    int front() {
        if (output.empty()) {
            while (!input.empty()) {
                output.push(input.top());
                input.pop();
            }
        }
        
        if (output.empty()) {
            throw underflow_error("Queue is empty");
        }
        
        return output.top();
    }
    
    bool empty() {
        return input.empty() && output.empty();
    }
};
```

### 2. Stack using Two Queues
```cpp
class StackUsingQueues {
private:
    queue<int> q1;
    queue<int> q2;

public:
    void push(int x) {
        q2.push(x);
        while (!q1.empty()) {
            q2.push(q1.front());
            q1.pop();
        }
        swap(q1, q2);
    }
    
    int pop() {
        if (q1.empty()) {
            throw underflow_error("Stack is empty");
        }
        int top = q1.front();
        q1.pop();
        return top;
    }
    
    int top() {
        if (q1.empty()) {
            throw underflow_error("Stack is empty");
        }
        return q1.front();
    }
    
    bool empty() {
        return q1.empty();
    }
};
```

## Classic Problems with Three Approaches {#classic-problems}

### Problem 1: Valid Parentheses

#### Approach 1: Stack (Standard)
```cpp
bool isValid1(string s) {
    stack<char> st;
    unordered_map<char, char> matching = {
        {')', '('}, {']', '['}, {'}', '{'}
    };
    
    for (char c : s) {
        if (c == '(' || c == '[' || c == '{') {
            st.push(c);
        } else {
            if (st.empty() || st.top() != matching[c]) {
                return false;
            }
            st.pop();
        }
    }
    
    return st.empty();
}
// Time: O(n), Space: O(n)
```

#### Approach 2: Counter Method
```cpp
bool isValid2(string s) {
    int round = 0, square = 0, curly = 0;
    
    for (char c : s) {
        if (c == '(') round++;
        else if (c == ')') round--;
        else if (c == '[') square++;
        else if (c == ']') square--;
        else if (c == '{') curly++;
        else if (c == '}') curly--;
        
        // Early termination if any counter goes negative
        if (round < 0 || square < 0 || curly < 0) {
            return false;
        }
    }
    
    return round == 0 && square == 0 && curly == 0;
}
// Time: O(n), Space: O(1) - but only works for this specific problem
```

#### Approach 3: Recursive
```cpp
bool isValidHelper(string& s, int& index, char expected) {
    while (index < s.length()) {
        char c = s[index++];
        
        if (c == '(' || c == '[' || c == '{') {
            char closing = (c == '(') ? ')' : (c == '[') ? ']' : '}';
            if (!isValidHelper(s, index, closing)) {
                return false;
            }
        } else {
            return c == expected;
        }
    }
    return expected == '\0';
}

bool isValid3(string s) {
    int index = 0;
    return isValidHelper(s, index, '\0') && index == s.length();
}
// Time: O(n), Space: O(n) due to recursion
```

### Problem 2: Largest Rectangle in Histogram

#### Approach 1: Brute Force
```cpp
int largestRectangleArea1(vector<int>& heights) {
    int maxArea = 0;
    int n = heights.size();
    
    for (int i = 0; i < n; i++) {
        int minHeight = heights[i];
        for (int j = i; j < n; j++) {
            minHeight = min(minHeight, heights[j]);
            int area = minHeight * (j - i + 1);
            maxArea = max(maxArea, area);
        }
    }
    
    return maxArea;
}
// Time: O(n²), Space: O(1)
```

#### Approach 2: Divide and Conquer
```cpp
int largestRectangleAreaDC(vector<int>& heights, int left, int right) {
    if (left > right) return 0;
    
    // Find minimum height index
    int minIndex = left;
    for (int i = left + 1; i <= right; i++) {
        if (heights[i] < heights[minIndex]) {
            minIndex = i;
        }
    }
    
    int area = heights[minIndex] * (right - left + 1);
    area = max(area, largestRectangleAreaDC(heights, left, minIndex - 1));
    area = max(area, largestRectangleAreaDC(heights, minIndex + 1, right));
    
    return area;
}

int largestRectangleArea2(vector<int>& heights) {
    return largestRectangleAreaDC(heights, 0, heights.size() - 1);
}
// Time: O(n log n) average, O(n²) worst, Space: O(log n)
```

#### Approach 3: Stack (Optimal)
```cpp
int largestRectangleArea3(vector<int>& heights) {
    stack<int> st;
    int maxArea = 0;
    int n = heights.size();
    
    for (int i = 0; i <= n; i++) {
        int currentHeight = (i == n) ? 0 : heights[i];
        
        while (!st.empty() && heights[st.top()] > currentHeight) {
            int height = heights[st.top()];
            st.pop();
            int width = st.empty() ? i : i - st.top() - 1;
            maxArea = max(maxArea, height * width);
        }
        
        st.push(i);
    }
    
    return maxArea;
}
// Time: O(n), Space: O(n) - Optimal!
```

## Monotonic Stack/Queue Mastery {#monotonic}

### Monotonic Stack
```cpp
// Next Greater Element
vector<int> nextGreaterElement(vector<int>& nums) {
    vector<int> result(nums.size(), -1);
    stack<int> st; // Store indices
    
    for (int i = 0; i < nums.size(); i++) {
        while (!st.empty() && nums[st.top()] < nums[i]) {
            result[st.top()] = nums[i];
            st.pop();
        }
        st.push(i);
    }
    
    return result;
}

// Daily Temperatures
vector<int> dailyTemperatures(vector<int>& temperatures) {
    vector<int> result(temperatures.size(), 0);
    stack<int> st;
    
    for (int i = 0; i < temperatures.size(); i++) {
        while (!st.empty() && temperatures[st.top()] < temperatures[i]) {
            int index = st.top();
            st.pop();
            result[index] = i - index;
        }
        st.push(i);
    }
    
    return result;
}

// Remove K Digits
string removeKdigits(string num, int k) {
    stack<char> st;
    
    for (char digit : num) {
        while (!st.empty() && st.top() > digit && k > 0) {
            st.pop();
            k--;
        }
        st.push(digit);
    }
    
    // Remove remaining digits if k > 0
    while (k > 0) {
        st.pop();
        k--;
    }
    
    // Build result
    string result = "";
    while (!st.empty()) {
        result = st.top() + result;
        st.pop();
    }
    
    // Remove leading zeros
    int start = 0;
    while (start < result.length() && result[start] == '0') {
        start++;
    }
    
    result = result.substr(start);
    return result.empty() ? "0" : result;
}
```

### Monotonic Queue (Sliding Window Maximum)
```cpp
vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    deque<int> dq; // Store indices
    vector<int> result;
    
    for (int i = 0; i < nums.size(); i++) {
        // Remove indices outside window
        while (!dq.empty() && dq.front() <= i - k) {
            dq.pop_front();
        }
        
        // Remove indices with smaller values
        while (!dq.empty() && nums[dq.back()] <= nums[i]) {
            dq.pop_back();
        }
        
        dq.push_back(i);
        
        // Add to result if window size is k
        if (i >= k - 1) {
            result.push_back(nums[dq.front()]);
        }
    }
    
    return result;
}
```

## Applications and Real-World Usage {#applications}

### 1. Expression Evaluation
```cpp
// Infix to Postfix
string infixToPostfix(string infix) {
    stack<char> st;
    string postfix = "";
    
    auto precedence = [](char op) {
        if (op == '+' || op == '-') return 1;
        if (op == '*' || op == '/') return 2;
        if (op == '^') return 3;
        return 0;
    };
    
    auto isOperator = [](char c) {
        return c == '+' || c == '-' || c == '*' || c == '/' || c == '^';
    };
    
    for (char c : infix) {
        if (isalnum(c)) {
            postfix += c;
        } else if (c == '(') {
            st.push(c);
        } else if (c == ')') {
            while (!st.empty() && st.top() != '(') {
                postfix += st.top();
                st.pop();
            }
            st.pop(); // Remove '('
        } else if (isOperator(c)) {
            while (!st.empty() && precedence(st.top()) >= precedence(c)) {
                postfix += st.top();
                st.pop();
            }
            st.push(c);
        }
    }
    
    while (!st.empty()) {
        postfix += st.top();
        st.pop();
    }
    
    return postfix;
}

// Evaluate Postfix
int evaluatePostfix(string postfix) {
    stack<int> st;
    
    for (char c : postfix) {
        if (isdigit(c)) {
            st.push(c - '0');
        } else {
            int operand2 = st.top(); st.pop();
            int operand1 = st.top(); st.pop();
            
            switch (c) {
                case '+': st.push(operand1 + operand2); break;
                case '-': st.push(operand1 - operand2); break;
                case '*': st.push(operand1 * operand2); break;
                case '/': st.push(operand1 / operand2); break;
            }
        }
    }
    
    return st.top();
}
```

### 2. Function Call Stack Simulation
```cpp
class CallStack {
private:
    stack<string> calls;
    stack<unordered_map<string, int>> variables;

public:
    void enterFunction(string funcName) {
        calls.push(funcName);
        variables.push(unordered_map<string, int>());
        cout << "Entering: " << funcName << "\n";
    }
    
    void exitFunction() {
        if (!calls.empty()) {
            cout << "Exiting: " << calls.top() << "\n";
            calls.pop();
            variables.pop();
        }
    }
    
    void setVariable(string name, int value) {
        if (!variables.empty()) {
            variables.top()[name] = value;
        }
    }
    
    int getVariable(string name) {
        if (!variables.empty() && variables.top().count(name)) {
            return variables.top()[name];
        }
        return -1; // Variable not found
    }
    
    void printStack() {
        stack<string> temp = calls;
        cout << "Call Stack: ";
        while (!temp.empty()) {
            cout << temp.top() << " -> ";
            temp.pop();
        }
        cout << "main\n";
    }
};
```

### 3. Undo/Redo Operations
```cpp
template<typename T>
class UndoRedoManager {
private:
    stack<T> undoStack;
    stack<T> redoStack;

public:
    void execute(T operation) {
        undoStack.push(operation);
        // Clear redo stack on new operation
        while (!redoStack.empty()) {
            redoStack.pop();
        }
    }
    
    bool canUndo() {
        return !undoStack.empty();
    }
    
    bool canRedo() {
        return !redoStack.empty();
    }
    
    T undo() {
        if (!canUndo()) {
            throw runtime_error("Nothing to undo");
        }
        
        T operation = undoStack.top();
        undoStack.pop();
        redoStack.push(operation);
        return operation;
    }
    
    T redo() {
        if (!canRedo()) {
            throw runtime_error("Nothing to redo");
        }
        
        T operation = redoStack.top();
        redoStack.pop();
        undoStack.push(operation);
        return operation;
    }
};
```

## Practice Problems {#practice-problems}

### Easy Level - Stack
1. **Valid Parentheses** (Three approaches)
2. **Implement Stack using Queues**
3. **Min Stack** (Get minimum in O(1))
4. **Baseball Game** (Stack simulation)
5. **Remove All Adjacent Duplicates in String**

### Easy Level - Queue
1. **Implement Queue using Stacks**
2. **Design Circular Queue**
3. **Number of Recent Calls**
4. **First Unique Character in String**
5. **Moving Average from Data Stream**

### Medium Level - Stack
1. **Daily Temperatures** (Monotonic stack)
2. **Next Greater Element II**
3. **Largest Rectangle in Histogram**
4. **Evaluate Reverse Polish Notation**
5. **Decode String**
6. **Remove K Digits**

### Medium Level - Queue
1. **Sliding Window Maximum** (Monotonic deque)
2. **Task Scheduler**
3. **Design Hit Counter**
4. **Shortest Subarray with Sum at Least K**
5. **Reveal Cards in Increasing Order**

### Hard Level
1. **Maximal Rectangle** (2D histogram)
2. **Trapping Rain Water** (Stack approach)
3. **Basic Calculator** (Stack with operators)
4. **Longest Valid Parentheses**
5. **Maximum Frequency Stack**
6. **Constrained Subsequence Sum**

### System Design Problems
1. **LRU Cache** (Queue + HashMap)
2. **Browser History** (Two stacks)
3. **Process Scheduler** (Priority queue)
4. **Function Call Profiler** (Stack)
5. **Memory Management** (Stack allocation)

## Advanced Competitive Programming Techniques

### 1. Segment Tree with Lazy Propagation using Stacks
```cpp
// Stack-based segment tree traversal
class StackSegmentTree {
private:
    vector<int> tree;
    int n;
    
    struct Operation {
        int node, start, end, type;
        // type: 0 = query, 1 = update
    };

public:
    StackSegmentTree(vector<int>& arr) {
        n = arr.size();
        tree.resize(4 * n);
        build(arr);
    }
    
    void build(vector<int>& arr) {
        stack<Operation> st;
        st.push({1, 0, n - 1, 1});
        
        while (!st.empty()) {
            auto op = st.top();
            st.pop();
            
            if (op.start == op.end) {
                tree[op.node] = arr[op.start];
            } else {
                int mid = (op.start + op.end) / 2;
                st.push({2 * op.node, op.start, mid, 1});
                st.push({2 * op.node + 1, mid + 1, op.end, 1});
                tree[op.node] = tree[2 * op.node] + tree[2 * op.node + 1];
            }
        }
    }
};
```

### 2. Stack-based DFS with Path Tracking
```cpp
vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
    vector<vector<int>> result;
    stack<pair<int, vector<int>>> st;
    st.push({0, {0}});
    
    while (!st.empty()) {
        auto [node, path] = st.top();
        st.pop();
        
        if (node == graph.size() - 1) {
            result.push_back(path);
            continue;
        }
        
        for (int neighbor : graph[node]) {
            vector<int> newPath = path;
            newPath.push_back(neighbor);
            st.push({neighbor, newPath});
        }
    }
    
    return result;
}
```

## Key Takeaways

1. **Choose the Right Implementation**: Array vs Linked List based on requirements
2. **Stack Applications**: Expression evaluation, DFS, undo operations, function calls
3. **Queue Applications**: BFS, scheduling, buffering, level-order traversal
4. **Monotonic Structures**: Powerful for optimization problems
5. **Space-Time Tradeoffs**: Understand when to use auxiliary data structures
6. **Edge Cases**: Empty structures, single elements, overflow/underflow

## Next Chapter Preview

In **Chapter 10: Binary Trees**, we'll explore:
- Tree traversals (preorder, inorder, postorder, level-order)
- Binary search trees and their properties
- Tree construction and modification problems
- Advanced tree algorithms and optimizations
- Height-balanced trees and rotations

The foundation is strong! Let's build the tree of knowledge next! 🌳

---
*"In the world of programming, stacks teach us that sometimes we need to retrace our steps to move forward, while queues remind us that patience and order often lead to the best outcomes."*
