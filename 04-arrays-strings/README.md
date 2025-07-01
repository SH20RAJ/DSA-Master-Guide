# Chapter 4: Arrays & Strings Mastery 🎯

Master the foundation of all data structures - Arrays and Strings!

## 🎯 Learning Objectives
- Master array manipulation techniques
- Understand string algorithms and patterns
- Learn sliding window and two-pointer techniques
- Solve complex problems with multiple approaches

## 📋 Arrays: The Foundation

Arrays are contiguous memory locations that store elements of the same type.

### Array Fundamentals
```cpp
// Declaration and Initialization
int arr[5];                    // Fixed size array
int arr[] = {1, 2, 3, 4, 5};   // Initialize with values
int arr[5] = {1, 2};           // Partially initialized (rest are 0)

// Dynamic arrays using vectors
vector<int> v;                 // Empty vector
vector<int> v(10);             // Size 10, initialized to 0
vector<int> v(10, 5);          // Size 10, all elements = 5
vector<int> v = {1, 2, 3, 4, 5}; // Initialize with values

// 2D Arrays
int matrix[3][4];              // 3x4 matrix
vector<vector<int>> matrix(3, vector<int>(4, 0)); // Dynamic 2D vector
```

### Essential Array Operations
```cpp
// Traversal
void printArray(vector<int>& arr) {
    for (int i = 0; i < arr.size(); i++) {
        cout << arr[i] << " ";
    }
    cout << endl;
}

// Insertion at end: O(1) amortized
void insertEnd(vector<int>& arr, int value) {
    arr.push_back(value);
}

// Insertion at position: O(n)
void insertAt(vector<int>& arr, int pos, int value) {
    arr.insert(arr.begin() + pos, value);
}

// Deletion from end: O(1)
void deleteEnd(vector<int>& arr) {
    if (!arr.empty()) {
        arr.pop_back();
    }
}

// Deletion from position: O(n)
void deleteAt(vector<int>& arr, int pos) {
    if (pos >= 0 && pos < arr.size()) {
        arr.erase(arr.begin() + pos);
    }
}

// Search: O(n) for unsorted, O(log n) for sorted
int linearSearch(vector<int>& arr, int target) {
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] == target) return i;
    }
    return -1;
}

int binarySearch(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    return -1;
}
```

## 🏹 Core Array Techniques

### 1. Two Pointer Technique
Used for problems involving pairs, subarrays, or when you need to scan from both ends.

```cpp
// Example: Check if array is palindrome
bool isPalindrome(vector<int>& arr) {
    int left = 0, right = arr.size() - 1;
    
    while (left < right) {
        if (arr[left] != arr[right]) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}

// Example: Two Sum in sorted array
vector<int> twoSumSorted(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    
    while (left < right) {
        int sum = arr[left] + arr[right];
        if (sum == target) {
            return {left, right};
        } else if (sum < target) {
            left++;
        } else {
            right--;
        }
    }
    return {};
}

// Example: Remove duplicates from sorted array
int removeDuplicates(vector<int>& arr) {
    if (arr.empty()) return 0;
    
    int writeIndex = 1;
    for (int readIndex = 1; readIndex < arr.size(); readIndex++) {
        if (arr[readIndex] != arr[readIndex - 1]) {
            arr[writeIndex] = arr[readIndex];
            writeIndex++;
        }
    }
    return writeIndex;
}
```

### 2. Sliding Window Technique
Perfect for subarray problems with a fixed or variable window size.

