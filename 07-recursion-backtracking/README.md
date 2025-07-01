# Chapter 7: Recursion & Backtracking 🌀

Master the art of solving complex problems by breaking them into smaller pieces!

## 🎯 Learning Objectives
- Understand recursion fundamentals and design principles
- Master different types of recursive problems
- Learn backtracking methodology and applications
- Optimize recursive solutions with memoization
- Solve complex combinatorial problems

## 🔄 Introduction to Recursion

Recursion is a programming technique where a function calls itself to solve a smaller instance of the same problem.

### The Recursion Trinity:
1. **Base Case**: The stopping condition
2. **Recursive Case**: The function calls itself
3. **Progress**: Each call moves closer to the base case

### Why Recursion?
- **Natural Problem Solving**: Many problems have recursive structure
- **Code Simplicity**: Often leads to cleaner, more readable code
- **Divide and Conquer**: Break complex problems into simpler ones
- **Tree/Graph Traversal**: Essential for hierarchical data structures

## 🧱 Recursion Fundamentals

### Basic Structure
```cpp
ReturnType recursiveFunction(parameters) {
    // Base case(s)
    if (baseCondition) {
        return baseValue;
    }
    
    // Recursive case
    // Do some work
    return recursiveFunction(modifiedParameters);
}
```

### Simple Examples

#### 1. Factorial
```cpp
// Approach 1: Basic recursion - O(n) time, O(n) space
long long factorial(int n) {
    // Base case
    if (n <= 1) {
        return 1;
    }
    
    // Recursive case
    return n * factorial(n - 1);
}

// Approach 2: Tail recursion (optimizable)
long long factorialTail(int n, long long acc = 1) {
    if (n <= 1) {
        return acc;
    }
    return factorialTail(n - 1, n * acc);
}

// Approach 3: Iterative (for comparison)
long long factorialIterative(int n) {
    long long result = 1;
    for (int i = 2; i <= n; i++) {
        result *= i;
    }
    return result;
}
```

#### 2. Fibonacci Sequence
```cpp
// Approach 1: Naive recursion - O(2^n) time, O(n) space
int fibonacciNaive(int n) {
    if (n <= 1) {
        return n;
    }
    return fibonacciNaive(n - 1) + fibonacciNaive(n - 2);
}

// Approach 2: Memoization - O(n) time, O(n) space
int fibonacciMemo(int n, vector<int>& memo) {
    if (n <= 1) {
        return n;
    }
    
    if (memo[n] != -1) {
        return memo[n];
    }
    
    memo[n] = fibonacciMemo(n - 1, memo) + fibonacciMemo(n - 2, memo);
    return memo[n];
}

// Wrapper function
int fibonacci(int n) {
    vector<int> memo(n + 1, -1);
    return fibonacciMemo(n, memo);
}

// Approach 3: Bottom-up DP - O(n) time, O(1) space
int fibonacciDP(int n) {
    if (n <= 1) return n;
    
    int prev2 = 0, prev1 = 1;
    for (int i = 2; i <= n; i++) {
        int current = prev1 + prev2;
        prev2 = prev1;
        prev1 = current;
    }
    return prev1;
}
```

#### 3. Power Function
```cpp
// Approach 1: Linear recursion - O(n) time
long long powerLinear(int base, int exp) {
    if (exp == 0) return 1;
    return base * powerLinear(base, exp - 1);
}

// Approach 2: Fast exponentiation - O(log n) time
long long powerFast(int base, int exp) {
    if (exp == 0) return 1;
    
    long long half = powerFast(base, exp / 2);
    
    if (exp % 2 == 0) {
        return half * half;
    } else {
        return base * half * half;
    }
}

// Approach 3: With modulo (for large numbers)
long long powerMod(long long base, long long exp, long long mod) {
    if (exp == 0) return 1;
    
    long long half = powerMod(base, exp / 2, mod);
    half = (half * half) % mod;
    
    if (exp % 2 == 1) {
        half = (half * base) % mod;
    }
    
    return half;
}
```

## 🌳 Tree Recursion Patterns

### Binary Tree Problems

#### Tree Structure
```cpp
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};
```

