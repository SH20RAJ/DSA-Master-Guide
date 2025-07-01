# Chapter 2: C++ Essentials for DSA 💻

Master the C++ features that will make you a DSA superhuman!

## 🎯 Learning Objectives
- Master STL containers and algorithms
- Learn competitive programming shortcuts
- Understand memory management
- Use advanced C++ features for DSA

## 🚀 The Power of STL

The Standard Template Library (STL) is your secret weapon in competitive programming.

### Essential Headers
```cpp
#include <bits/stdc++.h>  // Includes everything (competitive programming)
// OR include specific headers:
#include <iostream>
#include <vector>
#include <algorithm>
#include <map>
#include <set>
#include <queue>
#include <stack>
#include <string>
```

### Fast I/O Setup
```cpp
ios_base::sync_with_stdio(false);
cin.tie(NULL);
cout.tie(NULL);
```

## 📦 STL Containers Deep Dive

### 1. Vector - Dynamic Array
```cpp
#include <vector>
using namespace std;

// Declaration
vector<int> v;                    // Empty vector
vector<int> v(5);                 // Size 5, initialized to 0
vector<int> v(5, 10);             // Size 5, all elements = 10
vector<int> v = {1, 2, 3, 4, 5};  // Initialize with values

// Essential Operations
v.push_back(6);        // Add to end: O(1)
v.pop_back();          // Remove from end: O(1)
v.size();              // Get size: O(1)
v.empty();             // Check if empty: O(1)
v.clear();             // Remove all elements: O(n)
v.front();             // First element: O(1)
v.back();              // Last element: O(1)

// Insertion and deletion at any position
v.insert(v.begin() + 2, 99);     // Insert 99 at index 2
v.erase(v.begin() + 1);          // Remove element at index 1
v.erase(v.begin() + 1, v.begin() + 3);  // Remove range [1,3)

// Iteration
for (int i = 0; i < v.size(); i++) {
    cout << v[i] << " ";
}

// Range-based for loop (C++11)
for (int x : v) {
    cout << x << " ";
}

// Iterator-based
for (auto it = v.begin(); it != v.end(); it++) {
    cout << *it << " ";
}
```

### 2. String - Character Sequences
```cpp
#include <string>
using namespace std;

string s = "Hello";
string s2("World");
string s3(5, 'A');  // "AAAAA"

// Essential Operations
s.length();          // or s.size()
s.empty();
s.push_back('!');    // Add character
s.pop_back();        // Remove last character
s.substr(1, 3);      // Substring from index 1, length 3
s.find("llo");       // Find substring, returns position or string::npos
s.replace(1, 2, "i"); // Replace 2 chars starting at index 1 with "i"

// String to number conversion
int num = stoi("123");           // String to integer
long long ll_num = stoll("123"); // String to long long
double d = stod("3.14");         // String to double

// Number to string conversion
string str = to_string(123);
```

### 3. Pair - Two Related Values
```cpp
#include <utility>
using namespace std;

pair<int, int> p = {3, 5};
pair<int, string> p2 = make_pair(1, "hello");

// Access elements
cout << p.first << " " << p.second << endl;

// Comparison (lexicographic)
pair<int, int> a = {1, 5};
pair<int, int> b = {1, 3};
cout << (a > b) << endl;  // true, because 5 > 3

// Useful for coordinate problems
vector<pair<int, int>> points = {{1, 2}, {3, 4}, {0, 1}};
```

### 4. Set - Unique Elements in Sorted Order
```cpp
#include <set>
using namespace std;

set<int> s;
s.insert(5);         // O(log n)
s.insert(3);
s.insert(8);
s.insert(3);         // Duplicate, won't be added

s.erase(5);          // Remove element: O(log n)
s.count(3);          // Check if exists (returns 0 or 1): O(log n)
s.find(8);           // Returns iterator: O(log n)

// Iteration (always in sorted order)
for (int x : s) {
    cout << x << " ";  // Output: 3 8
}

// Multiset allows duplicates
multiset<int> ms;
ms.insert(3);
ms.insert(3);
ms.insert(5);
cout << ms.count(3) << endl;  // Output: 2
```