```cpp
// Fixed Window: Maximum sum of k consecutive elements
int maxSumFixedWindow(vector<int>& arr, int k) {
    if (arr.size() < k) return -1;
    
    // Calculate sum of first window
    int windowSum = 0;
    for (int i = 0; i < k; i++) {
        windowSum += arr[i];
    }
    
    int maxSum = windowSum;
    
    // Slide the window
    for (int i = k; i < arr.size(); i++) {
        windowSum += arr[i] - arr[i - k];  // Add new, remove old
        maxSum = max(maxSum, windowSum);
    }
    
    return maxSum;
}

// Variable Window: Longest subarray with sum ≤ target
int longestSubarrayWithSum(vector<int>& arr, int target) {
    int left = 0, sum = 0, maxLength = 0;
    
    for (int right = 0; right < arr.size(); right++) {
        sum += arr[right];
        
        // Shrink window if sum exceeds target
        while (sum > target && left <= right) {
            sum -= arr[left];
            left++;
        }
        
        maxLength = max(maxLength, right - left + 1);
    }
    
    return maxLength;
}

// Variable Window: Longest substring with at most k distinct characters
int longestSubstringKDistinct(string s, int k) {
    if (k == 0) return 0;
    
    unordered_map<char, int> charCount;
    int left = 0, maxLength = 0;
    
    for (int right = 0; right < s.length(); right++) {
        charCount[s[right]]++;
        
        // Shrink window if more than k distinct characters
        while (charCount.size() > k) {
            charCount[s[left]]--;
            if (charCount[s[left]] == 0) {
                charCount.erase(s[left]);
            }
            left++;
        }
        
        maxLength = max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

### 3. Prefix Sum Technique
Efficient for range sum queries and subarray problems.

```cpp
// Basic Prefix Sum
vector<int> buildPrefixSum(vector<int>& arr) {
    vector<int> prefixSum(arr.size() + 1, 0);
    for (int i = 0; i < arr.size(); i++) {
        prefixSum[i + 1] = prefixSum[i] + arr[i];
    }
    return prefixSum;
}

// Range sum query: O(1) after O(n) preprocessing
int rangeSum(vector<int>& prefixSum, int left, int right) {
    return prefixSum[right + 1] - prefixSum[left];
}

// Subarray sum equals k
int subarraySum(vector<int>& arr, int k) {
    unordered_map<int, int> prefixSumCount;
    prefixSumCount[0] = 1;  // Empty prefix
    
    int currentSum = 0, count = 0;
    for (int num : arr) {
        currentSum += num;
        
        // Check if (currentSum - k) exists
        if (prefixSumCount.count(currentSum - k)) {
            count += prefixSumCount[currentSum - k];
        }
        
        prefixSumCount[currentSum]++;
    }
    
    return count;
}
```

## 📝 String Algorithms

Strings are arrays of characters with special properties and operations.

### String Fundamentals
```cpp
// String creation and manipulation
string s1 = "Hello";
string s2("World");
string s3(5, 'A');        // "AAAAA"
string s4 = s1 + " " + s2; // "Hello World"

// Common string operations
s1.length();              // or s1.size()
s1.empty();
s1.substr(1, 3);         // Substring from index 1, length 3
s1.find("llo");          // Find substring
s1.replace(1, 2, "i");   // Replace
s1.push_back('!');       // Add character
s1.pop_back();           // Remove last character

// Character operations
char c = 'A';
islower(c);              // Check if lowercase
isupper(c);              // Check if uppercase
isdigit(c);              // Check if digit
tolower(c);              // Convert to lowercase
toupper(c);              // Convert to uppercase
```

### String Pattern Matching

#### 1. Naive String Matching - O(nm)
```cpp
vector<int> naiveSearch(string text, string pattern) {
    vector<int> matches;
    int n = text.length(), m = pattern.length();
    
    for (int i = 0; i <= n - m; i++) {
        int j;
        for (j = 0; j < m; j++) {
            if (text[i + j] != pattern[j]) {
                break;
            }
        }
        if (j == m) {
            matches.push_back(i);
        }
    }
    
    return matches;
}
```

#### 2. KMP Algorithm - O(n + m)
```cpp
vector<int> buildLPS(string pattern) {
    int m = pattern.length();
    vector<int> lps(m, 0);
    int len = 0, i = 1;
    
    while (i < m) {
        if (pattern[i] == pattern[len]) {
            len++;
            lps[i] = len;
            i++;
        } else {
            if (len != 0) {
                len = lps[len - 1];
            } else {
                lps[i] = 0;
                i++;
            }
        }
    }
    
    return lps;
}