#### 1. Tree Traversals
```cpp
// Inorder traversal: Left -> Root -> Right
void inorderTraversal(TreeNode* root, vector<int>& result) {
    if (root == nullptr) return;
    
    inorderTraversal(root->left, result);
    result.push_back(root->val);
    inorderTraversal(root->right, result);
}

// Preorder traversal: Root -> Left -> Right
void preorderTraversal(TreeNode* root, vector<int>& result) {
    if (root == nullptr) return;
    
    result.push_back(root->val);
    preorderTraversal(root->left, result);
    preorderTraversal(root->right, result);
}

// Postorder traversal: Left -> Right -> Root
void postorderTraversal(TreeNode* root, vector<int>& result) {
    if (root == nullptr) return;
    
    postorderTraversal(root->left, result);
    postorderTraversal(root->right, result);
    result.push_back(root->val);
}
```

#### 2. Tree Properties
```cpp
// Maximum depth of binary tree
int maxDepth(TreeNode* root) {
    if (root == nullptr) return 0;
    
    int leftDepth = maxDepth(root->left);
    int rightDepth = maxDepth(root->right);
    
    return 1 + max(leftDepth, rightDepth);
}

// Check if tree is balanced
bool isBalanced(TreeNode* root) {
    if (root == nullptr) return true;
    
    int leftHeight = maxDepth(root->left);
    int rightHeight = maxDepth(root->right);
    
    return abs(leftHeight - rightHeight) <= 1 && 
           isBalanced(root->left) && 
           isBalanced(root->right);
}

// Optimized balanced check - O(n) time
int checkHeight(TreeNode* root) {
    if (root == nullptr) return 0;
    
    int leftHeight = checkHeight(root->left);
    if (leftHeight == -1) return -1;  // Left subtree is unbalanced
    
    int rightHeight = checkHeight(root->right);
    if (rightHeight == -1) return -1;  // Right subtree is unbalanced
    
    if (abs(leftHeight - rightHeight) > 1) {
        return -1;  // Current tree is unbalanced
    }
    
    return 1 + max(leftHeight, rightHeight);
}

bool isBalancedOptimal(TreeNode* root) {
    return checkHeight(root) != -1;
}

// Check if two trees are identical
bool isSameTree(TreeNode* p, TreeNode* q) {
    if (p == nullptr && q == nullptr) return true;
    if (p == nullptr || q == nullptr) return false;
    
    return p->val == q->val && 
           isSameTree(p->left, q->left) && 
           isSameTree(p->right, q->right);
}

// Invert binary tree
TreeNode* invertTree(TreeNode* root) {
    if (root == nullptr) return nullptr;
    
    TreeNode* temp = root->left;
    root->left = invertTree(root->right);
    root->right = invertTree(temp);
    
    return root;
}
```

#### 3. Path Problems
```cpp
// Binary tree paths (all root-to-leaf paths)
void findPaths(TreeNode* root, string path, vector<string>& result) {
    if (root == nullptr) return;
    
    path += to_string(root->val);
    
    if (root->left == nullptr && root->right == nullptr) {
        result.push_back(path);
        return;
    }
    
    path += "->";
    findPaths(root->left, path, result);
    findPaths(root->right, path, result);
}

vector<string> binaryTreePaths(TreeNode* root) {
    vector<string> result;
    findPaths(root, "", result);
    return result;
}

// Path sum - check if path with given sum exists
bool hasPathSum(TreeNode* root, int targetSum) {
    if (root == nullptr) return false;
    
    if (root->left == nullptr && root->right == nullptr) {
        return root->val == targetSum;
    }
    
    return hasPathSum(root->left, targetSum - root->val) || 
           hasPathSum(root->right, targetSum - root->val);
}

// Path sum II - find all paths with given sum
void findPathSum(TreeNode* root, int targetSum, vector<int>& path, 
                 vector<vector<int>>& result) {
    if (root == nullptr) return;
    
    path.push_back(root->val);
    
    if (root->left == nullptr && root->right == nullptr && 
        root->val == targetSum) {
        result.push_back(path);
    } else {
        findPathSum(root->left, targetSum - root->val, path, result);
        findPathSum(root->right, targetSum - root->val, path, result);
    }
    
    path.pop_back();  // Backtrack
}

// Maximum path sum (any node to any node)
int maxPathSumHelper(TreeNode* root, int& globalMax) {
    if (root == nullptr) return 0;
    
    int leftSum = max(0, maxPathSumHelper(root->left, globalMax));
    int rightSum = max(0, maxPathSumHelper(root->right, globalMax));
    
    int currentMax = root->val + leftSum + rightSum;
    globalMax = max(globalMax, currentMax);
    
    return root->val + max(leftSum, rightSum);
}

int maxPathSum(TreeNode* root) {
    int globalMax = INT_MIN;
    maxPathSumHelper(root, globalMax);
    return globalMax;
}
```

