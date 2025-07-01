# Chapter 3: Time & Space Complexity Analysis ⏰

Master the art of measuring algorithmic efficiency like a pro!

## 🎯 Learning Objectives
- Understand Big O notation thoroughly
- Analyze time and space complexity of algorithms
- Compare different approaches effectively
- Optimize solutions systematically

## 🤔 Why Complexity Analysis Matters

Imagine you have two solutions:
- Solution A: Works for small inputs but takes 1 hour for large inputs
- Solution B: Takes 1 second for both small and large inputs

Which would you choose? Complexity analysis helps you make this decision!

## 📊 Big O Notation

Big O describes the **worst-case** performance of an algorithm as input size grows.

### Mathematical Definition
f(n) = O(g(n)) if there exist constants c and n₀ such that:
f(n) ≤ c × g(n) for all n ≥ n₀

**Translation**: We ignore constants and lower-order terms, focusing on the dominant factor.

## 🏆 Common Time Complexities (Best to Worst)

### O(1) - Constant Time
```cpp
// Examples of O(1) operations
int getFirst(vector<int>& arr) {
    return arr[0];  // Always takes the same time
}

void insertAtEnd(vector<int>& arr, int value) {
    arr.push_back(value);  // Amortized O(1)
}

bool isEmpty(stack<int>& st) {
    return st.empty();  // Always O(1)
}
```

**Characteristics**:
- Execution time doesn't depend on input size
- Most basic operations (arithmetic, array access, etc.)

### O(log n) - Logarithmic Time
```cpp
// Binary Search - Classic O(log n) algorithm
int binarySearch(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    
    return -1;  // Not found
}

// Height of balanced binary tree
int getHeight(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(getHeight(root->left), getHeight(root->right));
}
// Height = O(log n) for balanced tree
```

**Characteristics**:
- Cuts problem size in half each step
- Very efficient for large inputs
- Common in divide-and-conquer algorithms

### O(n) - Linear Time
```cpp
// Linear Search
int linearSearch(vector<int>& arr, int target) {
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] == target) return i;
    }
    return -1;
}

// Sum of array elements
int sumArray(vector<int>& arr) {
    int sum = 0;
    for (int num : arr) {
        sum += num;  // O(1) operation done n times = O(n)
    }
    return sum;
}

// Finding maximum element
int findMax(vector<int>& arr) {
    int maxVal = arr[0];
    for (int i = 1; i < arr.size(); i++) {
        maxVal = max(maxVal, arr[i]);
    }
    return maxVal;
}
```

**Characteristics**:
- Must examine each element once
- Unavoidable for many problems
- Efficient and often optimal

### O(n log n) - Linearithmic Time
```cpp
// Merge Sort - Classic O(n log n) algorithm
void mergeSort(vector<int>& arr, int left, int right) {
    if (left >= right) return;
    
    int mid = left + (right - left) / 2;
    mergeSort(arr, left, mid);      // T(n/2)
    mergeSort(arr, mid + 1, right); // T(n/2)
    merge(arr, left, mid, right);   // O(n)
}
// Total: T(n) = 2T(n/2) + O(n) = O(n log n)

// Using STL sort (highly optimized)
void sortArray(vector<int>& arr) {
    sort(arr.begin(), arr.end());  // O(n log n)
}

// Building heap from array
void buildHeap(vector<int>& arr) {
    make_heap(arr.begin(), arr.end());  // O(n log n)
}
```

**Characteristics**:
- Optimal for comparison-based sorting
- Common in divide-and-conquer with linear merge
- Very practical for most inputs

### O(n²) - Quadratic Time
```cpp
// Bubble Sort
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {        // Outer loop: n times
        for (int j = 0; j < n - i - 1; j++) { // Inner loop: n times
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);     // O(1) operation
            }
        }
    }
}
// Total: n × n × O(1) = O(n²)

// Nested loop pattern
void printPairs(vector<int>& arr) {
    for (int i = 0; i < arr.size(); i++) {
        for (int j = i + 1; j < arr.size(); j++) {
            cout << arr[i] << ", " << arr[j] << endl;
        }
    }
}

// Selection Sort
void selectionSort(vector<int>& arr) {
    int n = arr.size();
    for (int i = 0; i < n - 1; i++) {
        int minIdx = i;
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIdx]) {
                minIdx = j;
            }
        }
        swap(arr[i], arr[minIdx]);
    }
}
```

**Characteristics**:
- Nested loops over the same data
- Quickly becomes impractical for large inputs
- Often indicates need for optimization

