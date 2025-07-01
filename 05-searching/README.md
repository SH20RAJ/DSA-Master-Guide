# Chapter 5: Searching Algorithms 🔍

Master the art of finding elements efficiently in any data structure!

## 🎯 Learning Objectives
- Understand different searching paradigms
- Master binary search and its variations
- Learn advanced searching techniques
- Solve complex search problems with optimal approaches

## 🔍 Introduction to Searching

Searching is the process of finding a particular element in a collection of data. The efficiency of search algorithms can make the difference between a fast and slow application.

### Types of Searching:
1. **Linear/Sequential Search** - Check every element
2. **Binary Search** - Divide and conquer on sorted data
3. **Hash-based Search** - Direct access using hash functions
4. **Tree-based Search** - Hierarchical searching
5. **Graph-based Search** - BFS/DFS for complex relationships

## 📊 Linear Search

The simplest searching algorithm - check every element until found.

### Basic Implementation
```cpp
// Approach 1: Basic Linear Search - O(n) time, O(1) space
int linearSearch(vector<int>& arr, int target) {
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] == target) {
            return i;  // Return index
        }
    }
    return -1;  // Not found
}

// Find all occurrences
vector<int> linearSearchAll(vector<int>& arr, int target) {
    vector<int> indices;
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] == target) {
            indices.push_back(i);
        }
    }
    return indices;
}

// Linear search with custom comparator
template<typename T, typename Compare>
int linearSearchCustom(vector<T>& arr, T target, Compare comp) {
    for (int i = 0; i < arr.size(); i++) {
        if (comp(arr[i], target)) {
            return i;
        }
    }
    return -1;
}

// Usage example
vector<string> names = {"Alice", "Bob", "Charlie"};
int index = linearSearchCustom(names, string("Bob"), 
    [](const string& a, const string& b) { return a == b; });
```

### Optimized Linear Search
```cpp
// Sentinel Linear Search - Slightly optimized
int sentinelLinearSearch(vector<int>& arr, int target) {
    int n = arr.size();
    int last = arr[n - 1];
    arr[n - 1] = target;  // Place sentinel
    
    int i = 0;
    while (arr[i] != target) {
        i++;
    }
    
    arr[n - 1] = last;  // Restore original value
    
    if (i < n - 1 || arr[n - 1] == target) {
        return i;
    }
    return -1;
}

// Jump Search - O(√n) for sorted arrays
int jumpSearch(vector<int>& arr, int target) {
    int n = arr.size();
    int step = sqrt(n);
    int prev = 0;
    
    // Find block where element is present
    while (arr[min(step, n) - 1] < target) {
        prev = step;
        step += sqrt(n);
        if (prev >= n) return -1;
    }
    
    // Linear search in identified block
    while (arr[prev] < target) {
        prev++;
        if (prev == min(step, n)) return -1;
    }
    
    if (arr[prev] == target) return prev;
    return -1;
}
```

## 🎯 Binary Search Mastery

Binary search is the most important searching algorithm for sorted data.

### Standard Binary Search
```cpp
// Iterative Binary Search - O(log n) time, O(1) space
int binarySearch(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;  // Prevent overflow
        
        if (arr[mid] == target) {
            return mid;
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return -1;  // Not found
}

// Recursive Binary Search - O(log n) time, O(log n) space
int binarySearchRecursive(vector<int>& arr, int target, int left, int right) {
    if (left > right) return -1;
    
    int mid = left + (right - left) / 2;
    
    if (arr[mid] == target) return mid;
    else if (arr[mid] < target) 
        return binarySearchRecursive(arr, target, mid + 1, right);
    else 
        return binarySearchRecursive(arr, target, left, mid - 1);
}

// Using STL
bool binarySearchSTL(vector<int>& arr, int target) {
    return binary_search(arr.begin(), arr.end(), target);
}
```

### Binary Search Variations

#### 1. First and Last Occurrence
```cpp
// Find first occurrence of target
int findFirst(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            result = mid;
            right = mid - 1;  // Continue searching left
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}

// Find last occurrence of target
int findLast(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    int result = -1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) {
            result = mid;
            left = mid + 1;  // Continue searching right
        } else if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid - 1;
        }
    }
    
    return result;
}

// Count occurrences using first and last
int countOccurrences(vector<int>& arr, int target) {
    int first = findFirst(arr, target);
    if (first == -1) return 0;
    
    int last = findLast(arr, target);
    return last - first + 1;
}
```

#### 2. Lower and Upper Bound
```cpp
// Lower bound: first position where element can be inserted
int lowerBound(vector<int>& arr, int target) {
    int left = 0, right = arr.size();
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] < target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return left;
}

// Upper bound: first position after last occurrence
int upperBound(vector<int>& arr, int target) {
    int left = 0, right = arr.size();
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] <= target) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return left;
}

// Using STL
auto lb = lower_bound(arr.begin(), arr.end(), target);
auto ub = upper_bound(arr.begin(), arr.end(), target);
int count = ub - lb;  // Number of occurrences
```