### 5. Map - Key-Value Pairs
```cpp
#include <map>
using namespace std;

map<string, int> m;
m["apple"] = 5;      // O(log n)
m["banana"] = 3;
m.insert({"orange", 7});

// Access
cout << m["apple"] << endl;      // 5
cout << m.count("banana") << endl; // 1 (exists)
cout << m.count("grape") << endl;  // 0 (doesn't exist)

// Iteration (sorted by key)
for (auto& p : m) {
    cout << p.first << ": " << p.second << endl;
}

// Unordered map for O(1) average operations
#include <unordered_map>
unordered_map<string, int> um;
um["key"] = value;   // O(1) average, O(n) worst case
```

### 6. Queue - FIFO Structure
```cpp
#include <queue>
using namespace std;

queue<int> q;
q.push(1);          // Add to back
q.push(2);
q.push(3);

cout << q.front() << endl;  // 1 (first element)
cout << q.back() << endl;   // 3 (last element)
q.pop();            // Remove from front
cout << q.size() << endl;   // 2

// Priority Queue (Max Heap by default)
priority_queue<int> pq;
pq.push(3);
pq.push(1);
pq.push(4);
cout << pq.top() << endl;   // 4 (maximum element)
pq.pop();           // Remove maximum

// Min Heap
priority_queue<int, vector<int>, greater<int>> min_pq;
min_pq.push(3);
min_pq.push(1);
min_pq.push(4);
cout << min_pq.top() << endl;  // 1 (minimum element)
```

### 7. Stack - LIFO Structure
```cpp
#include <stack>
using namespace std;

stack<int> st;
st.push(1);         // Add to top
st.push(2);
st.push(3);

cout << st.top() << endl;   // 3 (top element)
st.pop();           // Remove from top
cout << st.size() << endl;  // 2
```

## 🧮 STL Algorithms

### Sorting and Searching
```cpp
#include <algorithm>
using namespace std;

vector<int> v = {3, 1, 4, 1, 5, 9};

// Sorting
sort(v.begin(), v.end());              // Ascending: O(n log n)
sort(v.begin(), v.end(), greater<int>()); // Descending

// Custom comparator
sort(v.begin(), v.end(), [](int a, int b) {
    return a > b;  // Descending order
});

// Binary search (array must be sorted)
bool found = binary_search(v.begin(), v.end(), 4);  // O(log n)

// Lower bound and upper bound
auto it = lower_bound(v.begin(), v.end(), 4);  // First element >= 4
auto it2 = upper_bound(v.begin(), v.end(), 4); // First element > 4

// Min and max
int min_val = *min_element(v.begin(), v.end());
int max_val = *max_element(v.begin(), v.end());
auto minmax = minmax_element(v.begin(), v.end());
```

### Useful Algorithms
```cpp
// Reverse
reverse(v.begin(), v.end());

// Unique (remove consecutive duplicates, array should be sorted first)
sort(v.begin(), v.end());
v.erase(unique(v.begin(), v.end()), v.end());

// Count occurrences
int count = count(v.begin(), v.end(), 1);

// Sum of elements
int sum = accumulate(v.begin(), v.end(), 0);

// Check if all elements satisfy condition
bool all_positive = all_of(v.begin(), v.end(), [](int x) { return x > 0; });

// Permutations
do {
    // Process current permutation
} while (next_permutation(v.begin(), v.end()));
```

## 🏎️ Competitive Programming Macros

```cpp
#include <bits/stdc++.h>
using namespace std;

// Type definitions
#define ll long long
#define ull unsigned long long
#define ld long double
#define pii pair<int, int>
#define pll pair<long long, long long>
#define vi vector<int>
#define vll vector<long long>
#define vs vector<string>
#define vvi vector<vector<int>>

// Shortcuts
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define all(x) (x).begin(), (x).end()
#define rall(x) (x).rbegin(), (x).rend()
#define sz(x) (int)(x).size()

// Loops
#define FOR(i, a, b) for (int i = (a); i < (b); i++)
#define FORE(i, a, b) for (int i = (a); i <= (b); i++)
#define RFOR(i, a, b) for (int i = (a); i > (b); i--)
#define RFORE(i, a, b) for (int i = (a); i >= (b); i--)

// Constants
const int MOD = 1e9 + 7;
const int INF = 1e9;
const ll LINF = 1e18;
const double EPS = 1e-9;

// Utility functions
template<typename T>
void print_vector(vector<T>& v) {
    for (T x : v) cout << x << " ";
    cout << endl;
}

ll gcd(ll a, ll b) {
    return b ? gcd(b, a % b) : a;
}

ll lcm(ll a, ll b) {
    return a / gcd(a, b) * b;
}

ll power(ll a, ll b, ll mod = MOD) {
    ll result = 1;
    while (b > 0) {
        if (b & 1) result = (result * a) % mod;
        a = (a * a) % mod;
        b >>= 1;
    }
    return result;
}
```