### O(2ⁿ) - Exponential Time
```cpp
// Naive Fibonacci (very inefficient!)
int fibonacci(int n) {
    if (n <= 1) return n;
    return fibonacci(n - 1) + fibonacci(n - 2);
}
// Each call spawns 2 more calls → 2^n total calls

// Generate all subsets (2^n subsets exist)
void generateSubsets(vector<int>& arr, int index, vector<int>& current) {
    if (index == arr.size()) {
        // Process current subset
        return;
    }
    
    // Don't include current element
    generateSubsets(arr, index + 1, current);
    
    // Include current element
    current.push_back(arr[index]);
    generateSubsets(arr, index + 1, current);
    current.pop_back();
}

// Brute force for traveling salesman
int tsp(vector<vector<int>>& graph, int pos, int visited) {
    if (visited == (1 << graph.size()) - 1) {
        return graph[pos][0];  // Return to start
    }
    
    int result = INT_MAX;
    for (int city = 0; city < graph.size(); city++) {
        if (!(visited & (1 << city))) {
            int newResult = graph[pos][city] + 
                           tsp(graph, city, visited | (1 << city));
            result = min(result, newResult);
        }
    }
    return result;
}
```

**Characteristics**:
- Each step doubles the work
- Only feasible for very small inputs
- Often needs dynamic programming optimization

## 📈 Growth Rate Comparison

For n = 1,000,000:
- O(1): 1 operation
- O(log n): ~20 operations
- O(n): 1,000,000 operations
- O(n log n): ~20,000,000 operations
- O(n²): 1,000,000,000,000 operations
- O(2ⁿ): More than atoms in the universe!

## 💾 Space Complexity

Space complexity measures additional memory used by an algorithm.

### O(1) - Constant Space
```cpp
// In-place algorithms
void reverseArray(vector<int>& arr) {
    int left = 0, right = arr.size() - 1;
    while (left < right) {
        swap(arr[left], arr[right]);
        left++;
        right--;
    }
}
// Only uses a few variables, regardless of input size

// Finding maximum without extra space
int findMax(vector<int>& arr) {
    int maxVal = arr[0];
    for (int i = 1; i < arr.size(); i++) {
        maxVal = max(maxVal, arr[i]);
    }
    return maxVal;
}
```

### O(n) - Linear Space
```cpp
// Creating a copy of array
vector<int> copyArray(vector<int>& arr) {
    vector<int> copy(arr.size());
    for (int i = 0; i < arr.size(); i++) {
        copy[i] = arr[i];
    }
    return copy;  // Uses O(n) extra space
}

// Recursive function calls (call stack)
int factorial(int n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
// Each recursive call uses stack space → O(n) space
```

### O(n²) - Quadratic Space
```cpp
// 2D matrix
vector<vector<int>> createMatrix(int n) {
    vector<vector<int>> matrix(n, vector<int>(n, 0));
    return matrix;  // n × n = O(n²) space
}

// Adjacency matrix for graph
class Graph {
private:
    vector<vector<int>> adjMatrix;  // O(V²) space
    
public:
    Graph(int vertices) {
        adjMatrix.resize(vertices, vector<int>(vertices, 0));
    }
};
```

## 🔍 How to Analyze Complexity

### Step 1: Identify the Basic Operations
```cpp
void example(vector<int>& arr) {
    int sum = 0;                    // O(1)
    for (int i = 0; i < arr.size(); i++) {  // Loop runs n times
        sum += arr[i];              // O(1) operation
    }                               // Total: n × O(1) = O(n)
    
    for (int i = 0; i < arr.size(); i++) {
        for (int j = i + 1; j < arr.size(); j++) {  // Nested loops
            if (arr[i] + arr[j] == 0) {             // O(1) operation
                cout << "Found pair!" << endl;
            }
        }
    }  // Total: O(n²)
}
// Overall complexity: O(n) + O(n²) = O(n²)
```

### Step 2: Analyze Recursive Functions
```cpp
// Master Theorem: T(n) = aT(n/b) + f(n)
// Where a = number of subproblems, b = input size reduction factor

// Binary Search: T(n) = T(n/2) + O(1)
// a = 1, b = 2, f(n) = O(1)
// Result: O(log n)

// Merge Sort: T(n) = 2T(n/2) + O(n)
// a = 2, b = 2, f(n) = O(n)
// Result: O(n log n)

// Fibonacci (naive): T(n) = T(n-1) + T(n-2) + O(1)
// No simple reduction → O(2ⁿ)
```

### Step 3: Consider Best, Average, and Worst Cases
```cpp
// Quick Sort Analysis
int partition(vector<int>& arr, int low, int high) {
    // Partitioning takes O(n) time
}

void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

// Best Case: Pivot divides array evenly → O(n log n)
// Average Case: Random pivot → O(n log n)
// Worst Case: Pivot is always smallest/largest → O(n²)
```

## 🎯 Practical Examples

### Example 1: Find Duplicates in Array