vector<int> KMPSearch(string text, string pattern) {
    vector<int> matches;
    vector<int> lps = buildLPS(pattern);
    
    int n = text.length(), m = pattern.length();
    int i = 0, j = 0;
    
    while (i < n) {
        if (pattern[j] == text[i]) {
            i++;
            j++;
        }
        
        if (j == m) {
            matches.push_back(i - j);
            j = lps[j - 1];
        } else if (i < n && pattern[j] != text[i]) {
            if (j != 0) {
                j = lps[j - 1];
            } else {
                i++;
            }
        }
    }
    
    return matches;
}
```

### String Manipulation Techniques

#### 1. Palindrome Checking
```cpp
// Approach 1: Two pointers - O(n) time, O(1) space
bool isPalindromeSimple(string s) {
    int left = 0, right = s.length() - 1;
    
    while (left < right) {
        if (s[left] != s[right]) {
            return false;
        }
        left++;
        right--;
    }
    return true;
}

// Approach 2: Ignore non-alphanumeric - O(n) time, O(1) space
bool isPalindromeAlphanumeric(string s) {
    int left = 0, right = s.length() - 1;
    
    while (left < right) {
        while (left < right && !isalnum(s[left])) left++;
        while (left < right && !isalnum(s[right])) right--;
        
        if (tolower(s[left]) != tolower(s[right])) {
            return false;
        }
        
        left++;
        right--;
    }
    return true;
}

// Find longest palindromic substring
string longestPalindrome(string s) {
    if (s.empty()) return "";
    
    int start = 0, maxLength = 1;
    
    auto expandAroundCenter = [&](int left, int right) {
        while (left >= 0 && right < s.length() && s[left] == s[right]) {
            int currentLength = right - left + 1;
            if (currentLength > maxLength) {
                start = left;
                maxLength = currentLength;
            }
            left--;
            right++;
        }
    };
    
    for (int i = 0; i < s.length(); i++) {
        expandAroundCenter(i, i);     // Odd length palindromes
        expandAroundCenter(i, i + 1); // Even length palindromes
    }
    
    return s.substr(start, maxLength);
}
```

#### 2. Anagram Detection
```cpp
// Approach 1: Sorting - O(n log n) time, O(1) space
bool isAnagramSort(string s1, string s2) {
    if (s1.length() != s2.length()) return false;
    
    sort(s1.begin(), s1.end());
    sort(s2.begin(), s2.end());
    
    return s1 == s2;
}

// Approach 2: Character counting - O(n) time, O(1) space
bool isAnagramCount(string s1, string s2) {
    if (s1.length() != s2.length()) return false;
    
    vector<int> count(26, 0);
    
    for (int i = 0; i < s1.length(); i++) {
        count[s1[i] - 'a']++;
        count[s2[i] - 'a']--;
    }
    
    for (int c : count) {
        if (c != 0) return false;
    }
    
    return true;
}

// Group anagrams together
vector<vector<string>> groupAnagrams(vector<string>& strs) {
    unordered_map<string, vector<string>> groups;
    
    for (string s : strs) {
        string key = s;
        sort(key.begin(), key.end());
        groups[key].push_back(s);
    }
    
    vector<vector<string>> result;
    for (auto& group : groups) {
        result.push_back(group.second);
    }
    
    return result;
}
```

## 🎯 Advanced Problems with Three Approaches

### Problem 1: Container With Most Water

**Problem**: Given heights of vertical lines, find two lines that form a container with maximum water.

```cpp
// Approach 1: Brute Force - O(n²) time, O(1) space
int maxAreaBrute(vector<int>& height) {
    int maxArea = 0;
    int n = height.size();
    
    for (int i = 0; i < n; i++) {
        for (int j = i + 1; j < n; j++) {
            int area = min(height[i], height[j]) * (j - i);
            maxArea = max(maxArea, area);
        }
    }
    
    return maxArea;
}

// Approach 2: Two Pointers - O(n) time, O(1) space
int maxAreaOptimal(vector<int>& height) {
    int left = 0, right = height.size() - 1;
    int maxArea = 0;
    
    while (left < right) {
        int area = min(height[left], height[right]) * (right - left);
        maxArea = max(maxArea, area);
        
        // Move the pointer with smaller height
        if (height[left] < height[right]) {
            left++;
        } else {
            right--;
        }
    }
    
    return maxArea;
}
```

**Analysis**:
- **Brute Force**: Check all pairs - simple but slow
- **Two Pointers**: Move inward, always move shorter line - optimal!

### Problem 2: Longest Substring Without Repeating Characters

**Problem**: Find length of longest substring without repeating characters.

```cpp
// Approach 1: Brute Force - O(n³) time, O(min(m,n)) space
int lengthOfLongestSubstringBrute(string s) {
    int n = s.length();
    int maxLength = 0;
    
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            unordered_set<char> seen;
            bool allUnique = true;
            
            for (int k = i; k <= j; k++) {
                if (seen.count(s[k])) {
                    allUnique = false;
                    break;
                }
                seen.insert(s[k]);
            }
            
            if (allUnique) {
                maxLength = max(maxLength, j - i + 1);
            }
        }
    }
    
    return maxLength;
}

