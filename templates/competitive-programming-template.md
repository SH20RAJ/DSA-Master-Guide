# 🚀 Competitive Programming Template & Cheat Sheet

## 📝 Master Template

Save this as `template.cpp` for contests:

```cpp
#include <bits/stdc++.h>
using namespace std;

// ==================== TYPE DEFINITIONS ====================
#define ll long long
#define ull unsigned long long
#define ld long double
#define pii pair<int, int>
#define pll pair<long long, long long>
#define vi vector<int>
#define vll vector<long long>
#define vs vector<string>
#define vvi vector<vector<int>>
#define vvll vector<vector<long long>>
#define mii map<int, int>
#define mll map<long long, long long>
#define msi map<string, int>
#define umii unordered_map<int, int>
#define umll unordered_map<long long, long long>
#define umsi unordered_map<string, int>

// ==================== SHORTCUTS ====================
#define pb push_back
#define mp make_pair
#define fi first
#define se second
#define all(x) (x).begin(), (x).end()
#define rall(x) (x).rbegin(), (x).rend()
#define sz(x) (int)(x).size()
#define ins insert

// ==================== LOOPS ====================
#define FOR(i, a, b) for (int i = (a); i < (b); i++)
#define FORE(i, a, b) for (int i = (a); i <= (b); i++)
#define RFOR(i, a, b) for (int i = (a); i > (b); i--)
#define RFORE(i, a, b) for (int i = (a); i >= (b); i--)
#define EACH(x, a) for (auto& x : a)

// ==================== CONSTANTS ====================
const int MOD = 1e9 + 7;
const int MOD2 = 998244353;
const int INF = 1e9;
const ll LINF = 1e18;
const double EPS = 1e-9;
const double PI = acos(-1.0);

// ==================== DIRECTIONS ====================
const int dx[] = {-1, -1, -1, 0, 0, 1, 1, 1};
const int dy[] = {-1, 0, 1, -1, 1, -1, 0, 1};
const int dx4[] = {-1, 0, 1, 0};
const int dy4[] = {0, 1, 0, -1};

// ==================== UTILITY FUNCTIONS ====================
template<typename T>
void print_vector(const vector<T>& v, const string& sep = " ") {
    for (int i = 0; i < sz(v); i++) {
        cout << v[i];
        if (i != sz(v) - 1) cout << sep;
    }
    cout << "\n";
}

template<typename T>
void print_2d_vector(const vector<vector<T>>& v) {
    for (const auto& row : v) {
        print_vector(row);
    }
}

template<typename T>
T gcd(T a, T b) {
    return b ? gcd(b, a % b) : a;
}

template<typename T>
T lcm(T a, T b) {
    return a / gcd(a, b) * b;
}

ll power(ll a, ll b, ll mod = MOD) {
    ll result = 1;
    a %= mod;
    while (b > 0) {
        if (b & 1) result = (result * a) % mod;
        a = (a * a) % mod;
        b >>= 1;
    }
    return result;
}

ll modInverse(ll a, ll mod = MOD) {
    return power(a, mod - 2, mod);
}

bool isPrime(ll n) {
    if (n <= 1) return false;
    if (n <= 3) return true;
    if (n % 2 == 0 || n % 3 == 0) return false;
    for (ll i = 5; i * i <= n; i += 6) {
        if (n % i == 0 || n % (i + 2) == 0) return false;
    }
    return true;
}

vector<ll> sieve(int n) {
    vector<bool> is_prime(n + 1, true);
    is_prime[0] = is_prime[1] = false;
    for (int i = 2; i * i <= n; i++) {
        if (is_prime[i]) {
            for (int j = i * i; j <= n; j += i) {
                is_prime[j] = false;
            }
        }
    }
    vector<ll> primes;
    for (int i = 2; i <= n; i++) {
        if (is_prime[i]) primes.pb(i);
    }
    return primes;
}

// ==================== MAIN SOLUTION ====================
void solve() {
    // Your solution here
    
}

int main() {
    ios_base::sync_with_stdio(false);
    cin.tie(NULL);
    cout.tie(NULL);
    
    int t = 1;
    cin >> t;  // Comment this line if single test case
    
    while (t--) {
        solve();
    }
    
    return 0;
}
```

## 📊 Time Complexity Cheat Sheet

### Common Data Structures
| Data Structure | Access | Search | Insertion | Deletion | Space |
|---------------|--------|--------|-----------|----------|-------|
| Array | O(1) | O(n) | O(n) | O(n) | O(n) |
| Dynamic Array | O(1) | O(n) | O(1)* | O(n) | O(n) |
| Linked List | O(n) | O(n) | O(1) | O(1) | O(n) |
| Stack | O(n) | O(n) | O(1) | O(1) | O(n) |
| Queue | O(n) | O(n) | O(1) | O(1) | O(n) |
| Hash Table | N/A | O(1)* | O(1)* | O(1)* | O(n) |
| Binary Search Tree | O(log n)* | O(log n)* | O(log n)* | O(log n)* | O(n) |
| Red-Black Tree | O(log n) | O(log n) | O(log n) | O(log n) | O(n) |
| Heap | N/A | O(n) | O(log n) | O(log n) | O(n) |