#### 3. Peak Element
```cpp
// Find peak element (element greater than its neighbors)
int findPeakElement(vector<int>& arr) {
    int left = 0, right = arr.size() - 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] > arr[mid + 1]) {
            right = mid;  // Peak is on the left side
        } else {
            left = mid + 1;  // Peak is on the right side
        }
    }
    
    return left;
}

// Find peak in 2D matrix
pair<int, int> findPeakIn2D(vector<vector<int>>& matrix) {
    int rows = matrix.size(), cols = matrix[0].size();
    int left = 0, right = cols - 1;
    
    while (left <= right) {
        int midCol = left + (right - left) / 2;
        
        // Find maximum element in middle column
        int maxRow = 0;
        for (int i = 0; i < rows; i++) {
            if (matrix[i][midCol] > matrix[maxRow][midCol]) {
                maxRow = i;
            }
        }
        
        // Check if it's a peak
        bool isLeftSmaller = (midCol == 0 || 
            matrix[maxRow][midCol - 1] < matrix[maxRow][midCol]);
        bool isRightSmaller = (midCol == cols - 1 || 
            matrix[maxRow][midCol + 1] < matrix[maxRow][midCol]);
        
        if (isLeftSmaller && isRightSmaller) {
            return {maxRow, midCol};
        } else if (!isLeftSmaller) {
            right = midCol - 1;
        } else {
            left = midCol + 1;
        }
    }
    
    return {-1, -1};
}
```

## 🌊 Search in Special Arrays

### Rotated Sorted Array
```cpp
// Approach 1: Find rotation point, then binary search
int findRotationPoint(vector<int>& arr) {
    int left = 0, right = arr.size() - 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] > arr[right]) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    
    return left;
}

int searchInRotatedArray(vector<int>& arr, int target) {
    int rotationPoint = findRotationPoint(arr);
    int n = arr.size();
    
    // Decide which half to search
    if (target >= arr[rotationPoint] && target <= arr[n - 1]) {
        return binarySearch(arr, target, rotationPoint, n - 1);
    } else {
        return binarySearch(arr, target, 0, rotationPoint - 1);
    }
}

// Approach 2: Direct binary search in rotated array
int searchRotatedDirect(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) return mid;
        
        // Check which half is sorted
        if (arr[left] <= arr[mid]) {
            // Left half is sorted
            if (target >= arr[left] && target < arr[mid]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        } else {
            // Right half is sorted
            if (target > arr[mid] && target <= arr[right]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
    
    return -1;
}

// Handle duplicates in rotated array
int searchRotatedWithDuplicates(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target) return mid;
        
        // Handle duplicates at boundaries
        if (arr[left] == arr[mid] && arr[mid] == arr[right]) {
            left++;
            right--;
        } else if (arr[left] <= arr[mid]) {
            if (target >= arr[left] && target < arr[mid]) {
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        } else {
            if (target > arr[mid] && target <= arr[right]) {
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }
    }
    
    return -1;
}
```

### Search in 2D Matrix
```cpp
// Approach 1: Row-wise sorted, each row's first > previous row's last
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    if (matrix.empty() || matrix[0].empty()) return false;
    
    int rows = matrix.size(), cols = matrix[0].size();
    int left = 0, right = rows * cols - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        int midValue = matrix[mid / cols][mid % cols];
        
        if (midValue == target) return true;
        else if (midValue < target) left = mid + 1;
        else right = mid - 1;
    }
    
    return false;
}

// Approach 2: Each row and column sorted, start from top-right
bool searchMatrixII(vector<vector<int>>& matrix, int target) {
    if (matrix.empty() || matrix[0].empty()) return false;
    
    int row = 0, col = matrix[0].size() - 1;
    
    while (row < matrix.size() && col >= 0) {
        if (matrix[row][col] == target) return true;
        else if (matrix[row][col] > target) col--;
        else row++;
    }
    
    return false;
}

// Count elements smaller than target in sorted matrix
int countSmaller(vector<vector<int>>& matrix, int target) {
    int rows = matrix.size(), cols = matrix[0].size();
    int row = rows - 1, col = 0;
    int count = 0;
    
    while (row >= 0 && col < cols) {
        if (matrix[row][col] < target) {
            count += row + 1;  // All elements in this column up to row
            col++;
        } else {
            row--;
        }
    }
    
    return count;
}
```

## 🎯 Advanced Search Techniques

### Ternary Search
Used for finding maximum/minimum in unimodal functions.