```cpp
// Approach 1: Brute Force - O(n²) time, O(1) space
bool hasDuplicatesBrute(vector<int>& arr) {
    for (int i = 0; i < arr.size(); i++) {
        for (int j = i + 1; j < arr.size(); j++) {
            if (arr[i] == arr[j]) return true;
        }
    }
    return false;
}

// Approach 2: Sorting - O(n log n) time, O(1) space
bool hasDuplicatesSort(vector<int>& arr) {
    sort(arr.begin(), arr.end());
    for (int i = 1; i < arr.size(); i++) {
        if (arr[i] == arr[i-1]) return true;
    }
    return false;
}

// Approach 3: Hash Set - O(n) time, O(n) space
bool hasDuplicatesHash(vector<int>& arr) {
    unordered_set<int> seen;
    for (int num : arr) {
        if (seen.count(num)) return true;
        seen.insert(num);
    }
    return false;
}
```

**Analysis**:
- Brute Force: Simple but slow for large inputs
- Sorting: Good compromise, modifies original array
- Hash Set: Fastest but uses extra memory

### Example 2: Fibonacci Optimization

```cpp
// Approach 1: Naive Recursion - O(2ⁿ) time, O(n) space
int fibNaive(int n) {
    if (n <= 1) return n;
    return fibNaive(n-1) + fibNaive(n-2);
}

// Approach 2: Memoization - O(n) time, O(n) space
int fibMemo(int n, vector<int>& memo) {
    if (n <= 1) return n;
    if (memo[n] != -1) return memo[n];
    
    memo[n] = fibMemo(n-1, memo) + fibMemo(n-2, memo);
    return memo[n];
}

// Approach 3: Iterative - O(n) time, O(1) space
int fibIterative(int n) {
    if (n <= 1) return n;
    
    int prev2 = 0, prev1 = 1;
    for (int i = 2; i <= n; i++) {
        int current = prev1 + prev2;
        prev2 = prev1;
        prev1 = current;
    }
    return prev1;
}

// Approach 4: Matrix Exponentiation - O(log n) time, O(1) space
// Advanced technique using matrix multiplication
```

## 📋 Complexity Analysis Cheat Sheet

### Common Patterns
| Pattern | Time Complexity | Example |
|---------|----------------|---------|
| Single loop | O(n) | Linear search |
| Nested loops | O(n²) | Bubble sort |
| Divide and conquer | O(n log n) | Merge sort |
| Binary search pattern | O(log n) | Binary search |
| Dynamic programming | O(n × states) | Fibonacci memo |
| Backtracking | O(2ⁿ) or worse | Generate subsets |

### STL Complexities
| Operation | vector | set/map | unordered_set/map |
|-----------|--------|---------|-------------------|
| Insert | O(1) amortized | O(log n) | O(1) average |
| Delete | O(n) | O(log n) | O(1) average |
| Search | O(n) | O(log n) | O(1) average |
| Access by index | O(1) | - | - |

## 🎯 Practice Problems

### Problem 1: Analyze This Code
```cpp
void mysteryFunction(int n) {
    for (int i = 1; i <= n; i++) {
        for (int j = 1; j <= i; j++) {
            cout << "*";
        }
        cout << endl;
    }
}
```
**Question**: What's the time complexity?
**Answer**: O(n²) - Inner loop runs 1+2+3+...+n = n(n+1)/2 times

### Problem 2: Optimize This
```cpp
// Find the intersection of two arrays
vector<int> intersection(vector<int>& arr1, vector<int>& arr2) {
    vector<int> result;
    for (int i = 0; i < arr1.size(); i++) {
        for (int j = 0; j < arr2.size(); j++) {
            if (arr1[i] == arr2[j]) {
                result.push_back(arr1[i]);
                break;
            }
        }
    }
    return result;
}
```
**Current**: O(n×m) time
**Optimized**: Use hash set → O(n+m) time

## ✅ Chapter Summary

- Big O notation measures algorithm efficiency as input grows
- Focus on dominant terms, ignore constants
- Consider both time and space complexity
- Choose the right approach based on constraints
- Practice analyzing different patterns

## 🎯 Complexity Decision Framework

When choosing between algorithms:

1. **Small inputs (n < 100)**: Any reasonable approach works
2. **Medium inputs (n < 10⁶)**: O(n log n) or better needed
3. **Large inputs (n > 10⁶)**: O(n) or O(log n) preferred
4. **Memory constraints**: Prefer lower space complexity
5. **Real-time systems**: Predictable performance matters

## 🎯 Next Chapter Preview

In [Chapter 4: Arrays & Strings](../04-arrays-strings/README.md), we'll dive deep into the most fundamental data structures and master essential algorithms for manipulating them.

---

**Golden Rule**: "Premature optimization is the root of all evil, but understanding complexity is the root of all good algorithms!"

[← Previous Chapter](../02-cpp-essentials/README.md) | [Next Chapter: Arrays & Strings →](../04-arrays-strings/README.md)
