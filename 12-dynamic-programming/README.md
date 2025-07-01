# Chapter 12: Dynamic Programming - The Optimization Wizard

## Table of Contents
- [Introduction to Dynamic Programming](#introduction)
- [DP Fundamentals and Problem Identification](#fundamentals)
- [Memoization vs Tabulation](#memoization-vs-tabulation)
- [Classic DP Patterns](#classic-patterns)
- [Advanced DP Techniques](#advanced-techniques)
- [Three-Approach Methodology for DP](#three-approach)
- [Space Optimization Strategies](#space-optimization)
- [State Design and Transition](#state-design)
- [Practice Problems by Category](#practice-problems)

## Introduction to Dynamic Programming {#introduction}

Dynamic Programming (DP) is an algorithmic technique for solving complex problems by breaking them down into simpler subproblems and storing the results to avoid redundant calculations.

### Why DP Matters in DSA
- **Optimization Problems**: Finding optimal solutions efficiently
- **Combinatorial Problems**: Counting possibilities and arrangements
- **Decision Making**: Choosing best strategies under constraints
- **Interview Favorites**: Tests problem-solving and optimization skills
- **Real-World Applications**: Resource allocation, scheduling, bioinformatics

### DP Core Principles
```cpp
// Two essential properties for DP problems:

// 1. OPTIMAL SUBSTRUCTURE
// The optimal solution can be constructed from optimal solutions of subproblems
// Example: Shortest path from A to C through B = Shortest(A to B) + Shortest(B to C)

// 2. OVERLAPPING SUBPROBLEMS  
// The same subproblems are solved multiple times
// Example: Fibonacci - fib(n) = fib(n-1) + fib(n-2)
//          Both fib(n-1) and fib(n-2) compute many same values

// DP Approach:
// 1. Identify the problem has optimal substructure
// 2. Recognize overlapping subproblems
// 3. Design state representation
// 4. Define recurrence relation
// 5. Choose base cases
// 6. Implement (memoization or tabulation)
```

## DP Fundamentals and Problem Identification {#fundamentals}

### How to Recognize DP Problems
```cpp
// Common DP problem indicators:
// 1. "Find the maximum/minimum..."
// 2. "Count the number of ways..."
// 3. "Is it possible to reach..."
// 4. "What is the optimal way to..."
// 5. Problems involving choices at each step
// 6. Problems with overlapping substructures

// DP Problem Categories:
// 1. Linear DP (1D problems)
// 2. 2D DP (Grid problems)
// 3. Interval DP
// 4. Tree DP
// 5. Digit DP
// 6. Bitmask DP
// 7. Probability DP
```

### The DP Problem-Solving Framework
```cpp
class DPFramework {
public:
    // Step 1: Define the state
    // What does dp[i] or dp[i][j] represent?
    
    // Step 2: Find the recurrence relation
    // How to compute dp[i] from previous states?
    
    // Step 3: Identify base cases
    // What are the simplest cases we can solve directly?
    
    // Step 4: Determine computation order
    // In what order should we fill the DP table?
    
    // Step 5: Implement and optimize
    // Code the solution and optimize space if possible
    
    // Example: Fibonacci
    int fibonacci(int n) {
        // State: dp[i] = i-th Fibonacci number
        // Recurrence: dp[i] = dp[i-1] + dp[i-2]
        // Base cases: dp[0] = 0, dp[1] = 1
        // Order: 0 to n
        
        if (n <= 1) return n;
        
        vector<int> dp(n + 1);
        dp[0] = 0;
        dp[1] = 1;
        
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i-1] + dp[i-2];
        }
        
        return dp[n];
    }
};
```

## Memoization vs Tabulation {#memoization-vs-tabulation}

### Memoization (Top-Down Approach)
```cpp
class Memoization {
private:
    unordered_map<int, long long> memo;
    
public:
    // Fibonacci with memoization
    long long fibMemo(int n) {
        if (n <= 1) return n;
        
        if (memo.find(n) != memo.end()) {
            return memo[n];
        }
        
        memo[n] = fibMemo(n-1) + fibMemo(n-2);
        return memo[n];
    }
    
    // Coin Change with memoization
    int coinChangeMemo(vector<int>& coins, int amount) {
        unordered_map<int, int> memo;
        return coinChangeHelper(coins, amount, memo);
    }
    
private:
    int coinChangeHelper(vector<int>& coins, int amount, unordered_map<int, int>& memo) {
        if (amount == 0) return 0;
        if (amount < 0) return -1;
        
        if (memo.find(amount) != memo.end()) {
            return memo[amount];
        }
        
        int minCoins = INT_MAX;
        for (int coin : coins) {
            int result = coinChangeHelper(coins, amount - coin, memo);
            if (result != -1) {
                minCoins = min(minCoins, result + 1);
            }
        }
        
        memo[amount] = (minCoins == INT_MAX) ? -1 : minCoins;
        return memo[amount];
    }
    
    // Advantages of Memoization:
    // - Natural recursive thinking
    // - Only computes needed subproblems
    // - Easy to implement from recursive solution
    
    // Disadvantages:
    // - Function call overhead
    // - Risk of stack overflow for deep recursion
    // - Less cache-friendly memory access
};
```

### Tabulation (Bottom-Up Approach)
```cpp
class Tabulation {
public:
    // Fibonacci with tabulation
    long long fibTable(int n) {
        if (n <= 1) return n;
        
        vector<long long> dp(n + 1);
        dp[0] = 0;
        dp[1] = 1;
        
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i-1] + dp[i-2];
        }
        
        return dp[n];
    }
    
    // Coin Change with tabulation
    int coinChangeTable(vector<int>& coins, int amount) {
        vector<int> dp(amount + 1, amount + 1); // Initialize with impossible value
        dp[0] = 0;
        
        for (int i = 1; i <= amount; i++) {
            for (int coin : coins) {
                if (coin <= i) {
                    dp[i] = min(dp[i], dp[i - coin] + 1);
                }
            }
        }
        
        return dp[amount] > amount ? -1 : dp[amount];
    }
    
    // Longest Increasing Subsequence
    int lengthOfLIS(vector<int>& nums) {
        if (nums.empty()) return 0;
        
        vector<int> dp(nums.size(), 1);
        
        for (int i = 1; i < nums.size(); i++) {
            for (int j = 0; j < i; j++) {
                if (nums[j] < nums[i]) {
                    dp[i] = max(dp[i], dp[j] + 1);
                }
            }
        }
        
        return *max_element(dp.begin(), dp.end());
    }
    
    // Advantages of Tabulation:
    // - No function call overhead
    // - Better cache locality
    // - Avoids stack overflow
    // - Easier to optimize space
    
    // Disadvantages:
    // - Computes all subproblems (even unneeded ones)
    // - Less intuitive than recursion
    // - Need to figure out correct order
};
```

## Classic DP Patterns {#classic-patterns}

### 1. Linear DP (1D)
```cpp
class LinearDP {
public:
    // House Robber
    int rob(vector<int>& nums) {
        if (nums.empty()) return 0;
        if (nums.size() == 1) return nums[0];
        
        vector<int> dp(nums.size());
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        
        for (int i = 2; i < nums.size(); i++) {
            dp[i] = max(dp[i-1], dp[i-2] + nums[i]);
        }
        
        return dp[nums.size() - 1];
    }
    
    // Maximum Subarray (Kadane's Algorithm)
    int maxSubArray(vector<int>& nums) {
        int maxSoFar = nums[0];
        int maxEndingHere = nums[0];
        
        for (int i = 1; i < nums.size(); i++) {
            maxEndingHere = max(nums[i], maxEndingHere + nums[i]);
            maxSoFar = max(maxSoFar, maxEndingHere);
        }
        
        return maxSoFar;
    }
    
    // Climbing Stairs
    int climbStairs(int n) {
        if (n <= 2) return n;
        
        int prev2 = 1, prev1 = 2;
        for (int i = 3; i <= n; i++) {
            int current = prev1 + prev2;
            prev2 = prev1;
            prev1 = current;
        }
        
        return prev1;
    }
    
    // Decode Ways
    int numDecodings(string s) {
        if (s.empty() || s[0] == '0') return 0;
        
        vector<int> dp(s.length() + 1);
        dp[0] = 1; // Empty string
        dp[1] = 1; // First character
        
        for (int i = 2; i <= s.length(); i++) {
            // Single digit
            if (s[i-1] != '0') {
                dp[i] += dp[i-1];
            }
            
            // Two digits
            int twoDigit = stoi(s.substr(i-2, 2));
            if (twoDigit >= 10 && twoDigit <= 26) {
                dp[i] += dp[i-2];
            }
        }
        
        return dp[s.length()];
    }
};
```

### 2. Grid DP (2D)
```cpp
class GridDP {
public:
    // Unique Paths
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, 1));
        
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = dp[i-1][j] + dp[i][j-1];
            }
        }
        
        return dp[m-1][n-1];
    }
    
    // Minimum Path Sum
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n));
        
        dp[0][0] = grid[0][0];
        
        // First row
        for (int j = 1; j < n; j++) {
            dp[0][j] = dp[0][j-1] + grid[0][j];
        }
        
        // First column
        for (int i = 1; i < m; i++) {
            dp[i][0] = dp[i-1][0] + grid[i][0];
        }
        
        // Fill rest of the table
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                dp[i][j] = min(dp[i-1][j], dp[i][j-1]) + grid[i][j];
            }
        }
        
        return dp[m-1][n-1];
    }
    
    // Longest Common Subsequence
    int longestCommonSubsequence(string text1, string text2) {
        int m = text1.length(), n = text2.length();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (text1[i-1] == text2[j-1]) {
                    dp[i][j] = dp[i-1][j-1] + 1;
                } else {
                    dp[i][j] = max(dp[i-1][j], dp[i][j-1]);
                }
            }
        }
        
        return dp[m][n];
    }
    
    // Edit Distance
    int minDistance(string word1, string word2) {
        int m = word1.length(), n = word2.length();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1));
        
        // Base cases
        for (int i = 0; i <= m; i++) dp[i][0] = i;
        for (int j = 0; j <= n; j++) dp[0][j] = j;
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1[i-1] == word2[j-1]) {
                    dp[i][j] = dp[i-1][j-1];
                } else {
                    dp[i][j] = 1 + min({dp[i-1][j],    // Delete
                                       dp[i][j-1],    // Insert
                                       dp[i-1][j-1]}); // Replace
                }
            }
        }
        
        return dp[m][n];
    }
};
```

### 3. Knapsack Problems
```cpp
class KnapsackDP {
public:
    // 0/1 Knapsack
    int knapsack01(vector<int>& weights, vector<int>& values, int capacity) {
        int n = weights.size();
        vector<vector<int>> dp(n + 1, vector<int>(capacity + 1, 0));
        
        for (int i = 1; i <= n; i++) {
            for (int w = 1; w <= capacity; w++) {
                if (weights[i-1] <= w) {
                    dp[i][w] = max(dp[i-1][w], 
                                  dp[i-1][w - weights[i-1]] + values[i-1]);
                } else {
                    dp[i][w] = dp[i-1][w];
                }
            }
        }
        
        return dp[n][capacity];
    }
    
    // Unbounded Knapsack
    int knapsackUnbounded(vector<int>& weights, vector<int>& values, int capacity) {
        vector<int> dp(capacity + 1, 0);
        
        for (int w = 1; w <= capacity; w++) {
            for (int i = 0; i < weights.size(); i++) {
                if (weights[i] <= w) {
                    dp[w] = max(dp[w], dp[w - weights[i]] + values[i]);
                }
            }
        }
        
        return dp[capacity];
    }
    
    // Subset Sum
    bool canPartition(vector<int>& nums) {
        int sum = accumulate(nums.begin(), nums.end(), 0);
        if (sum % 2 != 0) return false;
        
        int target = sum / 2;
        vector<bool> dp(target + 1, false);
        dp[0] = true;
        
        for (int num : nums) {
            for (int j = target; j >= num; j--) {
                dp[j] = dp[j] || dp[j - num];
            }
        }
        
        return dp[target];
    }
    
    // Coin Change II (Number of ways)
    int change(int amount, vector<int>& coins) {
        vector<int> dp(amount + 1, 0);
        dp[0] = 1;
        
        for (int coin : coins) {
            for (int i = coin; i <= amount; i++) {
                dp[i] += dp[i - coin];
            }
        }
        
        return dp[amount];
    }
};
```

## Three-Approach Methodology for DP {#three-approach}

### Problem: Longest Increasing Subsequence

#### Approach 1: Brute Force (Recursive)
```cpp
int lisRecursive(vector<int>& nums) {
    return lisHelper(nums, 0, INT_MIN);
}

int lisHelper(vector<int>& nums, int index, int prevValue) {
    if (index >= nums.size()) return 0;
    
    // Option 1: Skip current element
    int skip = lisHelper(nums, index + 1, prevValue);
    
    // Option 2: Take current element (if valid)
    int take = 0;
    if (nums[index] > prevValue) {
        take = 1 + lisHelper(nums, index + 1, nums[index]);
    }
    
    return max(skip, take);
}
// Time: O(2^n), Space: O(n) - Exponential, not practical
```

#### Approach 2: DP (Memoization/Tabulation)
```cpp
// Memoization version
int lisMemo(vector<int>& nums) {
    map<pair<int, int>, int> memo;
    return lisMemoHelper(nums, 0, INT_MIN, memo);
}

int lisMemoHelper(vector<int>& nums, int index, int prevValue, 
                  map<pair<int, int>, int>& memo) {
    if (index >= nums.size()) return 0;
    
    pair<int, int> key = {index, prevValue};
    if (memo.find(key) != memo.end()) {
        return memo[key];
    }
    
    int skip = lisMemoHelper(nums, index + 1, prevValue, memo);
    int take = 0;
    if (nums[index] > prevValue) {
        take = 1 + lisMemoHelper(nums, index + 1, nums[index], memo);
    }
    
    memo[key] = max(skip, take);
    return memo[key];
}

// Tabulation version (O(n²))
int lisTabulation(vector<int>& nums) {
    if (nums.empty()) return 0;
    
    vector<int> dp(nums.size(), 1);
    
    for (int i = 1; i < nums.size(); i++) {
        for (int j = 0; j < i; j++) {
            if (nums[j] < nums[i]) {
                dp[i] = max(dp[i], dp[j] + 1);
            }
        }
    }
    
    return *max_element(dp.begin(), dp.end());
}
// Time: O(n²), Space: O(n) - Much better!
```

#### Approach 3: Optimized (Binary Search)
```cpp
int lisOptimal(vector<int>& nums) {
    vector<int> tails;
    
    for (int num : nums) {
        auto it = lower_bound(tails.begin(), tails.end(), num);
        if (it == tails.end()) {
            tails.push_back(num);
        } else {
            *it = num;
        }
    }
    
    return tails.size();
}
// Time: O(n log n), Space: O(n) - Optimal!

// The idea: tails[i] stores the smallest tail of all increasing 
// subsequences of length i+1
```

## Advanced DP Techniques {#advanced-techniques}

### 1. Bitmask DP
```cpp
class BitMaskDP {
public:
    // Traveling Salesman Problem
    int tsp(vector<vector<int>>& dist) {
        int n = dist.size();
        vector<vector<int>> dp(1 << n, vector<int>(n, INT_MAX));
        
        // Base case: start at city 0
        dp[1][0] = 0;
        
        for (int mask = 1; mask < (1 << n); mask++) {
            for (int u = 0; u < n; u++) {
                if (!(mask & (1 << u)) || dp[mask][u] == INT_MAX) continue;
                
                for (int v = 0; v < n; v++) {
                    if (mask & (1 << v)) continue;
                    
                    int newMask = mask | (1 << v);
                    dp[newMask][v] = min(dp[newMask][v], dp[mask][u] + dist[u][v]);
                }
            }
        }
        
        int result = INT_MAX;
        for (int i = 1; i < n; i++) {
            if (dp[(1 << n) - 1][i] != INT_MAX) {
                result = min(result, dp[(1 << n) - 1][i] + dist[i][0]);
            }
        }
        
        return result;
    }
    
    // Assign Tasks to Workers
    int assignTasks(vector<vector<int>>& cost) {
        int n = cost.size();
        vector<int> dp(1 << n, INT_MAX);
        dp[0] = 0;
        
        for (int mask = 0; mask < (1 << n); mask++) {
            if (dp[mask] == INT_MAX) continue;
            
            int worker = __builtin_popcount(mask);
            if (worker >= n) continue;
            
            for (int task = 0; task < n; task++) {
                if (!(mask & (1 << task))) {
                    int newMask = mask | (1 << task);
                    dp[newMask] = min(dp[newMask], dp[mask] + cost[worker][task]);
                }
            }
        }
        
        return dp[(1 << n) - 1];
    }
};
```

### 2. Digit DP
```cpp
class DigitDP {
private:
    string num;
    vector<vector<vector<int>>> memo;
    
public:
    // Count numbers with sum of digits divisible by K
    int countNumbers(int n, int k) {
        num = to_string(n);
        memo.assign(num.length(), vector<vector<int>>(k, vector<int>(2, -1)));
        return solve(0, 0, true);
    }
    
private:
    int solve(int pos, int sum, bool tight) {
        if (pos == num.length()) {
            return (sum % k == 0) ? 1 : 0;
        }
        
        if (memo[pos][sum][tight] != -1) {
            return memo[pos][sum][tight];
        }
        
        int limit = tight ? (num[pos] - '0') : 9;
        int result = 0;
        
        for (int digit = 0; digit <= limit; digit++) {
            bool newTight = tight && (digit == limit);
            result += solve(pos + 1, (sum + digit) % k, newTight);
        }
        
        return memo[pos][sum][tight] = result;
    }
};
```

### 3. Tree DP
```cpp
struct TreeNode {
    int val;
    vector<TreeNode*> children;
    TreeNode(int x) : val(x) {}
};

class TreeDP {
public:
    // Maximum sum with no two adjacent nodes
    int rob(TreeNode* root) {
        auto result = robHelper(root);
        return max(result.first, result.second);
    }
    
private:
    pair<int, int> robHelper(TreeNode* node) {
        if (!node) return {0, 0};
        
        int withNode = node->val;
        int withoutNode = 0;
        
        for (TreeNode* child : node->children) {
            auto childResult = robHelper(child);
            withNode += childResult.second; // Can't take child if taking node
            withoutNode += max(childResult.first, childResult.second);
        }
        
        return {withNode, withoutNode};
    }
    
    // Diameter of tree
    int diameter = 0;
    
    int treeDiameter(TreeNode* root) {
        diameter = 0;
        dfs(root);
        return diameter;
    }
    
    int dfs(TreeNode* node) {
        if (!node) return 0;
        
        vector<int> childHeights;
        for (TreeNode* child : node->children) {
            childHeights.push_back(dfs(child));
        }
        
        sort(childHeights.rbegin(), childHeights.rend());
        
        if (childHeights.size() >= 2) {
            diameter = max(diameter, childHeights[0] + childHeights[1]);
        } else if (childHeights.size() == 1) {
            diameter = max(diameter, childHeights[0]);
        }
        
        return childHeights.empty() ? 1 : childHeights[0] + 1;
    }
};
```

## Space Optimization Strategies {#space-optimization}

### Rolling Array Technique
```cpp
class SpaceOptimization {
public:
    // Fibonacci with O(1) space
    int fibOptimized(int n) {
        if (n <= 1) return n;
        
        int prev2 = 0, prev1 = 1;
        for (int i = 2; i <= n; i++) {
            int current = prev1 + prev2;
            prev2 = prev1;
            prev1 = current;
        }
        
        return prev1;
    }
    
    // 0/1 Knapsack with O(capacity) space
    int knapsackOptimized(vector<int>& weights, vector<int>& values, int capacity) {
        vector<int> dp(capacity + 1, 0);
        
        for (int i = 0; i < weights.size(); i++) {
            for (int w = capacity; w >= weights[i]; w--) {
                dp[w] = max(dp[w], dp[w - weights[i]] + values[i]);
            }
        }
        
        return dp[capacity];
    }
    
    // LCS with O(min(m,n)) space
    int lcsOptimized(string text1, string text2) {
        if (text1.length() > text2.length()) {
            swap(text1, text2);
        }
        
        vector<int> prev(text1.length() + 1, 0);
        vector<int> curr(text1.length() + 1, 0);
        
        for (int i = 1; i <= text2.length(); i++) {
            for (int j = 1; j <= text1.length(); j++) {
                if (text1[j-1] == text2[i-1]) {
                    curr[j] = prev[j-1] + 1;
                } else {
                    curr[j] = max(prev[j], curr[j-1]);
                }
            }
            prev = curr;
            fill(curr.begin(), curr.end(), 0);
        }
        
        return prev[text1.length()];
    }
    
    // Unique Paths with O(min(m,n)) space
    int uniquePathsOptimized(int m, int n) {
        if (m > n) swap(m, n);
        
        vector<int> dp(m, 1);
        
        for (int i = 1; i < n; i++) {
            for (int j = 1; j < m; j++) {
                dp[j] += dp[j-1];
            }
        }
        
        return dp[m-1];
    }
};
```

## State Design and Transition {#state-design}

### Complex State Management
```cpp
class StateDesign {
public:
    // Stock problems with multiple states
    // Best Time to Buy and Sell Stock with Transaction Fee
    int maxProfitWithFee(vector<int>& prices, int fee) {
        int buy = -prices[0]; // Max profit when holding stock
        int sell = 0;         // Max profit when not holding stock
        
        for (int i = 1; i < prices.size(); i++) {
            int newBuy = max(buy, sell - prices[i]);
            int newSell = max(sell, buy + prices[i] - fee);
            buy = newBuy;
            sell = newSell;
        }
        
        return sell;
    }
    
    // Best Time to Buy and Sell Stock with Cooldown
    int maxProfitWithCooldown(vector<int>& prices) {
        if (prices.size() <= 1) return 0;
        
        int buy = -prices[0];  // Max profit when holding stock
        int sell = 0;          // Max profit when not holding stock (can buy)
        int rest = 0;          // Max profit when not holding stock (cooldown)
        
        for (int i = 1; i < prices.size(); i++) {
            int newBuy = max(buy, rest - prices[i]);
            int newSell = buy + prices[i];
            int newRest = max(rest, sell);
            
            buy = newBuy;
            sell = newSell;
            rest = newRest;
        }
        
        return max(sell, rest);
    }
    
    // Palindrome Partitioning II
    int minCut(string s) {
        int n = s.length();
        vector<vector<bool>> isPalindrome(n, vector<bool>(n, false));
        
        // Precompute palindromes
        for (int i = 0; i < n; i++) {
            for (int j = 0; j <= i; j++) {
                if (s[i] == s[j] && (i - j <= 2 || isPalindrome[j+1][i-1])) {
                    isPalindrome[j][i] = true;
                }
            }
        }
        
        vector<int> dp(n, INT_MAX);
        for (int i = 0; i < n; i++) {
            if (isPalindrome[0][i]) {
                dp[i] = 0;
            } else {
                for (int j = 0; j < i; j++) {
                    if (isPalindrome[j+1][i]) {
                        dp[i] = min(dp[i], dp[j] + 1);
                    }
                }
            }
        }
        
        return dp[n-1];
    }
};
```

## Practice Problems by Category {#practice-problems}

### Easy Level - Linear DP
1. **Climbing Stairs** (Basic DP)
2. **House Robber** (Skip pattern)
3. **Maximum Subarray** (Kadane's algorithm)
4. **Min Cost Climbing Stairs** (Choice at each step)
5. **Fibonacci Number** (Classic example)
6. **N-th Tribonacci Number** (Extension of Fibonacci)

### Easy Level - 2D DP
1. **Unique Paths** (Grid navigation)
2. **Minimum Path Sum** (Grid with costs)
3. **Range Sum Query 2D** (Prefix sums)

### Medium Level - Classical Patterns
1. **Coin Change** (Unbounded knapsack)
2. **Longest Increasing Subsequence** (3 approaches)
3. **Longest Common Subsequence** (2D DP)
4. **Edit Distance** (String DP)
5. **0/1 Knapsack** (Classic problem)
6. **Partition Equal Subset Sum** (Subset sum)
7. **Target Sum** (Assignment problem)
8. **Decode Ways** (Linear DP with constraints)
9. **Word Break** (String segmentation)
10. **Palindromic Substrings** (String DP)

### Medium Level - Advanced 2D
1. **Longest Palindromic Subsequence** (String DP)
2. **Minimum Window Subsequence** (Two pointers + DP)
3. **Interleaving String** (3D to 2D optimization)
4. **Distinct Subsequences** (String matching)
5. **Regular Expression Matching** (Complex transitions)
6. **Wildcard Matching** (Pattern matching)

### Hard Level - Complex States
1. **Best Time to Buy and Sell Stock** (All variations)
2. **Burst Balloons** (Interval DP)
3. **Remove Boxes** (3D DP)
4. **Strange Printer** (Interval DP)
5. **Minimum Cost to Cut a Stick** (Interval DP)
6. **Stone Game** (Game theory DP)
7. **Palindrome Partitioning II** (String + optimization)

### Hard Level - Advanced Techniques
1. **Count Different Palindromic Subsequences** (String DP)
2. **Number of Ways to Paint N × 3 Grid** (State machine DP)
3. **Cherry Pickup** (Path DP)
4. **Minimum Difficulty of Job Schedule** (Optimization DP)
5. **Frog Jump** (Position-dependent DP)

### Competitive Programming - Expert Level
1. **Digit DP Problems** (Count numbers with properties)
2. **Bitmask DP** (TSP, assignment problems)
3. **Tree DP** (Rerooting technique)
4. **DP on DAGs** (Topological order)
5. **Probability DP** (Expected value problems)
6. **Matrix Exponentiation** (Linear recurrence optimization)

## Key Takeaways

1. **Problem Recognition**: Learn to identify DP problems by their characteristics
2. **State Design**: Choose the right state representation
3. **Recurrence Relations**: Master the art of breaking problems into subproblems
4. **Implementation Choice**: Understand when to use memoization vs tabulation
5. **Space Optimization**: Always consider if you can reduce space complexity
6. **Multiple Approaches**: Practice the three-approach methodology
7. **Pattern Recognition**: Master classical patterns and their variations

## Next Steps in Your DP Journey

1. **Practice Regularly**: Solve problems from each category
2. **Optimize Solutions**: Always try to optimize space after getting correct solution
3. **Understand Patterns**: Recognize when problems are variations of known patterns
4. **Build Intuition**: Practice enough to develop DP intuition
5. **Advanced Topics**: Explore digit DP, bitmask DP, and probability DP

## Chapter Summary

Dynamic Programming is the art of breaking complex problems into simpler ones and remembering the solutions. Master the fundamentals, recognize patterns, and practice the three-approach methodology. With consistent practice, you'll develop the intuition to tackle even the most challenging optimization problems.

Remember: Every DP expert was once a beginner who kept practicing! 🚀

---
*"Dynamic Programming teaches us that sometimes the best way forward is to remember where we've been, and that optimal solutions are built step by step from smaller optimal choices."*
