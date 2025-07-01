# Chapter 6: Sorting Algorithms 🔄

Master the art of arranging data in perfect order!

## 🎯 Learning Objectives
- Understand all major sorting algorithms
- Analyze time and space complexities
- Choose the right sorting algorithm for each scenario
- Master advanced sorting techniques and optimizations

## 📊 Why Sorting Matters

Sorting is fundamental to computer science because:
- **Search Efficiency**: Binary search requires sorted data
- **Data Organization**: Makes data easier to understand and process
- **Algorithm Preprocessing**: Many algorithms require sorted input
- **Database Operations**: Crucial for joins, indexing, and queries
- **Problem Solving**: Many problems become easier with sorted data

## 🎯 Sorting Algorithm Classification

### By Comparison:
- **Comparison-based**: Compare elements to determine order
- **Non-comparison**: Use element properties (counting, radix)

### By Stability:
- **Stable**: Maintains relative order of equal elements
- **Unstable**: May change relative order of equal elements

### By Memory:
- **In-place**: O(1) extra space
- **Out-of-place**: Requires additional space

### By Adaptability:
- **Adaptive**: Performs better on partially sorted data
- **Non-adaptive**: Same performance regardless of input order

## 🐌 Simple Sorting Algorithms

### 1. Bubble Sort
Repeatedly swap adjacent elements if they're in wrong order.

```cpp
// Basic Bubble Sort - O(n²) time, O(1) space
void bubbleSort(vector<int>& arr) {
    int n = arr.size();
    
    for (int i = 0; i < n - 1; i++) {
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
            }
        }
    }
}

// Optimized Bubble Sort (early termination)
void bubbleSortOptimized(vector<int>& arr) {
    int n = arr.size();
    bool swapped;
    
    for (int i = 0; i < n - 1; i++) {
        swapped = false;
        
        for (int j = 0; j < n - i - 1; j++) {
            if (arr[j] > arr[j + 1]) {
                swap(arr[j], arr[j + 1]);
                swapped = true;
            }
        }
        
        // If no swapping occurred, array is sorted
        if (!swapped) break;
    }
}

// Recursive Bubble Sort
void bubbleSortRecursive(vector<int>& arr, int n) {
    if (n == 1) return;
    
    // One pass of bubble sort
    for (int i = 0; i < n - 1; i++) {
        if (arr[i] > arr[i + 1]) {
            swap(arr[i], arr[i + 1]);
        }
    }
    
    // Recursively sort remaining elements
    bubbleSortRecursive(arr, n - 1);
}
```

### 2. Selection Sort
Find minimum element and place it at the beginning.

```cpp
// Selection Sort - O(n²) time, O(1) space
void selectionSort(vector<int>& arr) {
    int n = arr.size();
    
    for (int i = 0; i < n - 1; i++) {
        int minIndex = i;
        
        // Find minimum element in remaining array
        for (int j = i + 1; j < n; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
        }
        
        // Swap minimum element with first element
        if (minIndex != i) {
            swap(arr[i], arr[minIndex]);
        }
    }
}

// Selection Sort with finding both min and max
void selectionSortOptimized(vector<int>& arr) {
    int n = arr.size();
    
    for (int i = 0; i < n / 2; i++) {
        int minIndex = i, maxIndex = i;
        
        // Find both min and max in remaining array
        for (int j = i + 1; j < n - i; j++) {
            if (arr[j] < arr[minIndex]) {
                minIndex = j;
            }
            if (arr[j] > arr[maxIndex]) {
                maxIndex = j;
            }
        }
        
        // Place min at beginning
        swap(arr[i], arr[minIndex]);
        
        // If max was at position i, it's now at minIndex
        if (maxIndex == i) {
            maxIndex = minIndex;
        }
        
        // Place max at end
        swap(arr[n - 1 - i], arr[maxIndex]);
    }
}
```

### 3. Insertion Sort
Build sorted array one element at a time.