```cpp
// Find maximum in unimodal array
double ternarySearch(double left, double right, function<double(double)> f) {
    const double EPS = 1e-9;
    
    while (right - left > EPS) {
        double m1 = left + (right - left) / 3;
        double m2 = right - (right - left) / 3;
        
        if (f(m1) < f(m2)) {
            left = m1;
        } else {
            right = m2;
        }
    }
    
    return (left + right) / 2;
}

// Find peak in mountain array
int findInMountainArray(int target, vector<int>& mountainArr) {
    // First find the peak
    int left = 0, right = mountainArr.size() - 1;
    while (left < right) {
        int mid = left + (right - left) / 2;
        if (mountainArr[mid] < mountainArr[mid + 1]) {
            left = mid + 1;
        } else {
            right = mid;
        }
    }
    int peak = left;
    
    // Search in ascending part
    int result = binarySearch(mountainArr, target, 0, peak);
    if (result != -1) return result;
    
    // Search in descending part
    left = peak + 1;
    right = mountainArr.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (mountainArr[mid] == target) return mid;
        else if (mountainArr[mid] > target) left = mid + 1;
        else right = mid - 1;
    }
    
    return -1;
}
```

### Exponential Search
Good for unbounded or infinite arrays.

```cpp
// Exponential search for sorted array
int exponentialSearch(vector<int>& arr, int target) {
    if (arr[0] == target) return 0;
    
    // Find range for binary search
    int i = 1;
    while (i < arr.size() && arr[i] <= target) {
        i *= 2;
    }
    
    // Binary search in found range
    return binarySearch(arr, target, i/2, min(i, (int)arr.size() - 1));
}

// Search in infinite sorted array
int searchInfinite(vector<int>& arr, int target) {
    int left = 0, right = 1;
    
    // Find upper bound
    while (arr[right] < target) {
        left = right;
        right *= 2;
    }
    
    // Binary search in [left, right]
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (arr[mid] == target) return mid;
        else if (arr[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    
    return -1;
}
```

### Interpolation Search
Better than binary search for uniformly distributed data.

```cpp
// Interpolation search - O(log log n) for uniform distribution
int interpolationSearch(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    
    while (left <= right && target >= arr[left] && target <= arr[right]) {
        if (left == right) {
            if (arr[left] == target) return left;
            return -1;
        }
        
        // Interpolation formula
        int pos = left + ((double)(target - arr[left]) / 
                         (arr[right] - arr[left])) * (right - left);
        
        if (arr[pos] == target) return pos;
        else if (arr[pos] < target) left = pos + 1;
        else right = pos - 1;
    }
    
    return -1;
}
```

## 🎯 Complex Search Problems

### Problem 1: Search Insert Position

```cpp
// Three approaches for finding insert position

// Approach 1: Linear scan - O(n)
int searchInsertLinear(vector<int>& nums, int target) {
    for (int i = 0; i < nums.size(); i++) {
        if (nums[i] >= target) return i;
    }
    return nums.size();
}

// Approach 2: Binary search - O(log n)
int searchInsertBinary(vector<int>& nums, int target) {
    int left = 0, right = nums.size() - 1;
    
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) return mid;
        else if (nums[mid] < target) left = mid + 1;
        else right = mid - 1;
    }
    
    return left;  // Insert position
}

// Approach 3: STL lower_bound - O(log n)
int searchInsertSTL(vector<int>& nums, int target) {
    return lower_bound(nums.begin(), nums.end(), target) - nums.begin();
}
```

### Problem 2: Find Minimum in Rotated Array

```cpp
// Approach 1: Linear scan - O(n)
int findMinLinear(vector<int>& nums) {
    int minVal = nums[0];
    for (int num : nums) {
        minVal = min(minVal, num);
    }
    return minVal;
}

// Approach 2: Binary search - O(log n)
int findMinBinary(vector<int>& nums) {
    int left = 0, right = nums.size() - 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] > nums[right]) {
            left = mid + 1;  // Minimum is in right half
        } else {
            right = mid;     // Minimum is in left half or mid
        }
    }
    
    return nums[left];
}

// With duplicates - O(log n) average, O(n) worst case
int findMinWithDuplicates(vector<int>& nums) {
    int left = 0, right = nums.size() - 1;
    
    while (left < right) {
        int mid = left + (right - left) / 2;
        
        if (nums[mid] > nums[right]) {
            left = mid + 1;
        } else if (nums[mid] < nums[right]) {
            right = mid;
        } else {
            right--;  // Can't determine, reduce search space
        }
    }
    
    return nums[left];
}
```

### Problem 3: Median of Two Sorted Arrays

