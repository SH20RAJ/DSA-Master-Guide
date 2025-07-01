# 🎯 Sample Problem Collection: Two Sum Variations

This file demonstrates the **Three-Approach Methodology** used throughout this book. For every problem, we explore multiple solutions from brute force to optimal.

---

## 📋 Problem 1: Classic Two Sum

**Problem Statement**: Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

**Example**:
```
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: nums[0] + nums[1] = 2 + 7 = 9
```

**Constraints**:
- 2 ≤ nums.length ≤ 10⁴
- -10⁹ ≤ nums[i] ≤ 10⁹
- -10⁹ ≤ target ≤ 10⁹
- Only one valid answer exists

---

### 🐌 Approach 1: Brute Force
**Strategy**: Check every pair of numbers.

```cpp
class Solution {
public:
    vector<int> twoSumBruteForce(vector<int>& nums, int target) {
        int n = nums.size();
        
        // Check every pair
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                if (nums[i] + nums[j] == target) {
                    return {i, j};
                }
            }
        }
        
        return {}; // No solution found (shouldn't happen per constraints)
    }
};
```

**Complexity Analysis**:
- **Time**: O(n²) - We check all pairs
- **Space**: O(1) - Only using constant extra space

**Pros**: 
- Simple to understand and implement
- No extra space needed

**Cons**: 
- Slow for large arrays
- Not suitable for competitive programming

---

### ⚡ Approach 2: Two-Pass Hash Map
**Strategy**: Use hash map to store values and their indices, then look for complements.

```cpp
class Solution {
public:
    vector<int> twoSumTwoPass(vector<int>& nums, int target) {
        unordered_map<int, int> numToIndex;
        
        // First pass: store all numbers and their indices
        for (int i = 0; i < nums.size(); i++) {
            numToIndex[nums[i]] = i;
        }
        
        // Second pass: look for complement
        for (int i = 0; i < nums.size(); i++) {
            int complement = target - nums[i];
            
            // Check if complement exists and is not the same element
            if (numToIndex.count(complement) && numToIndex[complement] != i) {
                return {i, numToIndex[complement]};
            }
        }
        
        return {};
    }
};
```

**Complexity Analysis**:
- **Time**: O(n) - Two passes through the array
- **Space**: O(n) - Hash map stores all elements

**Pros**: 
- Much faster than brute force
- Easy to understand

**Cons**: 
- Uses extra space
- Makes two passes (can be optimized)

---

### 🚀 Approach 3: One-Pass Hash Map (Optimal)
**Strategy**: Build hash map while searching for complements.

```cpp
class Solution {
public:
    vector<int> twoSumOptimal(vector<int>& nums, int target) {
        unordered_map<int, int> numToIndex;
        
        for (int i = 0; i < nums.size(); i++) {
            int complement = target - nums[i];
            
            // Check if complement exists in map
            if (numToIndex.count(complement)) {
                return {numToIndex[complement], i};
            }
            
            // Add current number to map
            numToIndex[nums[i]] = i;
        }
        
        return {};
    }
};
```

**Complexity Analysis**:
- **Time**: O(n) - Single pass through array
- **Space**: O(n) - Hash map in worst case

**Pros**: 
- Optimal time complexity
- Single pass through array
- Clean and efficient

**Cons**: 
- Uses extra space (unavoidable for this approach)

---

## 📋 Problem 2: Two Sum II - Input Array is Sorted

**Problem Statement**: Given a 1-indexed array of integers `numbers` that is already sorted in non-decreasing order, find two numbers such that they add up to a specific `target` number.

**Example**:
```
Input: numbers = [2,7,11,15], target = 9
Output: [1,2]
Explanation: numbers[1] + numbers[2] = 2 + 7 = 9
```

---

### 🐌 Approach 1: Brute Force
**Strategy**: Same as before, check all pairs.

```cpp
class Solution {
public:
    vector<int> twoSumBruteForce(vector<int>& numbers, int target) {
        int n = numbers.size();
        
        for (int i = 0; i < n - 1; i++) {
            for (int j = i + 1; j < n; j++) {
                if (numbers[i] + numbers[j] == target) {
                    return {i + 1, j + 1}; // 1-indexed
                }
                // Early termination since array is sorted
                if (numbers[i] + numbers[j] > target) {
                    break;
                }
            }
        }
        
        return {};
    }
};
```

**Complexity Analysis**:
- **Time**: O(n²) worst case, but early termination helps
- **Space**: O(1)

---

### ⚡ Approach 2: Hash Map
**Strategy**: Use the sorted property but still use hash map.

```cpp
class Solution {
public:
    vector<int> twoSumHashMap(vector<int>& numbers, int target) {
        unordered_map<int, int> numToIndex;
        
        for (int i = 0; i < numbers.size(); i++) {
            int complement = target - numbers[i];
            
            if (numToIndex.count(complement)) {
                return {numToIndex[complement] + 1, i + 1}; // 1-indexed
            }
            
            numToIndex[numbers[i]] = i;
        }
        
        return {};
    }
};
```

**Complexity Analysis**:
- **Time**: O(n)
- **Space**: O(n)

---

### 🚀 Approach 3: Two Pointers (Optimal)
**Strategy**: Use sorted property with two pointers technique.

```cpp
class Solution {
public:
    vector<int> twoSumOptimal(vector<int>& numbers, int target) {
        int left = 0, right = numbers.size() - 1;
        
        while (left < right) {
            int sum = numbers[left] + numbers[right];
            
            if (sum == target) {
                return {left + 1, right + 1}; // 1-indexed
            } else if (sum < target) {
                left++;  // Need larger sum
            } else {
                right--; // Need smaller sum
            }
        }
        
        return {};
    }
};
```