```cpp
// Insertion Sort - O(n²) worst, O(n) best, O(1) space
void insertionSort(vector<int>& arr) {
    int n = arr.size();
    
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        int j = i - 1;
        
        // Move elements greater than key one position ahead
        while (j >= 0 && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        
        arr[j + 1] = key;
    }
}

// Binary Insertion Sort (reduces comparisons)
void binaryInsertionSort(vector<int>& arr) {
    int n = arr.size();
    
    for (int i = 1; i < n; i++) {
        int key = arr[i];
        
        // Find insertion position using binary search
        int left = 0, right = i;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (arr[mid] > key) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        
        // Shift elements to make space
        for (int j = i; j > left; j--) {
            arr[j] = arr[j - 1];
        }
        
        arr[left] = key;
    }
}

// Recursive Insertion Sort
void insertionSortRecursive(vector<int>& arr, int n) {
    if (n <= 1) return;
    
    // Sort first n-1 elements
    insertionSortRecursive(arr, n - 1);
    
    // Insert last element at correct position
    int last = arr[n - 1];
    int j = n - 2;
    
    while (j >= 0 && arr[j] > last) {
        arr[j + 1] = arr[j];
        j--;
    }
    
    arr[j + 1] = last;
}
```

## 🚀 Efficient Sorting Algorithms

### 1. Merge Sort
Divide array into halves, sort them, then merge.

```cpp
// Merge function for merge sort
void merge(vector<int>& arr, int left, int mid, int right) {
    int n1 = mid - left + 1;
    int n2 = right - mid;
    
    // Create temporary arrays
    vector<int> leftArr(n1), rightArr(n2);
    
    // Copy data to temporary arrays
    for (int i = 0; i < n1; i++) {
        leftArr[i] = arr[left + i];
    }
    for (int j = 0; j < n2; j++) {
        rightArr[j] = arr[mid + 1 + j];
    }
    
    // Merge the temporary arrays back
    int i = 0, j = 0, k = left;
    
    while (i < n1 && j < n2) {
        if (leftArr[i] <= rightArr[j]) {
            arr[k] = leftArr[i];
            i++;
        } else {
            arr[k] = rightArr[j];
            j++;
        }
        k++;
    }
    
    // Copy remaining elements
    while (i < n1) {
        arr[k] = leftArr[i];
        i++;
        k++;
    }
    
    while (j < n2) {
        arr[k] = rightArr[j];
        j++;
        k++;
    }
}

// Merge Sort - O(n log n) time, O(n) space
void mergeSort(vector<int>& arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        
        mergeSort(arr, left, mid);      // Sort left half
        mergeSort(arr, mid + 1, right); // Sort right half
        merge(arr, left, mid, right);   // Merge sorted halves
    }
}

// Bottom-up Merge Sort (iterative)
void mergeSortIterative(vector<int>& arr) {
    int n = arr.size();
    
    // Start with subarrays of size 1, double size each iteration
    for (int currSize = 1; currSize < n; currSize *= 2) {
        // Pick starting point of left subarray
        for (int leftStart = 0; leftStart < n - 1; leftStart += 2 * currSize) {
            int mid = min(leftStart + currSize - 1, n - 1);
            int rightEnd = min(leftStart + 2 * currSize - 1, n - 1);
            
            if (mid < rightEnd) {
                merge(arr, leftStart, mid, rightEnd);
            }
        }
    }
}

// In-place Merge Sort (advanced technique)
void mergeSortInPlace(vector<int>& arr, int left, int right) {
    if (left < right) {
        int mid = left + (right - left) / 2;
        
        mergeSortInPlace(arr, left, mid);
        mergeSortInPlace(arr, mid + 1, right);
        
        // In-place merge (complex implementation)
        inplaceMerge(arr, left, mid, right);
    }
}
```

### 2. Quick Sort
Pick a pivot and partition array around it.