// Approach 2: Sliding Window with Set - O(2n) time, O(min(m,n)) space
int lengthOfLongestSubstringSet(string s) {
    unordered_set<char> seen;
    int left = 0, maxLength = 0;
    
    for (int right = 0; right < s.length(); right++) {
        while (seen.count(s[right])) {
            seen.erase(s[left]);
            left++;
        }
        seen.insert(s[right]);
        maxLength = max(maxLength, right - left + 1);
    }
    
    return maxLength;
}

// Approach 3: Optimized Sliding Window - O(n) time, O(min(m,n)) space
int lengthOfLongestSubstringOptimal(string s) {
    unordered_map<char, int> charIndex;
    int left = 0, maxLength = 0;
    
    for (int right = 0; right < s.length(); right++) {
        if (charIndex.count(s[right]) && charIndex[s[right]] >= left) {
            left = charIndex[s[right]] + 1;
        }
        charIndex[s[right]] = right;
        maxLength = max(maxLength, right - left + 1);
    }
    
    return maxLength;
}
```

**Analysis**:
- **Brute Force**: Check all substrings - very slow
- **Sliding Window**: Expand and contract window - good
- **Optimized**: Jump directly to next position - optimal!

### Problem 3: Minimum Window Substring

**Problem**: Find minimum window in string S that contains all characters of string T.

```cpp
// Approach 1: Brute Force - O(|S|² × |T|) time
string minWindowBrute(string s, string t) {
    if (s.empty() || t.empty()) return "";
    
    unordered_map<char, int> tCount;
    for (char c : t) tCount[c]++;
    
    int minLength = INT_MAX;
    string result = "";
    
    for (int i = 0; i < s.length(); i++) {
        for (int j = i; j < s.length(); j++) {
            unordered_map<char, int> windowCount;
            
            // Count characters in current window
            for (int k = i; k <= j; k++) {
                windowCount[s[k]]++;
            }
            
            // Check if window contains all characters of t
            bool valid = true;
            for (auto& p : tCount) {
                if (windowCount[p.first] < p.second) {
                    valid = false;
                    break;
                }
            }
            
            if (valid && j - i + 1 < minLength) {
                minLength = j - i + 1;
                result = s.substr(i, minLength);
            }
        }
    }
    
    return result;
}

// Approach 2: Sliding Window - O(|S| + |T|) time
string minWindowOptimal(string s, string t) {
    if (s.empty() || t.empty()) return "";
    
    unordered_map<char, int> tCount, windowCount;
    for (char c : t) tCount[c]++;
    
    int required = tCount.size();
    int formed = 0;
    int left = 0, right = 0;
    int minLength = INT_MAX;
    int minLeft = 0;
    
    while (right < s.length()) {
        // Expand window
        char c = s[right];
        windowCount[c]++;
        
        if (tCount.count(c) && windowCount[c] == tCount[c]) {
            formed++;
        }
        
        // Contract window
        while (left <= right && formed == required) {
            if (right - left + 1 < minLength) {
                minLength = right - left + 1;
                minLeft = left;
            }
            
            char leftChar = s[left];
            windowCount[leftChar]--;
            if (tCount.count(leftChar) && windowCount[leftChar] < tCount[leftChar]) {
                formed--;
            }
            left++;
        }
        
        right++;
    }
    
    return minLength == INT_MAX ? "" : s.substr(minLeft, minLength);
}
```

## 🎯 2D Array Techniques

### Matrix Traversal Patterns
```cpp
// Spiral traversal
vector<int> spiralOrder(vector<vector<int>>& matrix) {
    if (matrix.empty()) return {};
    
    vector<int> result;
    int top = 0, bottom = matrix.size() - 1;
    int left = 0, right = matrix[0].size() - 1;
    
    while (top <= bottom && left <= right) {
        // Traverse right
        for (int i = left; i <= right; i++) {
            result.push_back(matrix[top][i]);
        }
        top++;
        
        // Traverse down
        for (int i = top; i <= bottom; i++) {
            result.push_back(matrix[i][right]);
        }
        right--;
        
        // Traverse left
        if (top <= bottom) {
            for (int i = right; i >= left; i--) {
                result.push_back(matrix[bottom][i]);
            }
            bottom--;
        }
        
        // Traverse up
        if (left <= right) {
            for (int i = bottom; i >= top; i--) {
                result.push_back(matrix[i][left]);
            }
            left++;
        }
    }
    
    return result;
}