## 🎯 Advanced Recursion Techniques

### 1. Divide and Conquer
Break problem into subproblems, solve recursively, combine results.

```cpp
// Merge Sort (divide and conquer classic)
void merge(vector<int>& arr, int left, int mid, int right) {
    vector<int> temp(right - left + 1);
    int i = left, j = mid + 1, k = 0;
    
    while (i <= mid && j <= right) {
        if (arr[i] <= arr[j]) {
            temp[k++] = arr[i++];
        } else {
            temp[k++] = arr[j++];
        }
    }
    
    while (i <= mid) temp[k++] = arr[i++];
    while (j <= right) temp[k++] = arr[j++];
    
    for (int i = 0; i < k; i++) {
        arr[left + i] = temp[i];
    }
}

void mergeSort(vector<int>& arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        mergeSort(arr, left, mid);
        mergeSort(arr, mid + 1, right);
        merge(arr, left, mid, right);
    }
}

// Quick Sort
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];
    int i = low - 1;
    
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        quickSort(arr, low, pi - 1);
        quickSort(arr, pi + 1, high);
    }
}

// Maximum subarray (divide and conquer)
int maxCrossingSum(vector<int>& arr, int left, int mid, int right) {
    int leftSum = INT_MIN;
    int sum = 0;
    
    for (int i = mid; i >= left; i--) {
        sum += arr[i];
        if (sum > leftSum) leftSum = sum;
    }
    
    int rightSum = INT_MIN;
    sum = 0;
    
    for (int i = mid + 1; i <= right; i++) {
        sum += arr[i];
        if (sum > rightSum) rightSum = sum;
    }
    
    return leftSum + rightSum;
}

int maxSubarrayDC(vector<int>& arr, int left, int right) {
    if (left == right) return arr[left];
    
    int mid = left + (right - left) / 2;
    
    return max({maxSubarrayDC(arr, left, mid),
                maxSubarrayDC(arr, mid + 1, right),
                maxCrossingSum(arr, left, mid, right)});
}
```

### 2. Tail Recursion
When recursive call is the last operation in the function.

```cpp
// GCD using tail recursion
int gcd(int a, int b) {
    if (b == 0) return a;
    return gcd(b, a % b);
}

// Reverse a string using tail recursion
string reverseString(string s, int start = 0) {
    int n = s.length();
    if (start >= n / 2) return s;
    
    swap(s[start], s[n - 1 - start]);
    return reverseString(s, start + 1);
}

// Check if array is sorted (tail recursion)
bool isSorted(vector<int>& arr, int index = 0) {
    if (index >= arr.size() - 1) return true;
    
    if (arr[index] > arr[index + 1]) return false;
    
    return isSorted(arr, index + 1);
}
```

## 🎯 Backtracking Fundamentals

Backtracking is an algorithmic approach that systematically searches for solutions by building candidates incrementally and abandoning ("backtracking") partial candidates that cannot possibly lead to a valid solution.

### Backtracking Template
```cpp
bool backtrack(state) {
    if (isGoal(state)) {
        processSolution(state);
        return true; // or false if you want all solutions
    }
    
    for (each choice in choices) {
        if (isValid(choice, state)) {
            makeChoice(choice, state);
            
            if (backtrack(state)) {
                return true; // Found solution
            }
            
            undoChoice(choice, state); // Backtrack
        }
    }
    
    return false; // No solution found
}
```

### Classic Backtracking Problems