```cpp
// Partition function for quick sort
int partition(vector<int>& arr, int low, int high) {
    int pivot = arr[high];  // Choose last element as pivot
    int i = low - 1;        // Index of smaller element
    
    for (int j = low; j < high; j++) {
        if (arr[j] < pivot) {
            i++;
            swap(arr[i], arr[j]);
        }
    }
    
    swap(arr[i + 1], arr[high]);
    return i + 1;
}

// Quick Sort - O(n log n) average, O(n²) worst, O(log n) space
void quickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = partition(arr, low, high);
        
        quickSort(arr, low, pi - 1);   // Sort elements before partition
        quickSort(arr, pi + 1, high);  // Sort elements after partition
    }
}

// Randomized Quick Sort (better average case)
int randomizedPartition(vector<int>& arr, int low, int high) {
    int randomIndex = low + rand() % (high - low + 1);
    swap(arr[randomIndex], arr[high]);
    return partition(arr, low, high);
}

void randomizedQuickSort(vector<int>& arr, int low, int high) {
    if (low < high) {
        int pi = randomizedPartition(arr, low, high);
        
        randomizedQuickSort(arr, low, pi - 1);
        randomizedQuickSort(arr, pi + 1, high);
    }
}

// 3-Way Quick Sort (handles duplicates efficiently)
void quickSort3Way(vector<int>& arr, int low, int high) {
    if (low >= high) return;
    
    int lt = low, gt = high;
    int pivot = arr[low];
    int i = low + 1;
    
    while (i <= gt) {
        if (arr[i] < pivot) {
            swap(arr[lt], arr[i]);
            lt++;
            i++;
        } else if (arr[i] > pivot) {
            swap(arr[i], arr[gt]);
            gt--;
        } else {
            i++;
        }
    }
    
    quickSort3Way(arr, low, lt - 1);
    quickSort3Way(arr, gt + 1, high);
}

// Iterative Quick Sort (avoids recursion stack overflow)
void quickSortIterative(vector<int>& arr) {
    stack<pair<int, int>> st;
    st.push({0, arr.size() - 1});
    
    while (!st.empty()) {
        auto [low, high] = st.top();
        st.pop();
        
        if (low < high) {
            int pi = partition(arr, low, high);
            st.push({low, pi - 1});
            st.push({pi + 1, high});
        }
    }
}
```

### 3. Heap Sort
Use heap data structure to sort elements.

```cpp
// Heapify a subtree rooted at index i
void heapify(vector<int>& arr, int n, int i) {
    int largest = i;       // Initialize largest as root
    int left = 2 * i + 1;  // Left child
    int right = 2 * i + 2; // Right child
    
    // If left child is larger than root
    if (left < n && arr[left] > arr[largest]) {
        largest = left;
    }
    
    // If right child is larger than largest so far
    if (right < n && arr[right] > arr[largest]) {
        largest = right;
    }
    
    // If largest is not root
    if (largest != i) {
        swap(arr[i], arr[largest]);
        heapify(arr, n, largest);  // Recursively heapify affected subtree
    }
}

// Heap Sort - O(n log n) time, O(1) space
void heapSort(vector<int>& arr) {
    int n = arr.size();
    
    // Build heap (rearrange array)
    for (int i = n / 2 - 1; i >= 0; i--) {
        heapify(arr, n, i);
    }
    
    // Extract elements from heap one by one
    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);  // Move current root to end
        heapify(arr, i, 0);    // Call heapify on reduced heap
    }
}

// Min Heap Sort (ascending order)
void minHeapify(vector<int>& arr, int n, int i) {
    int smallest = i;
    int left = 2 * i + 1;
    int right = 2 * i + 2;
    
    if (left < n && arr[left] < arr[smallest]) {
        smallest = left;
    }
    
    if (right < n && arr[right] < arr[smallest]) {
        smallest = right;
    }
    
    if (smallest != i) {
        swap(arr[i], arr[smallest]);
        minHeapify(arr, n, smallest);
    }
}

void heapSortDescending(vector<int>& arr) {
    int n = arr.size();
    
    // Build min heap
    for (int i = n / 2 - 1; i >= 0; i--) {
        minHeapify(arr, n, i);
    }
    
    // Extract elements from heap
    for (int i = n - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        minHeapify(arr, i, 0);
    }
}
```