```cpp
// Approach 1: Merge and find median - O(m + n)
double findMedianSortedArraysMerge(vector<int>& nums1, vector<int>& nums2) {
    vector<int> merged;
    int i = 0, j = 0;
    
    while (i < nums1.size() && j < nums2.size()) {
        if (nums1[i] <= nums2[j]) {
            merged.push_back(nums1[i++]);
        } else {
            merged.push_back(nums2[j++]);
        }
    }
    
    while (i < nums1.size()) merged.push_back(nums1[i++]);
    while (j < nums2.size()) merged.push_back(nums2[j++]);
    
    int n = merged.size();
    if (n % 2 == 1) {
        return merged[n / 2];
    } else {
        return (merged[n / 2 - 1] + merged[n / 2]) / 2.0;
    }
}

// Approach 2: Binary search - O(log(min(m, n)))
double findMedianSortedArraysBinary(vector<int>& nums1, vector<int>& nums2) {
    if (nums1.size() > nums2.size()) {
        return findMedianSortedArraysBinary(nums2, nums1);
    }
    
    int m = nums1.size(), n = nums2.size();
    int left = 0, right = m;
    
    while (left <= right) {
        int cut1 = (left + right) / 2;
        int cut2 = (m + n + 1) / 2 - cut1;
        
        int left1 = (cut1 == 0) ? INT_MIN : nums1[cut1 - 1];
        int left2 = (cut2 == 0) ? INT_MIN : nums2[cut2 - 1];
        
        int right1 = (cut1 == m) ? INT_MAX : nums1[cut1];
        int right2 = (cut2 == n) ? INT_MAX : nums2[cut2];
        
        if (left1 <= right2 && left2 <= right1) {
            if ((m + n) % 2 == 0) {
                return (max(left1, left2) + min(right1, right2)) / 2.0;
            } else {
                return max(left1, left2);
            }
        } else if (left1 > right2) {
            right = cut1 - 1;
        } else {
            left = cut1 + 1;
        }
    }
    
    return 1.0;
}
```

## 🎯 Search in Strings

### KMP String Searching (covered in detail in strings chapter)
```cpp
// Build failure function for KMP
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

// KMP search
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

## 📊 Search Algorithm Comparison

| Algorithm | Time Complexity | Space | Use Case |
|-----------|----------------|-------|----------|
| Linear Search | O(n) | O(1) | Unsorted data, small arrays |
| Binary Search | O(log n) | O(1) | Sorted arrays |
| Jump Search | O(√n) | O(1) | Large sorted arrays |
| Interpolation | O(log log n) avg | O(1) | Uniformly distributed |
| Exponential | O(log n) | O(1) | Unbounded arrays |
| Ternary Search | O(log₃ n) | O(1) | Unimodal functions |

## 🎯 Practice Problems

### Easy Level
1. **First Bad Version**: Find first bad version using binary search
2. **Sqrt(x)**: Implement integer square root using binary search
3. **Valid Perfect Square**: Check if number is perfect square
4. **Search Insert Position**: Find position to insert target

### Medium Level
1. **Find Peak Element**: Find any peak in array
2. **Search in Rotated Array**: Handle rotation in sorted array
3. **Find Minimum in Rotated Array**: Handle duplicates
4. **Kth Smallest in Sorted Matrix**: Use binary search on answer

### Hard Level
1. **Median of Two Sorted Arrays**: O(log(min(m,n))) solution
2. **Split Array Largest Sum**: Binary search on answer
3. **Aggressive Cows**: Classic binary search application
4. **Find K Closest Elements**: Combine binary search with sliding window

## ✅ Chapter Summary

### Key Concepts Mastered:
- **Linear Search**: Foundation of all searching
- **Binary Search**: Most important searching algorithm
- **Search Variations**: Handle duplicates, rotation, 2D arrays
- **Advanced Techniques**: Ternary, exponential, interpolation search
- **Problem Patterns**: Insert position, peak finding, rotated arrays

### Binary Search Template:
```cpp
int binarySearch(vector<int>& arr, int target) {
    int left = 0, right = arr.size() - 1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (condition(mid)) {
            // Update result and search left/right
        } else {
            // Search other half
        }
    }
    return result;
}
```

### When to Use Each Algorithm:
- **Linear**: Small data, unsorted, simple implementation needed
- **Binary**: Large sorted data, need O(log n) performance
- **Jump**: Large sorted data, cache-friendly access
- **Interpolation**: Uniformly distributed data
- **Exponential**: Unbounded data, don't know size

## 🎯 Next Chapter Preview

In [Chapter 6: Sorting Algorithms](../06-sorting/README.md), we'll master the art of arranging data in order, from simple bubble sort to advanced techniques like quicksort and mergesort, plus external sorting for big data.

---

**Search Master's Secret**: "Binary search isn't just about finding elements - it's about eliminating half the possibilities with each decision!"

[← Previous Chapter](../04-arrays-strings/README.md) | [Next Chapter: Sorting →](../06-sorting/README.md)