**Complexity Analysis**:
- **Time**: O(n) - Single pass with two pointers
- **Space**: O(1) - Only using two pointers

**Why This is Optimal for Sorted Arrays**:
- Takes advantage of sorted property
- No extra space needed
- Cannot be improved further

---

## 📋 Problem 3: 3Sum

**Problem Statement**: Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

**Example**:
```
Input: nums = [-1,0,1,2,-1,-4]
Output: [[-1,-1,2],[-1,0,1]]
```

---

### 🐌 Approach 1: Brute Force
**Strategy**: Check all possible triplets.

```cpp
class Solution {
public:
    vector<vector<int>> threeSumBruteForce(vector<int>& nums) {
        set<vector<int>> resultSet; // Use set to avoid duplicates
        int n = nums.size();
        
        for (int i = 0; i < n - 2; i++) {
            for (int j = i + 1; j < n - 1; j++) {
                for (int k = j + 1; k < n; k++) {
                    if (nums[i] + nums[j] + nums[k] == 0) {
                        vector<int> triplet = {nums[i], nums[j], nums[k]};
                        sort(triplet.begin(), triplet.end());
                        resultSet.insert(triplet);
                    }
                }
            }
        }
        
        return vector<vector<int>>(resultSet.begin(), resultSet.end());
    }
};
```

**Complexity Analysis**:
- **Time**: O(n³ log n) - Three nested loops + sorting for dedup
- **Space**: O(number of triplets) for result

---

### ⚡ Approach 2: Sort + Hash Set
**Strategy**: Sort array and use hash set for two sum.

```cpp
class Solution {
public:
    vector<vector<int>> threeSumHashSet(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> result;
        int n = nums.size();
        
        for (int i = 0; i < n - 2; i++) {
            // Skip duplicates for first element
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            
            unordered_set<int> seen;
            int target = -nums[i];
            
            for (int j = i + 1; j < n; j++) {
                int complement = target - nums[j];
                
                if (seen.count(complement)) {
                    result.push_back({nums[i], complement, nums[j]});
                    
                    // Skip duplicates for second element
                    while (j + 1 < n && nums[j] == nums[j + 1]) j++;
                }
                
                seen.insert(nums[j]);
            }
        }
        
        return result;
    }
};
```

**Complexity Analysis**:
- **Time**: O(n² + n log n) = O(n²)
- **Space**: O(n) for hash set

---

### 🚀 Approach 3: Sort + Two Pointers (Optimal)
**Strategy**: Sort array and use two pointers for each fixed element.

```cpp
class Solution {
public:
    vector<vector<int>> threeSumOptimal(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        vector<vector<int>> result;
        int n = nums.size();
        
        for (int i = 0; i < n - 2; i++) {
            // Skip duplicates for first element
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            
            int left = i + 1, right = n - 1;
            int target = -nums[i];
            
            while (left < right) {
                int sum = nums[left] + nums[right];
                
                if (sum == target) {
                    result.push_back({nums[i], nums[left], nums[right]});
                    
                    // Skip duplicates for second element
                    while (left < right && nums[left] == nums[left + 1]) left++;
                    // Skip duplicates for third element
                    while (left < right && nums[right] == nums[right - 1]) right--;
                    
                    left++;
                    right--;
                } else if (sum < target) {
                    left++;
                } else {
                    right--;
                }
            }
        }
        
        return result;
    }
};
```

**Complexity Analysis**:
- **Time**: O(n²) - O(n log n) for sorting + O(n²) for main algorithm
- **Space**: O(1) extra space (not counting output)

**Why This is Optimal**:
- No extra space for computation
- Handles duplicates elegantly
- Cannot improve time complexity further for this problem

---

## 🎯 Key Learning Points

### **Problem-Solving Framework**:
1. **Understand**: Read problem carefully, identify constraints
2. **Brute Force**: Start with simplest solution
3. **Optimize**: Look for patterns, data structure opportunities
4. **Implement**: Write clean, bug-free code
5. **Test**: Verify with examples and edge cases

### **Common Optimization Techniques**:
- **Hash Maps**: Trade space for time
- **Two Pointers**: Exploit sorted data
- **Sorting**: Enable other optimizations
- **Early Termination**: Skip unnecessary work

### **Complexity Trade-offs**:
- **Time vs Space**: Often can improve one at cost of other
- **Simplicity vs Efficiency**: Brute force is simple but slow
- **Preprocessing**: Sometimes worth sorting for better algorithm

### **Interview Tips**:
1. **Always start with brute force** - shows you can solve the problem
2. **Explain your thinking** - walk through your optimization process
3. **Consider constraints** - they often hint at expected complexity
4. **Handle edge cases** - empty arrays, single elements, duplicates
5. **Write clean code** - proper variable names, clear logic

---

## 🔥 Practice Problems

Try solving these variations using the three-approach methodology:

### Easy
1. **Two Sum IV - Input is a BST**: Find pair in BST that sums to target
2. **Two Sum Less Than K**: Find pair with maximum sum less than K
3. **Valid Perfect Square**: Check if number is perfect square

### Medium  
1. **4Sum**: Find all unique quadruplets that sum to target
2. **3Sum Closest**: Find triplet closest to target sum
3. **Two Sum III - Data Structure Design**: Design add/find operations

### Hard
1. **4Sum II**: Count tuples from four arrays that sum to zero
2. **Subarray Sum Equals K**: Count subarrays with given sum
3. **Max Number of K-Sum Pairs**: Maximize pairs with sum K

---

**Remember**: "Every expert was once a beginner who refused to give up. Practice the three-approach methodology until it becomes second nature!"

---

[← Back to Main Book](../README.md) | [Start Chapter 1 →](../01-introduction/README.md)