## ⚡ Advanced Sorting Algorithms

### 1. Counting Sort
Sort elements by counting occurrences.

```cpp
// Counting Sort - O(n + k) time, O(k) space, where k is range
void countingSort(vector<int>& arr) {
    if (arr.empty()) return;
    
    int maxElement = *max_element(arr.begin(), arr.end());
    int minElement = *min_element(arr.begin(), arr.end());
    int range = maxElement - minElement + 1;
    
    vector<int> count(range, 0);
    vector<int> output(arr.size());
    
    // Count occurrences of each element
    for (int num : arr) {
        count[num - minElement]++;
    }
    
    // Calculate cumulative count
    for (int i = 1; i < range; i++) {
        count[i] += count[i - 1];
    }
    
    // Build output array
    for (int i = arr.size() - 1; i >= 0; i--) {
        output[count[arr[i] - minElement] - 1] = arr[i];
        count[arr[i] - minElement]--;
    }
    
    // Copy output array to original array
    for (int i = 0; i < arr.size(); i++) {
        arr[i] = output[i];
    }
}

// Counting Sort for specific range [0, k]
void countingSortRange(vector<int>& arr, int k) {
    vector<int> count(k + 1, 0);
    
    // Count occurrences
    for (int num : arr) {
        count[num]++;
    }
    
    // Reconstruct array
    int index = 0;
    for (int i = 0; i <= k; i++) {
        while (count[i]-- > 0) {
            arr[index++] = i;
        }
    }
}
```

### 2. Radix Sort
Sort by processing digits from least to most significant.

```cpp
// Get maximum value to know number of digits
int getMax(vector<int>& arr) {
    return *max_element(arr.begin(), arr.end());
}

// Counting sort for a specific digit place
void countingSortForRadix(vector<int>& arr, int exp) {
    vector<int> output(arr.size());
    vector<int> count(10, 0);
    
    // Count occurrences of each digit
    for (int num : arr) {
        count[(num / exp) % 10]++;
    }
    
    // Calculate cumulative count
    for (int i = 1; i < 10; i++) {
        count[i] += count[i - 1];
    }
    
    // Build output array
    for (int i = arr.size() - 1; i >= 0; i--) {
        output[count[(arr[i] / exp) % 10] - 1] = arr[i];
        count[(arr[i] / exp) % 10]--;
    }
    
    // Copy output to original array
    for (int i = 0; i < arr.size(); i++) {
        arr[i] = output[i];
    }
}

// Radix Sort - O(d * (n + b)) time, where d is digits, b is base
void radixSort(vector<int>& arr) {
    int maxVal = getMax(arr);
    
    // Do counting sort for every digit
    for (int exp = 1; maxVal / exp > 0; exp *= 10) {
        countingSortForRadix(arr, exp);
    }
}

// Radix Sort for strings
void radixSortStrings(vector<string>& arr) {
    if (arr.empty()) return;
    
    int maxLength = 0;
    for (const string& s : arr) {
        maxLength = max(maxLength, (int)s.length());
    }
    
    // Sort by each character position from right to left
    for (int pos = maxLength - 1; pos >= 0; pos--) {
        vector<vector<string>> buckets(256);  // ASCII characters
        
        for (const string& s : arr) {
            char ch = (pos < s.length()) ? s[pos] : 0;  // Use null for shorter strings
            buckets[ch].push_back(s);
        }
        
        // Collect strings from buckets
        int index = 0;
        for (auto& bucket : buckets) {
            for (const string& s : bucket) {
                arr[index++] = s;
            }
        }
    }
}
```

### 3. Bucket Sort
Divide elements into buckets and sort individually.