*Average case

### Sorting Algorithms
| Algorithm | Best | Average | Worst | Space | Stable |
|-----------|------|---------|-------|-------|--------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | No |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Yes |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Yes |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | No |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | No |
| Counting Sort | O(n + k) | O(n + k) | O(n + k) | O(k) | Yes |
| Radix Sort | O(d(n + b)) | O(d(n + b)) | O(d(n + b)) | O(n + b) | Yes |

### Graph Algorithms
| Algorithm | Time | Space | Use Case |
|-----------|------|-------|----------|
| DFS | O(V + E) | O(V) | Path finding, cycle detection |
| BFS | O(V + E) | O(V) | Shortest path (unweighted) |
| Dijkstra | O((V + E) log V) | O(V) | Shortest path (weighted, non-negative) |
| Bellman-Ford | O(VE) | O(V) | Shortest path (negative weights) |
| Floyd-Warshall | O(V³) | O(V²) | All pairs shortest path |
| Kruskal's | O(E log E) | O(V) | Minimum spanning tree |
| Prim's | O((V + E) log V) | O(V) | Minimum spanning tree |

## 🎯 Common Problem Patterns

### 1. Two Pointers
```cpp
// Template for two pointers
int left = 0, right = n - 1;
while (left < right) {
    if (condition) {
        // Process and move pointers
        left++;
    } else {
        right--;
    }
}
```

### 2. Sliding Window
```cpp
// Template for sliding window
int left = 0, result = 0;
for (int right = 0; right < n; right++) {
    // Expand window
    // Update state
    
    while (window_condition_violated) {
        // Shrink window
        left++;
    }
    
    // Update result
    result = max(result, right - left + 1);
}
```

### 3. Binary Search
```cpp
// Template for binary search
int left = 0, right = n - 1;
while (left <= right) {
    int mid = left + (right - left) / 2;
    if (check(mid)) {
        result = mid;
        right = mid - 1;  // or left = mid + 1
    } else {
        left = mid + 1;   // or right = mid - 1
    }
}
```

### 4. DFS Template
```cpp
void dfs(int node, vector<vector<int>>& graph, vector<bool>& visited) {
    visited[node] = true;
    
    for (int neighbor : graph[node]) {
        if (!visited[neighbor]) {
            dfs(neighbor, graph, visited);
        }
    }
}
```

### 5. BFS Template
```cpp
void bfs(int start, vector<vector<int>>& graph) {
    queue<int> q;
    vector<bool> visited(graph.size(), false);
    
    q.push(start);
    visited[start] = true;
    
    while (!q.empty()) {
        int node = q.front();
        q.pop();
        
        for (int neighbor : graph[node]) {
            if (!visited[neighbor]) {
                visited[neighbor] = true;
                q.push(neighbor);
            }
        }
    }
}
```

### 6. Dynamic Programming
```cpp
// Top-down (Memoization)
int memo[MAXN];
int dp(int state) {
    if (base_case) return base_value;
    if (memo[state] != -1) return memo[state];
    
    // Compute result
    int result = 0;
    for (transition) {
        result = max(result, dp(new_state) + cost);
    }
    
    return memo[state] = result;
}

// Bottom-up (Tabulation)
vector<int> dp(n + 1);
dp[0] = base_value;
for (int i = 1; i <= n; i++) {
    for (transition) {
        dp[i] = max(dp[i], dp[prev_state] + cost);
    }
}
```

## 🔧 Useful STL Functions

### Vector Operations
```cpp
vector<int> v = {3, 1, 4, 1, 5};

// Sorting
sort(v.begin(), v.end());                    // Ascending
sort(v.begin(), v.end(), greater<int>());    // Descending

// Searching
bool found = binary_search(v.begin(), v.end(), 4);
auto it = lower_bound(v.begin(), v.end(), 3);  // First >= 3
auto it2 = upper_bound(v.begin(), v.end(), 3); // First > 3

// Min/Max
int min_val = *min_element(v.begin(), v.end());
int max_val = *max_element(v.begin(), v.end());

// Unique elements (array must be sorted first)
sort(v.begin(), v.end());
v.erase(unique(v.begin(), v.end()), v.end());

// Reverse
reverse(v.begin(), v.end());

// Sum
int sum = accumulate(v.begin(), v.end(), 0);

// Count
int count = count(v.begin(), v.end(), 1);

// Permutations
do {
    // Process permutation
} while (next_permutation(v.begin(), v.end()));
```