#### 1. N-Queens Problem
```cpp
class NQueens {
public:
    vector<vector<string>> solveNQueens(int n) {
        vector<vector<string>> solutions;
        vector<string> board(n, string(n, '.'));
        
        backtrack(board, 0, solutions);
        return solutions;
    }
    
private:
    void backtrack(vector<string>& board, int row, 
                   vector<vector<string>>& solutions) {
        if (row == board.size()) {
            solutions.push_back(board);
            return;
        }
        
        for (int col = 0; col < board.size(); col++) {
            if (isSafe(board, row, col)) {
                board[row][col] = 'Q';
                backtrack(board, row + 1, solutions);
                board[row][col] = '.'; // Backtrack
            }
        }
    }
    
    bool isSafe(vector<string>& board, int row, int col) {
        int n = board.size();
        
        // Check column
        for (int i = 0; i < row; i++) {
            if (board[i][col] == 'Q') return false;
        }
        
        // Check diagonal (top-left to bottom-right)
        for (int i = row - 1, j = col - 1; i >= 0 && j >= 0; i--, j--) {
            if (board[i][j] == 'Q') return false;
        }
        
        // Check diagonal (top-right to bottom-left)
        for (int i = row - 1, j = col + 1; i >= 0 && j < n; i--, j++) {
            if (board[i][j] == 'Q') return false;
        }
        
        return true;
    }
};

// Optimized N-Queens using bit manipulation
class NQueensOptimized {
public:
    int totalNQueens(int n) {
        return backtrack(0, 0, 0, 0, n);
    }
    
private:
    int backtrack(int row, int cols, int diag1, int diag2, int n) {
        if (row == n) return 1;
        
        int count = 0;
        int availablePos = ((1 << n) - 1) & (~(cols | diag1 | diag2));
        
        while (availablePos) {
            int pos = availablePos & (-availablePos); // Get rightmost bit
            availablePos &= (availablePos - 1); // Remove rightmost bit
            
            count += backtrack(row + 1, 
                             cols | pos,
                             (diag1 | pos) << 1,
                             (diag2 | pos) >> 1,
                             n);
        }
        
        return count;
    }
};
```

#### 2. Sudoku Solver
```cpp
class SudokuSolver {
public:
    void solveSudoku(vector<vector<char>>& board) {
        backtrack(board);
    }
    
private:
    bool backtrack(vector<vector<char>>& board) {
        for (int row = 0; row < 9; row++) {
            for (int col = 0; col < 9; col++) {
                if (board[row][col] == '.') {
                    for (char num = '1'; num <= '9'; num++) {
                        if (isValid(board, row, col, num)) {
                            board[row][col] = num;
                            
                            if (backtrack(board)) return true;
                            
                            board[row][col] = '.'; // Backtrack
                        }
                    }
                    return false; // No valid number found
                }
            }
        }
        return true; // All cells filled
    }
    
    bool isValid(vector<vector<char>>& board, int row, int col, char num) {
        // Check row
        for (int j = 0; j < 9; j++) {
            if (board[row][j] == num) return false;
        }
        
        // Check column
        for (int i = 0; i < 9; i++) {
            if (board[i][col] == num) return false;
        }
        
        // Check 3x3 box
        int boxRow = (row / 3) * 3;
        int boxCol = (col / 3) * 3;
        
        for (int i = boxRow; i < boxRow + 3; i++) {
            for (int j = boxCol; j < boxCol + 3; j++) {
                if (board[i][j] == num) return false;
            }
        }
        
        return true;
    }
};
```

#### 3. Word Search
```cpp
class WordSearch {
public:
    bool exist(vector<vector<char>>& board, string word) {
        int m = board.size(), n = board[0].size();
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (board[i][j] == word[0] && 
                    backtrack(board, word, i, j, 0)) {
                    return true;
                }
            }
        }
        
        return false;
    }
    
private:
    bool backtrack(vector<vector<char>>& board, string& word, 
                   int row, int col, int index) {
        if (index == word.length()) return true;
        
        if (row < 0 || row >= board.size() || 
            col < 0 || col >= board[0].size() ||
            board[row][col] != word[index]) {
            return false;
        }
        
        char temp = board[row][col];
        board[row][col] = '#'; // Mark as visited
        
        // Explore all 4 directions
        bool found = backtrack(board, word, row + 1, col, index + 1) ||
                     backtrack(board, word, row - 1, col, index + 1) ||
                     backtrack(board, word, row, col + 1, index + 1) ||
                     backtrack(board, word, row, col - 1, index + 1);
        
        board[row][col] = temp; // Backtrack
        return found;
    }
};
```

#### 4. Generate Parentheses
```cpp
class GenerateParentheses {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> result;
        backtrack(result, "", 0, 0, n);
        return result;
    }
    
private:
    void backtrack(vector<string>& result, string current, 
                   int open, int close, int max) {
        if (current.length() == max * 2) {
            result.push_back(current);
            return;
        }
        
        if (open < max) {
            backtrack(result, current + "(", open + 1, close, max);
        }
        
        if (close < open) {
            backtrack(result, current + ")", open, close + 1, max);
        }
    }
};
```