```cpp
// Bucket Sort - O(n + k) average time, O(n * k) worst case
void bucketSort(vector<float>& arr) {
    if (arr.empty()) return;
    
    int n = arr.size();
    vector<vector<float>> buckets(n);
    
    // Put array elements in different buckets
    for (float num : arr) {
        int bucketIndex = n * num;  // Assumes input is in [0, 1)
        buckets[bucketIndex].push_back(num);
    }
    
    // Sort individual buckets
    for (auto& bucket : buckets) {
        sort(bucket.begin(), bucket.end());
    }
    
    // Concatenate all buckets
    int index = 0;
    for (const auto& bucket : buckets) {
        for (float num : bucket) {
            arr[index++] = num;
        }
    }
}

// Generic Bucket Sort for integers
void bucketSortInt(vector<int>& arr) {
    if (arr.empty()) return;
    
    int maxVal = *max_element(arr.begin(), arr.end());
    int minVal = *min_element(arr.begin(), arr.end());
    int range = maxVal - minVal + 1;
    int bucketCount = sqrt(arr.size());
    
    vector<vector<int>> buckets(bucketCount);
    
    // Distribute elements into buckets
    for (int num : arr) {
        int bucketIndex = (bucketCount * (num - minVal)) / range;
        if (bucketIndex == bucketCount) bucketIndex--;
        buckets[bucketIndex].push_back(num);
    }
    
    // Sort individual buckets and concatenate
    int index = 0;
    for (auto& bucket : buckets) {
        sort(bucket.begin(), bucket.end());
        for (int num : bucket) {
            arr[index++] = num;
        }
    }
}
```

## 🎯 Specialized Sorting Techniques

### 1. Shell Sort
Improved insertion sort using gap sequences.

```cpp
// Shell Sort - O(n^1.5) average time
void shellSort(vector<int>& arr) {
    int n = arr.size();
    
    // Start with large gap and reduce
    for (int gap = n / 2; gap > 0; gap /= 2) {
        // Perform gapped insertion sort
        for (int i = gap; i < n; i++) {
            int temp = arr[i];
            int j;
            
            for (j = i; j >= gap && arr[j - gap] > temp; j -= gap) {
                arr[j] = arr[j - gap];
            }
            
            arr[j] = temp;
        }
    }
}

// Shell Sort with Knuth sequence
void shellSortKnuth(vector<int>& arr) {
    int n = arr.size();
    
    // Generate Knuth sequence: 1, 4, 13, 40, 121, ...
    int gap = 1;
    while (gap < n / 3) {
        gap = 3 * gap + 1;
    }
    
    while (gap >= 1) {
        for (int i = gap; i < n; i++) {
            int temp = arr[i];
            int j = i;
            
            while (j >= gap && arr[j - gap] > temp) {
                arr[j] = arr[j - gap];
                j -= gap;
            }
            
            arr[j] = temp;
        }
        gap /= 3;
    }
}
```

### 2. Tim Sort (Hybrid Algorithm)
Combines merge sort and insertion sort (used in Python and Java).

```cpp
const int MIN_MERGE = 32;

// Get minimum run size for Tim Sort
int getMinRunSize(int n) {
    int r = 0;
    while (n >= MIN_MERGE) {
        r |= (n & 1);
        n >>= 1;
    }
    return n + r;
}

// Insertion sort for small arrays
void insertionSort(vector<int>& arr, int left, int right) {
    for (int i = left + 1; i <= right; i++) {
        int key = arr[i];
        int j = i - 1;
        
        while (j >= left && arr[j] > key) {
            arr[j + 1] = arr[j];
            j--;
        }
        
        arr[j + 1] = key;
    }
}

// Tim Sort (simplified version)
void timSort(vector<int>& arr) {
    int n = arr.size();
    int minRun = getMinRunSize(n);
    
    // Sort individual subarrays of size minRun using insertion sort
    for (int i = 0; i < n; i += minRun) {
        insertionSort(arr, i, min(i + minRun - 1, n - 1));
    }
    
    // Start merging from size minRun
    for (int size = minRun; size < n; size *= 2) {
        for (int left = 0; left < n; left += 2 * size) {
            int mid = left + size - 1;
            int right = min(left + 2 * size - 1, n - 1);
            
            if (mid < right) {
                merge(arr, left, mid, right);
            }
        }
    }
}
```

### 3. Intro Sort (Hybrid Algorithm)
Combines quicksort, heapsort, and insertion sort.