### String Operations
```cpp
string s = "hello world";

// Case conversion
transform(s.begin(), s.end(), s.begin(), ::tolower);
transform(s.begin(), s.end(), s.begin(), ::toupper);

// Substring
string sub = s.substr(6, 5);  // "world"

// Find
size_t pos = s.find("world");  // Returns position or string::npos

// Replace
s.replace(6, 5, "C++");  // "hello C++"

// Erase
s.erase(5, 6);  // "hello"

// Split string
vector<string> split(const string& s, char delimiter) {
    vector<string> tokens;
    stringstream ss(s);
    string token;
    while (getline(ss, token, delimiter)) {
        tokens.push_back(token);
    }
    return tokens;
}
```

## 🎯 Contest Strategy

### Time Management
- **Read all problems first** (5-10 minutes)
- **Solve easiest problems first**
- **Implement brute force if stuck** (partial points better than zero)
- **Leave 15 minutes for debugging**

### Problem Selection Priority
1. **Easy implementation** + **Familiar pattern**
2. **High point value** + **Confident solution**
3. **Partial scoring** if available
4. **Learning opportunity** if time permits

### Debugging Tips
```cpp
// Debug macros
#ifdef LOCAL
#define debug(x) cerr << #x << " = " << x << endl
#define debug2(x, y) cerr << #x << " = " << x << ", " << #y << " = " << y << endl
#else
#define debug(x)
#define debug2(x, y)
#endif

// Usage
debug(variable_name);
debug2(a, b);
```

### Input/Output Optimization
```cpp
// Fast I/O
ios_base::sync_with_stdio(false);
cin.tie(NULL);
cout.tie(NULL);

// For very large outputs, use '\n' instead of endl
cout << result << '\n';  // Faster than endl

// Reading large inputs
const int MAXN = 1e6;
int arr[MAXN];
int n;
cin >> n;
for (int i = 0; i < n; i++) {
    cin >> arr[i];
}
```

## 📝 Common Mistakes to Avoid

### 1. Integer Overflow
```cpp
// Wrong
int result = a * b;

// Correct
long long result = (long long)a * b;

// For modular arithmetic
long long result = ((long long)a * b) % MOD;
```

### 2. Array Bounds
```cpp
// Wrong
if (i + 1 < n && arr[i + 1] > arr[i])

// Correct
if (i < n - 1 && arr[i + 1] > arr[i])
```

### 3. Comparison with Floating Point
```cpp
// Wrong
if (a == b)

// Correct
if (abs(a - b) < EPS)
```

### 4. Uninitialized Variables
```cpp
// Wrong
int dp[1001];  // Contains garbage values

// Correct
int dp[1001] = {0};  // All elements initialized to 0
// or
memset(dp, 0, sizeof(dp));
// or
fill(dp, dp + 1001, 0);
```

## 🚀 Advanced Techniques

### Coordinate Compression
```cpp
vector<int> compress(vector<int>& arr) {
    vector<int> sorted_arr = arr;
    sort(sorted_arr.begin(), sorted_arr.end());
    sorted_arr.erase(unique(sorted_arr.begin(), sorted_arr.end()), sorted_arr.end());
    
    for (int& x : arr) {
        x = lower_bound(sorted_arr.begin(), sorted_arr.end(), x) - sorted_arr.begin();
    }
    
    return sorted_arr;  // Original values
}
```

### Binary Search on Answer
```cpp
bool check(int mid) {
    // Return true if mid is achievable
}

int binary_search_answer(int left, int right) {
    int result = -1;
    while (left <= right) {
        int mid = left + (right - left) / 2;
        if (check(mid)) {
            result = mid;
            left = mid + 1;  // Try for better answer
        } else {
            right = mid - 1;
        }
    }
    return result;
}
```

### Prefix Sums
```cpp
// 1D Prefix Sum
vector<long long> prefix(n + 1, 0);
for (int i = 0; i < n; i++) {
    prefix[i + 1] = prefix[i] + arr[i];
}
// Sum from l to r (inclusive): prefix[r + 1] - prefix[l]

// 2D Prefix Sum
vector<vector<long long>> prefix(m + 1, vector<long long>(n + 1, 0));
for (int i = 1; i <= m; i++) {
    for (int j = 1; j <= n; j++) {
        prefix[i][j] = matrix[i-1][j-1] + prefix[i-1][j] + 
                      prefix[i][j-1] - prefix[i-1][j-1];
    }
}
// Sum in rectangle (r1,c1) to (r2,c2):
// prefix[r2+1][c2+1] - prefix[r1][c2+1] - prefix[r2+1][c1] + prefix[r1][c1]
```

---

**Pro Tip**: "Practice with this template until muscle memory kicks in. In contests, you want to focus on problem-solving, not syntax!"

---

[← Back to Main Book](../README.md) | [Problem Collection →](../sample-problems/two-sum-variations.md)