#### 5. Permutations and Combinations
```cpp
class PermutationsCombinations {
public:
    // Generate all permutations
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> result;
        backtrackPermutations(nums, 0, result);
        return result;
    }
    
    // Generate all combinations of k elements
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> result;
        vector<int> current;
        backtrackCombinations(1, n, k, current, result);
        return result;
    }
    
    // Generate all subsets
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> result;
        vector<int> current;
        backtrackSubsets(nums, 0, current, result);
        return result;
    }
    
private:
    void backtrackPermutations(vector<int>& nums, int start, 
                              vector<vector<int>>& result) {
        if (start == nums.size()) {
            result.push_back(nums);
            return;
        }
        
        for (int i = start; i < nums.size(); i++) {
            swap(nums[start], nums[i]);
            backtrackPermutations(nums, start + 1, result);
            swap(nums[start], nums[i]); // Backtrack
        }
    }
    
    void backtrackCombinations(int start, int n, int k, 
                              vector<int>& current, 
                              vector<vector<int>>& result) {
        if (current.size() == k) {
            result.push_back(current);
            return;
        }
        
        for (int i = start; i <= n; i++) {
            current.push_back(i);
            backtrackCombinations(i + 1, n, k, current, result);
            current.pop_back(); // Backtrack
        }
    }
    
    void backtrackSubsets(vector<int>& nums, int start, 
                         vector<int>& current, 
                         vector<vector<int>>& result) {
        result.push_back(current);
        
        for (int i = start; i < nums.size(); i++) {
            current.push_back(nums[i]);
            backtrackSubsets(nums, i + 1, current, result);
            current.pop_back(); // Backtrack
        }
    }
};
```

## 🎯 Optimization Techniques

### 1. Memoization
Store results of expensive function calls.

```cpp
// Fibonacci with memoization
unordered_map<int, long long> fibMemo;

long long fibonacciMemoized(int n) {
    if (n <= 1) return n;
    
    if (fibMemo.count(n)) {
        return fibMemo[n];
    }
    
    fibMemo[n] = fibonacciMemoized(n - 1) + fibonacciMemoized(n - 2);
    return fibMemo[n];
}

// Climbing stairs with memoization
class ClimbingStairs {
private:
    unordered_map<int, int> memo;
    
public:
    int climbStairs(int n) {
        if (n <= 2) return n;
        
        if (memo.count(n)) return memo[n];
        
        memo[n] = climbStairs(n - 1) + climbStairs(n - 2);
        return memo[n];
    }
};
```

### 2. Pruning
Eliminate branches that cannot lead to solutions.

```cpp
// Optimized N-Queens with pruning
class NQueensPruned {
private:
    vector<bool> cols, diag1, diag2;
    
public:
    int totalNQueens(int n) {
        cols.resize(n, false);
        diag1.resize(2 * n - 1, false);
        diag2.resize(2 * n - 1, false);
        
        return backtrack(0, n);
    }
    
private:
    int backtrack(int row, int n) {
        if (row == n) return 1;
        
        int count = 0;
        for (int col = 0; col < n; col++) {
            int d1 = row - col + n - 1;
            int d2 = row + col;
            
            if (!cols[col] && !diag1[d1] && !diag2[d2]) {
                cols[col] = diag1[d1] = diag2[d2] = true;
                count += backtrack(row + 1, n);
                cols[col] = diag1[d1] = diag2[d2] = false;
            }
        }
        
        return count;
    }
};
```

## 🎯 Complex Backtracking Problems

### Problem 1: Palindrome Partitioning
```cpp
class PalindromePartitioning {
public:
    vector<vector<string>> partition(string s) {
        vector<vector<string>> result;
        vector<string> current;
        backtrack(s, 0, current, result);
        return result;
    }
    
private:
    void backtrack(string& s, int start, vector<string>& current, 
                   vector<vector<string>>& result) {
        if (start == s.length()) {
            result.push_back(current);
            return;
        }
        
        for (int end = start; end < s.length(); end++) {
            if (isPalindrome(s, start, end)) {
                current.push_back(s.substr(start, end - start + 1));
                backtrack(s, end + 1, current, result);
                current.pop_back();
            }
        }
    }
    
    bool isPalindrome(string& s, int left, int right) {
        while (left < right) {
            if (s[left] != s[right]) return false;
            left++;
            right--;
        }
        return true;
    }
};
```