```cpp
// Intro Sort - O(n log n) worst case
void introSort(vector<int>& arr, int low, int high, int depthLimit) {
    int size = high - low + 1;
    
    // Use insertion sort for small arrays
    if (size <= 16) {
        insertionSort(arr, low, high);
        return;
    }
    
    // Use heap sort if recursion depth is too high
    if (depthLimit == 0) {
        heapSort(arr);  // Apply heap sort to entire array
        return;
    }
    
    // Use quicksort for normal cases
    int pivot = partition(arr, low, high);
    introSort(arr, low, pivot - 1, depthLimit - 1);
    introSort(arr, pivot + 1, high, depthLimit - 1);
}

void introSortMain(vector<int>& arr) {
    int depthLimit = 2 * log2(arr.size());
    introSort(arr, 0, arr.size() - 1, depthLimit);
}
```

## 🎯 Sorting Applications & Problems

### Problem 1: Sort Array by Parity

```cpp
// Approach 1: Two passes - O(n) time, O(n) space
vector<int> sortArrayByParityTwoPass(vector<int>& arr) {
    vector<int> result;
    
    // Add even numbers first
    for (int num : arr) {
        if (num % 2 == 0) {
            result.push_back(num);
        }
    }
    
    // Add odd numbers
    for (int num : arr) {
        if (num % 2 == 1) {
            result.push_back(num);
        }
    }
    
    return result;
}

// Approach 2: Two pointers - O(n) time, O(1) space
vector<int> sortArrayByParityTwoPointers(vector<int>& arr) {
    int left = 0, right = arr.size() - 1;
    
    while (left < right) {
        if (arr[left] % 2 == 1 && arr[right] % 2 == 0) {
            swap(arr[left], arr[right]);
        }
        
        if (arr[left] % 2 == 0) left++;
        if (arr[right] % 2 == 1) right--;
    }
    
    return arr;
}

// Approach 3: Partition approach - O(n) time, O(1) space
vector<int> sortArrayByParityPartition(vector<int>& arr) {
    int writeIndex = 0;
    
    // Move all even numbers to the front
    for (int i = 0; i < arr.size(); i++) {
        if (arr[i] % 2 == 0) {
            swap(arr[writeIndex], arr[i]);
            writeIndex++;
        }
    }
    
    return arr;
}
```

### Problem 2: Merge Intervals

```cpp
vector<vector<int>> mergeIntervals(vector<vector<int>>& intervals) {
    if (intervals.empty()) return {};
    
    // Sort intervals by start time
    sort(intervals.begin(), intervals.end());
    
    vector<vector<int>> merged;
    merged.push_back(intervals[0]);
    
    for (int i = 1; i < intervals.size(); i++) {
        // If current interval overlaps with the last merged interval
        if (intervals[i][0] <= merged.back()[1]) {
            merged.back()[1] = max(merged.back()[1], intervals[i][1]);
        } else {
            merged.push_back(intervals[i]);
        }
    }
    
    return merged;
}
```

### Problem 3: Custom Sort String

```cpp
// Sort string based on custom order
string customSortString(string order, string s) {
    vector<int> count(26, 0);
    
    // Count characters in s
    for (char c : s) {
        count[c - 'a']++;
    }
    
    string result;
    
    // Add characters in order
    for (char c : order) {
        while (count[c - 'a']-- > 0) {
            result += c;
        }
    }
    
    // Add remaining characters
    for (int i = 0; i < 26; i++) {
        while (count[i]-- > 0) {
            result += (char)('a' + i);
        }
    }
    
    return result;
}
```

## 📊 Sorting Algorithm Comparison