## 💡 Advanced C++ Features for DSA

### Lambda Functions
```cpp
// Sorting with custom comparator
vector<pii> points = {{1, 3}, {2, 1}, {0, 4}};
sort(points.begin(), points.end(), [](pii a, pii b) {
    return a.second < b.second;  // Sort by second element
});

// Using lambda with STL algorithms
int count_even = count_if(v.begin(), v.end(), [](int x) {
    return x % 2 == 0;
});
```

### Auto Keyword
```cpp
auto it = v.begin();           // Iterator
auto p = make_pair(1, "hello"); // pair<int, string>
auto sum = [](int a, int b) { return a + b; }; // Lambda function
```

### Range-based For Loops
```cpp
vector<int> v = {1, 2, 3, 4, 5};

// Read-only
for (int x : v) {
    cout << x << " ";
}

// Modify elements
for (int& x : v) {
    x *= 2;
}

// Auto with references
for (auto& x : v) {
    x += 1;
}
```

## 🎯 Practice Problems

### Problem 1: Two Sum
**Given an array and a target, find two numbers that add up to the target.**

```cpp
// Approach 1: Brute Force - O(n²)
vector<int> twoSumBrute(vector<int>& nums, int target) {
    for (int i = 0; i < nums.size(); i++) {
        for (int j = i + 1; j < nums.size(); j++) {
            if (nums[i] + nums[j] == target) {
                return {i, j};
            }
        }
    }
    return {};
}

// Approach 2: Hash Map - O(n)
vector<int> twoSumOptimal(vector<int>& nums, int target) {
    unordered_map<int, int> mp;
    for (int i = 0; i < nums.size(); i++) {
        int complement = target - nums[i];
        if (mp.count(complement)) {
            return {mp[complement], i};
        }
        mp[nums[i]] = i;
    }
    return {};
}
```

### Problem 2: Frequency Counter
**Count frequency of each character in a string.**

```cpp
// Using map
map<char, int> countFrequency(string s) {
    map<char, int> freq;
    for (char c : s) {
        freq[c]++;
    }
    return freq;
}

// Using array (for lowercase letters only)
vector<int> countFrequencyArray(string s) {
    vector<int> freq(26, 0);
    for (char c : s) {
        freq[c - 'a']++;
    }
    return freq;
}
```

## ✅ Chapter Summary

- STL is your best friend in competitive programming
- Master vectors, strings, sets, maps, queues, and stacks
- Use STL algorithms for common operations
- Macros can save time in contests
- Lambda functions provide flexible custom logic

## 🎯 Quick Reference Cheat Sheet

| Container | Use Case | Access | Insert | Delete | Search |
|-----------|----------|---------|---------|---------|---------|
| vector | Dynamic array | O(1) | O(1) amortized | O(n) | O(n) |
| set | Unique sorted elements | - | O(log n) | O(log n) | O(log n) |
| map | Key-value pairs | O(log n) | O(log n) | O(log n) | O(log n) |
| queue | FIFO operations | O(1) | O(1) | O(1) | - |
| stack | LIFO operations | O(1) | O(1) | O(1) | - |

## 🎯 Next Chapter Preview

In [Chapter 3: Time & Space Complexity](../03-complexity-analysis/README.md), we'll master the art of analyzing algorithms and learn to choose the most efficient solutions.

---

**Pro Tip**: "The difference between a good programmer and a great programmer is knowing when to use which data structure!"

[← Previous Chapter](../01-introduction/README.md) | [Next Chapter: Complexity Analysis →](../03-complexity-analysis/README.md)