### Problem 2: Expression Add Operators
```cpp
class ExpressionAddOperators {
public:
    vector<string> addOperators(string num, int target) {
        vector<string> result;
        backtrack(num, target, 0, 0, 0, "", result);
        return result;
    }
    
private:
    void backtrack(string& num, int target, int index, 
                   long long eval, long long mul, string expr, 
                   vector<string>& result) {
        if (index == num.length()) {
            if (eval == target) {
                result.push_back(expr);
            }
            return;
        }
        
        for (int i = index; i < num.length(); i++) {
            string current = num.substr(index, i - index + 1);
            
            if (current.length() > 1 && current[0] == '0') break;
            
            long long currentNum = stoll(current);
            
            if (index == 0) {
                backtrack(num, target, i + 1, currentNum, currentNum, 
                         current, result);
            } else {
                // Addition
                backtrack(num, target, i + 1, eval + currentNum, 
                         currentNum, expr + "+" + current, result);
                
                // Subtraction
                backtrack(num, target, i + 1, eval - currentNum, 
                         -currentNum, expr + "-" + current, result);
                
                // Multiplication
                backtrack(num, target, i + 1, eval - mul + mul * currentNum, 
                         mul * currentNum, expr + "*" + current, result);
            }
        }
    }
};
```

## 📊 Recursion vs Iteration

| Aspect | Recursion | Iteration |
|--------|-----------|-----------|
| **Readability** | Often cleaner for tree/graph problems | More explicit control flow |
| **Memory** | Uses call stack (O(depth)) | Usually O(1) extra space |
| **Performance** | Function call overhead | Generally faster |
| **Stack Overflow** | Risk with deep recursion | No risk |
| **Debugging** | Can be harder to trace | Easier to debug |

## 🎯 Practice Problems

### Easy Level
1. **Power of Two**: Check if number is power of 2 using recursion
2. **Reverse String**: Reverse string using recursion
3. **Sum of Digits**: Calculate sum of digits recursively
4. **Binary Tree Paths**: Find all root-to-leaf paths

### Medium Level
1. **Combination Sum**: Find combinations that sum to target
2. **Letter Combinations**: Phone number letter combinations
3. **Restore IP Addresses**: Generate valid IP addresses
4. **Unique Binary Search Trees**: Count structurally unique BSTs

### Hard Level
1. **Regular Expression Matching**: Implement regex with '.' and '*'
2. **Wildcard Matching**: Pattern matching with '?' and '*'
3. **N-Queens II**: Count solutions to N-Queens
4. **Sudoku Solver**: Solve any valid Sudoku puzzle

## ✅ Chapter Summary

### Key Concepts Mastered:
- **Recursion Fundamentals**: Base case, recursive case, progress
- **Tree Recursion**: Essential for hierarchical data structures
- **Backtracking**: Systematic search with pruning
- **Optimization**: Memoization and intelligent pruning
- **Problem Patterns**: Permutations, combinations, constraint satisfaction

### Recursion Design Process:
1. **Identify Base Case**: When does recursion stop?
2. **Define Recursive Relation**: How does problem break down?
3. **Ensure Progress**: Each call gets closer to base case
4. **Consider Optimization**: Memoization, tail recursion, pruning

### Backtracking Framework:
1. **Choose**: Make a decision
2. **Explore**: Recursively explore consequences
3. **Unchoose**: Backtrack if path doesn't work

### Master's Tips:
- **Draw recursion trees** to visualize the problem
- **Start simple** then optimize
- **Watch for stack overflow** with deep recursion
- **Use memoization** to avoid redundant calculations
- **Prune aggressively** in backtracking problems

## 🎯 Next Chapter Preview

In [Chapter 8: Linked Lists](../08-linked-lists/README.md), we'll master dynamic data structures, learning to manipulate pointers and solve complex problems involving chains of connected nodes.

---

**Recursion Master's Mantra**: "To understand recursion, you must first understand recursion. But seriously, break it down, solve the smallest case, and trust the process!"

[← Previous Chapter](../06-sorting/README.md) | [Next Chapter: Linked Lists →](../08-linked-lists/README.md)