| Algorithm | Best | Average | Worst | Space | Stable | In-place | Adaptive |
|-----------|------|---------|-------|-------|---------|----------|----------|
| Bubble | O(n) | O(n²) | O(n²) | O(1) | Yes | Yes | Yes |
| Selection | O(n²) | O(n²) | O(n²) | O(1) | No | Yes | No |
| Insertion | O(n) | O(n²) | O(n²) | O(1) | Yes | Yes | Yes |
| Merge | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes | No | No |
| Quick | O(n log n) | O(n log n) | O(n²) | O(log n) | No | Yes | No |
| Heap | O(n log n) | O(n log n) | O(n log n) | O(1) | No | Yes | No |
| Counting | O(n + k) | O(n + k) | O(n + k) | O(k) | Yes | No | No |
| Radix | O(d(n + b)) | O(d(n + b)) | O(d(n + b)) | O(n + b) | Yes | No | No |
| Bucket | O(n + k) | O(n + k) | O(n²) | O(n) | Yes | No | No |

## 🎯 When to Use Which Algorithm

### Quick Sort:
- **Best for**: General purpose, average case performance
- **Avoid when**: Need guaranteed O(n log n), stability required

### Merge Sort:
- **Best for**: Guaranteed O(n log n), stable sort needed, external sorting
- **Avoid when**: Memory is limited

### Heap Sort:
- **Best for**: Guaranteed O(n log n) with O(1) space
- **Avoid when**: Stability is required

### Insertion Sort:
- **Best for**: Small arrays, nearly sorted arrays
- **Avoid when**: Large random arrays

### Counting Sort:
- **Best for**: Small range of integers
- **Avoid when**: Large range or non-integer data

### Radix Sort:
- **Best for**: Large arrays of integers or strings
- **Avoid when**: Variable-length data

## 🎯 Practice Problems

### Easy Level
1. **Sort Colors**: Sort array with only 0s, 1s, and 2s
2. **Sort Array by Parity**: Even numbers before odd numbers
3. **Largest Number**: Arrange numbers to form largest possible number
4. **Valid Anagram**: Check if two strings are anagrams using sorting

### Medium Level
1. **Merge Intervals**: Merge overlapping intervals
2. **Meeting Rooms II**: Minimum rooms needed for meetings
3. **Top K Frequent Elements**: Find k most frequent elements
4. **Sort Characters by Frequency**: Sort string by character frequency

### Hard Level
1. **Count of Range Sum**: Count subarrays with sum in given range
2. **Maximum Gap**: Maximum difference between successive elements in sorted array
3. **Count of Smaller Numbers After Self**: For each element, count smaller elements to the right
4. **Reverse Pairs**: Count pairs where arr[i] > 2 * arr[j] and i < j

## ✅ Chapter Summary

### Key Concepts Mastered:
- **Simple Sorts**: Bubble, Selection, Insertion - O(n²) but simple
- **Efficient Sorts**: Merge, Quick, Heap - O(n log n) general purpose
- **Non-comparison Sorts**: Counting, Radix, Bucket - can beat O(n log n)
- **Hybrid Algorithms**: Tim Sort, Intro Sort - best of multiple worlds
- **Stability and In-place**: Important properties for real applications

### Algorithm Selection Strategy:
1. **Small arrays (n < 50)**: Insertion sort
2. **General purpose**: Quick sort or Merge sort
3. **Guaranteed performance**: Merge sort or Heap sort
4. **Integer data with small range**: Counting sort
5. **Stability required**: Merge sort or stable variants
6. **Memory constrained**: Heap sort or in-place quick sort

### Master's Tips:
- **Understand the data**: Choose algorithm based on data characteristics
- **Consider constraints**: Time vs space vs stability requirements
- **Hybrid approaches**: Often better than pure algorithms
- **STL sort()**: Uses intro sort - excellent general choice

## 🎯 Next Chapter Preview

In [Chapter 7: Recursion & Backtracking](../07-recursion-backtracking/README.md), we'll master the art of solving problems by breaking them down into smaller subproblems, and learn powerful backtracking techniques for exploring solution spaces.

---

**Sorting Sage's Wisdom**: "A good sorting algorithm is like a good friend - reliable, efficient, and always puts things in the right order!"

[← Previous Chapter](../05-searching/README.md) | [Next Chapter: Recursion & Backtracking →](../07-recursion-backtracking/README.md)