// Diagonal traversal
vector<int> diagonalTraversal(vector<vector<int>>& matrix) {
    if (matrix.empty()) return {};
    
    int m = matrix.size(), n = matrix[0].size();
    vector<int> result;
    
    // Upper diagonals
    for (int k = 0; k < n; k++) {
        int i = 0, j = k;
        while (i < m && j >= 0) {
            result.push_back(matrix[i][j]);
            i++;
            j--;
        }
    }
    
    // Lower diagonals
    for (int k = 1; k < m; k++) {
        int i = k, j = n - 1;
        while (i < m && j >= 0) {
            result.push_back(matrix[i][j]);
            i++;
            j--;
        }
    }
    
    return result;
}

// Rotate matrix 90 degrees clockwise
void rotate(vector<vector<int>>& matrix) {
    int n = matrix.size();
    
    // Transpose
    for (int i = 0; i < n; i++) {
        for (int j = i; j < n; j++) {
            swap(matrix[i][j], matrix[j][i]);
        }
    }
    
    // Reverse each row
    for (int i = 0; i < n; i++) {
        reverse(matrix[i].begin(), matrix[i].end());
    }
}
```

## 📚 Practice Problems Collection

### Easy Level
1. **Find Missing Number**: Array of n numbers containing 0 to n, find missing
2. **Move Zeros**: Move all zeros to end while maintaining relative order
3. **Valid Palindrome**: Check if string is palindrome ignoring non-alphanumeric
4. **Reverse String**: Reverse array of characters in-place

### Medium Level
1. **3Sum**: Find all unique triplets that sum to zero
2. **Product of Array Except Self**: Return array where each element is product of all others
3. **Longest Palindromic Substring**: Find the longest palindromic substring
4. **Search in Rotated Sorted Array**: Binary search in rotated array

### Hard Level
1. **Trapping Rain Water**: Calculate water trapped after raining
2. **Sliding Window Maximum**: Maximum element in each window of size k
3. **Edit Distance**: Minimum operations to convert one string to another
4. **Longest Valid Parentheses**: Length of longest valid parentheses substring

## ✅ Chapter Summary

### Key Techniques Mastered:
- **Two Pointers**: Efficient pair finding and array manipulation
- **Sliding Window**: Subarray and substring problems
- **Prefix Sum**: Range queries and subarray sums
- **String Algorithms**: Pattern matching and manipulation
- **Matrix Operations**: 2D array traversal and transformation

### Complexity Guidelines:
- Most array problems can be solved in O(n) or O(n log n)
- String problems often have O(n) solutions with proper techniques
- Space-time tradeoffs are common (hash maps vs. sorting)

### Problem-Solving Strategy:
1. Start with brute force to understand the problem
2. Look for patterns (sorted, pairs, subarrays)
3. Apply appropriate technique (two pointers, sliding window, etc.)
4. Optimize space complexity if needed

## 🎯 Next Chapter Preview

In [Chapter 5: Searching Algorithms](../05-searching/README.md), we'll explore efficient ways to find elements in data structures, from basic linear search to advanced techniques like binary search and its variations.

---

**Master's Tip**: "Arrays and strings are the foundation. Master these patterns, and 60% of coding interview problems become trivial!"

[← Previous Chapter](../03-complexity-analysis/README.md) | [Next Chapter: Searching →](../05-searching/README.md)
