# Chapter 10: Binary Trees - The Hierarchical Powerhouses

## Table of Contents
- [Introduction to Trees](#introduction)
- [Binary Tree Fundamentals](#binary-tree-fundamentals)
- [Tree Traversals](#tree-traversals)
- [Binary Search Trees](#binary-search-trees)
- [Tree Construction Problems](#tree-construction)
- [Advanced Tree Algorithms](#advanced-algorithms)
- [Classic Problems with Three Approaches](#classic-problems)
- [Balanced Trees](#balanced-trees)
- [Special Tree Types](#special-trees)
- [Practice Problems](#practice-problems)

## Introduction to Trees {#introduction}

Trees are hierarchical data structures consisting of nodes connected by edges. They represent relationships with a parent-child structure and are fundamental to many algorithms and applications.

### Why Trees Matter in DSA
- **Hierarchical Data**: File systems, organizational charts, decision trees
- **Search Efficiency**: Binary search trees offer O(log n) operations
- **Expression Parsing**: Syntax trees for compilers and calculators
- **Network Routing**: Spanning trees for network protocols
- **Database Indexing**: B-trees and B+ trees

### Tree Terminology
```cpp
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode() : val(0), left(nullptr), right(nullptr) {}
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
    TreeNode(int x, TreeNode* left, TreeNode* right) : val(x), left(left), right(right) {}
};

// Key Terms:
// - Root: Top node (no parent)
// - Leaf: Node with no children
// - Height: Longest path from root to leaf
// - Depth: Distance from root to node
// - Subtree: Tree rooted at any node
// - Binary Tree: Each node has at most 2 children
```

## Binary Tree Fundamentals {#binary-tree-fundamentals}

### Basic Properties
```cpp
class BinaryTree {
private:
    TreeNode* root;

public:
    BinaryTree() : root(nullptr) {}
    
    // Calculate height of tree - O(n)
    int height(TreeNode* node) {
        if (!node) return -1; // or 0 depending on definition
        return 1 + max(height(node->left), height(node->right));
    }
    
    // Count total nodes - O(n)
    int countNodes(TreeNode* node) {
        if (!node) return 0;
        return 1 + countNodes(node->left) + countNodes(node->right);
    }
    
    // Count leaf nodes - O(n)
    int countLeaves(TreeNode* node) {
        if (!node) return 0;
        if (!node->left && !node->right) return 1;
        return countLeaves(node->left) + countLeaves(node->right);
    }
    
    // Check if tree is full (every node has 0 or 2 children)
    bool isFull(TreeNode* node) {
        if (!node) return true;
        if (!node->left && !node->right) return true;
        if (node->left && node->right) {
            return isFull(node->left) && isFull(node->right);
        }
        return false;
    }
    
    // Check if tree is complete
    bool isComplete(TreeNode* root) {
        if (!root) return true;
        
        queue<TreeNode*> q;
        q.push(root);
        bool foundNull = false;
        
        while (!q.empty()) {
            TreeNode* node = q.front();
            q.pop();
            
            if (!node) {
                foundNull = true;
            } else {
                if (foundNull) return false; // Found node after null
                q.push(node->left);
                q.push(node->right);
            }
        }
        return true;
    }
    
    // Check if tree is perfect
    bool isPerfect(TreeNode* root) {
        int h = height(root);
        return isPerfectHelper(root, h, 0);
    }
    
private:
    bool isPerfectHelper(TreeNode* node, int height, int level) {
        if (!node) return true;
        if (!node->left && !node->right) return level == height;
        if (!node->left || !node->right) return false;
        
        return isPerfectHelper(node->left, height, level + 1) &&
               isPerfectHelper(node->right, height, level + 1);
    }
};
```

## Tree Traversals {#tree-traversals}

### Depth-First Traversals

#### 1. Preorder Traversal (Root -> Left -> Right)
```cpp
// Recursive Preorder
void preorderRecursive(TreeNode* root, vector<int>& result) {
    if (!root) return;
    result.push_back(root->val);        // Visit root
    preorderRecursive(root->left, result);   // Traverse left
    preorderRecursive(root->right, result);  // Traverse right
}

// Iterative Preorder using Stack
vector<int> preorderIterative(TreeNode* root) {
    vector<int> result;
    if (!root) return result;
    
    stack<TreeNode*> st;
    st.push(root);
    
    while (!st.empty()) {
        TreeNode* node = st.top();
        st.pop();
        result.push_back(node->val);
        
        // Push right first, then left (stack is LIFO)
        if (node->right) st.push(node->right);
        if (node->left) st.push(node->left);
    }
    
    return result;
}

// Morris Preorder (O(1) space)
vector<int> preorderMorris(TreeNode* root) {
    vector<int> result;
    TreeNode* curr = root;
    
    while (curr) {
        if (!curr->left) {
            result.push_back(curr->val);
            curr = curr->right;
        } else {
            TreeNode* pred = curr->left;
            while (pred->right && pred->right != curr) {
                pred = pred->right;
            }
            
            if (!pred->right) {
                pred->right = curr;
                result.push_back(curr->val);
                curr = curr->left;
            } else {
                pred->right = nullptr;
                curr = curr->right;
            }
        }
    }
    
    return result;
}
```

#### 2. Inorder Traversal (Left -> Root -> Right)
```cpp
// Recursive Inorder
void inorderRecursive(TreeNode* root, vector<int>& result) {
    if (!root) return;
    inorderRecursive(root->left, result);    // Traverse left
    result.push_back(root->val);        // Visit root
    inorderRecursive(root->right, result);   // Traverse right
}

// Iterative Inorder using Stack
vector<int> inorderIterative(TreeNode* root) {
    vector<int> result;
    stack<TreeNode*> st;
    TreeNode* curr = root;
    
    while (curr || !st.empty()) {
        // Go to leftmost node
        while (curr) {
            st.push(curr);
            curr = curr->left;
        }
        
        // Process current node
        curr = st.top();
        st.pop();
        result.push_back(curr->val);
        
        // Move to right subtree
        curr = curr->right;
    }
    
    return result;
}

// Morris Inorder (O(1) space)
vector<int> inorderMorris(TreeNode* root) {
    vector<int> result;
    TreeNode* curr = root;
    
    while (curr) {
        if (!curr->left) {
            result.push_back(curr->val);
            curr = curr->right;
        } else {
            TreeNode* pred = curr->left;
            while (pred->right && pred->right != curr) {
                pred = pred->right;
            }
            
            if (!pred->right) {
                pred->right = curr;
                curr = curr->left;
            } else {
                pred->right = nullptr;
                result.push_back(curr->val);
                curr = curr->right;
            }
        }
    }
    
    return result;
}
```

#### 3. Postorder Traversal (Left -> Right -> Root)
```cpp
// Recursive Postorder
void postorderRecursive(TreeNode* root, vector<int>& result) {
    if (!root) return;
    postorderRecursive(root->left, result);  // Traverse left
    postorderRecursive(root->right, result); // Traverse right
    result.push_back(root->val);        // Visit root
}

// Iterative Postorder using Two Stacks
vector<int> postorderIterative(TreeNode* root) {
    vector<int> result;
    if (!root) return result;
    
    stack<TreeNode*> st1, st2;
    st1.push(root);
    
    while (!st1.empty()) {
        TreeNode* node = st1.top();
        st1.pop();
        st2.push(node);
        
        if (node->left) st1.push(node->left);
        if (node->right) st1.push(node->right);
    }
    
    while (!st2.empty()) {
        result.push_back(st2.top()->val);
        st2.pop();
    }
    
    return result;
}

// Iterative Postorder using One Stack
vector<int> postorderOneStack(TreeNode* root) {
    vector<int> result;
    if (!root) return result;
    
    stack<TreeNode*> st;
    TreeNode* lastVisited = nullptr;
    
    while (root || !st.empty()) {
        if (root) {
            st.push(root);
            root = root->left;
        } else {
            TreeNode* peekNode = st.top();
            if (peekNode->right && lastVisited != peekNode->right) {
                root = peekNode->right;
            } else {
                result.push_back(peekNode->val);
                lastVisited = st.top();
                st.pop();
            }
        }
    }
    
    return result;
}
```

### Breadth-First Traversal (Level Order)
```cpp
// Standard Level Order
vector<int> levelOrder(TreeNode* root) {
    vector<int> result;
    if (!root) return result;
    
    queue<TreeNode*> q;
    q.push(root);
    
    while (!q.empty()) {
        TreeNode* node = q.front();
        q.pop();
        result.push_back(node->val);
        
        if (node->left) q.push(node->left);
        if (node->right) q.push(node->right);
    }
    
    return result;
}

// Level Order with Level Separation
vector<vector<int>> levelOrderByLevels(TreeNode* root) {
    vector<vector<int>> result;
    if (!root) return result;
    
    queue<TreeNode*> q;
    q.push(root);
    
    while (!q.empty()) {
        int levelSize = q.size();
        vector<int> currentLevel;
        
        for (int i = 0; i < levelSize; i++) {
            TreeNode* node = q.front();
            q.pop();
            currentLevel.push_back(node->val);
            
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        
        result.push_back(currentLevel);
    }
    
    return result;
}

// Zigzag Level Order
vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
    vector<vector<int>> result;
    if (!root) return result;
    
    queue<TreeNode*> q;
    q.push(root);
    bool leftToRight = true;
    
    while (!q.empty()) {
        int levelSize = q.size();
        vector<int> currentLevel(levelSize);
        
        for (int i = 0; i < levelSize; i++) {
            TreeNode* node = q.front();
            q.pop();
            
            int index = leftToRight ? i : levelSize - 1 - i;
            currentLevel[index] = node->val;
            
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
        
        leftToRight = !leftToRight;
        result.push_back(currentLevel);
    }
    
    return result;
}
```

## Binary Search Trees {#binary-search-trees}

### BST Properties and Operations
```cpp
class BST {
private:
    TreeNode* root;

public:
    BST() : root(nullptr) {}
    
    // Insert node - O(log n) average, O(n) worst
    TreeNode* insert(TreeNode* node, int val) {
        if (!node) return new TreeNode(val);
        
        if (val < node->val) {
            node->left = insert(node->left, val);
        } else if (val > node->val) {
            node->right = insert(node->right, val);
        }
        
        return node;
    }
    
    // Search node - O(log n) average, O(n) worst
    bool search(TreeNode* node, int val) {
        if (!node) return false;
        if (node->val == val) return true;
        
        if (val < node->val) {
            return search(node->left, val);
        } else {
            return search(node->right, val);
        }
    }
    
    // Iterative search
    bool searchIterative(TreeNode* node, int val) {
        while (node) {
            if (node->val == val) return true;
            if (val < node->val) {
                node = node->left;
            } else {
                node = node->right;
            }
        }
        return false;
    }
    
    // Find minimum
    TreeNode* findMin(TreeNode* node) {
        while (node && node->left) {
            node = node->left;
        }
        return node;
    }
    
    // Find maximum
    TreeNode* findMax(TreeNode* node) {
        while (node && node->right) {
            node = node->right;
        }
        return node;
    }
    
    // Delete node - O(log n) average, O(n) worst
    TreeNode* deleteNode(TreeNode* node, int val) {
        if (!node) return nullptr;
        
        if (val < node->val) {
            node->left = deleteNode(node->left, val);
        } else if (val > node->val) {
            node->right = deleteNode(node->right, val);
        } else {
            // Node to be deleted found
            if (!node->left) {
                TreeNode* temp = node->right;
                delete node;
                return temp;
            } else if (!node->right) {
                TreeNode* temp = node->left;
                delete node;
                return temp;
            }
            
            // Node with two children
            TreeNode* temp = findMin(node->right);
            node->val = temp->val;
            node->right = deleteNode(node->right, temp->val);
        }
        
        return node;
    }
    
    // Validate BST
    bool isValidBST(TreeNode* root) {
        return isValidBSTHelper(root, LONG_MIN, LONG_MAX);
    }
    
private:
    bool isValidBSTHelper(TreeNode* node, long long minVal, long long maxVal) {
        if (!node) return true;
        
        if (node->val <= minVal || node->val >= maxVal) {
            return false;
        }
        
        return isValidBSTHelper(node->left, minVal, node->val) &&
               isValidBSTHelper(node->right, node->val, maxVal);
    }
};
```

### BST Advanced Operations
```cpp
// Find Kth Smallest Element
int kthSmallest(TreeNode* root, int k) {
    stack<TreeNode*> st;
    TreeNode* curr = root;
    
    while (curr || !st.empty()) {
        while (curr) {
            st.push(curr);
            curr = curr->left;
        }
        
        curr = st.top();
        st.pop();
        k--;
        
        if (k == 0) return curr->val;
        
        curr = curr->right;
    }
    
    return -1; // Should not reach here
}

// Lowest Common Ancestor in BST
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    while (root) {
        if (p->val < root->val && q->val < root->val) {
            root = root->left;
        } else if (p->val > root->val && q->val > root->val) {
            root = root->right;
        } else {
            return root;
        }
    }
    return nullptr;
}

// Convert Sorted Array to BST
TreeNode* sortedArrayToBST(vector<int>& nums) {
    return sortedArrayToBSTHelper(nums, 0, nums.size() - 1);
}

TreeNode* sortedArrayToBSTHelper(vector<int>& nums, int left, int right) {
    if (left > right) return nullptr;
    
    int mid = left + (right - left) / 2;
    TreeNode* root = new TreeNode(nums[mid]);
    
    root->left = sortedArrayToBSTHelper(nums, left, mid - 1);
    root->right = sortedArrayToBSTHelper(nums, mid + 1, right);
    
    return root;
}

// Range Sum of BST
int rangeSumBST(TreeNode* root, int low, int high) {
    if (!root) return 0;
    
    if (root->val < low) {
        return rangeSumBST(root->right, low, high);
    } else if (root->val > high) {
        return rangeSumBST(root->left, low, high);
    } else {
        return root->val + 
               rangeSumBST(root->left, low, high) + 
               rangeSumBST(root->right, low, high);
    }
}
```

## Tree Construction Problems {#tree-construction}

### Build Tree from Traversals
```cpp
// Build Tree from Preorder and Inorder
TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
    unordered_map<int, int> inorderMap;
    for (int i = 0; i < inorder.size(); i++) {
        inorderMap[inorder[i]] = i;
    }
    
    int preorderIndex = 0;
    return buildTreeHelper(preorder, inorderMap, preorderIndex, 0, inorder.size() - 1);
}

TreeNode* buildTreeHelper(vector<int>& preorder, unordered_map<int, int>& inorderMap,
                         int& preorderIndex, int inorderStart, int inorderEnd) {
    if (inorderStart > inorderEnd) return nullptr;
    
    int rootVal = preorder[preorderIndex++];
    TreeNode* root = new TreeNode(rootVal);
    
    int inorderIndex = inorderMap[rootVal];
    
    root->left = buildTreeHelper(preorder, inorderMap, preorderIndex, 
                                inorderStart, inorderIndex - 1);
    root->right = buildTreeHelper(preorder, inorderMap, preorderIndex, 
                                 inorderIndex + 1, inorderEnd);
    
    return root;
}

// Build Tree from Postorder and Inorder
TreeNode* buildTreePostIn(vector<int>& postorder, vector<int>& inorder) {
    unordered_map<int, int> inorderMap;
    for (int i = 0; i < inorder.size(); i++) {
        inorderMap[inorder[i]] = i;
    }
    
    int postorderIndex = postorder.size() - 1;
    return buildTreePostInHelper(postorder, inorderMap, postorderIndex, 0, inorder.size() - 1);
}

TreeNode* buildTreePostInHelper(vector<int>& postorder, unordered_map<int, int>& inorderMap,
                               int& postorderIndex, int inorderStart, int inorderEnd) {
    if (inorderStart > inorderEnd) return nullptr;
    
    int rootVal = postorder[postorderIndex--];
    TreeNode* root = new TreeNode(rootVal);
    
    int inorderIndex = inorderMap[rootVal];
    
    // Note: Right subtree first in postorder
    root->right = buildTreePostInHelper(postorder, inorderMap, postorderIndex, 
                                       inorderIndex + 1, inorderEnd);
    root->left = buildTreePostInHelper(postorder, inorderMap, postorderIndex, 
                                      inorderStart, inorderIndex - 1);
    
    return root;
}
```

## Classic Problems with Three Approaches {#classic-problems}

### Problem 1: Maximum Depth of Binary Tree

#### Approach 1: Recursive DFS
```cpp
int maxDepth1(TreeNode* root) {
    if (!root) return 0;
    return 1 + max(maxDepth1(root->left), maxDepth1(root->right));
}
// Time: O(n), Space: O(h) where h is height
```

#### Approach 2: Iterative BFS
```cpp
int maxDepth2(TreeNode* root) {
    if (!root) return 0;
    
    queue<TreeNode*> q;
    q.push(root);
    int depth = 0;
    
    while (!q.empty()) {
        int levelSize = q.size();
        depth++;
        
        for (int i = 0; i < levelSize; i++) {
            TreeNode* node = q.front();
            q.pop();
            
            if (node->left) q.push(node->left);
            if (node->right) q.push(node->right);
        }
    }
    
    return depth;
}
// Time: O(n), Space: O(w) where w is maximum width
```

#### Approach 3: Iterative DFS with Stack
```cpp
int maxDepth3(TreeNode* root) {
    if (!root) return 0;
    
    stack<pair<TreeNode*, int>> st;
    st.push({root, 1});
    int maxDepth = 0;
    
    while (!st.empty()) {
        auto [node, depth] = st.top();
        st.pop();
        
        maxDepth = max(maxDepth, depth);
        
        if (node->left) st.push({node->left, depth + 1});
        if (node->right) st.push({node->right, depth + 1});
    }
    
    return maxDepth;
}
// Time: O(n), Space: O(h)
```

### Problem 2: Binary Tree Maximum Path Sum

#### Approach 1: Recursive with Global Variable
```cpp
class Solution1 {
private:
    int maxSum = INT_MIN;
    
    int maxPathHelper(TreeNode* node) {
        if (!node) return 0;
        
        int leftGain = max(maxPathHelper(node->left), 0);
        int rightGain = max(maxPathHelper(node->right), 0);
        
        int currentMax = node->val + leftGain + rightGain;
        maxSum = max(maxSum, currentMax);
        
        return node->val + max(leftGain, rightGain);
    }
    
public:
    int maxPathSum(TreeNode* root) {
        maxPathHelper(root);
        return maxSum;
    }
};
// Time: O(n), Space: O(h)
```

#### Approach 2: Recursive with Reference
```cpp
int maxPathSum2(TreeNode* root) {
    int maxSum = INT_MIN;
    maxPathHelper2(root, maxSum);
    return maxSum;
}

int maxPathHelper2(TreeNode* node, int& maxSum) {
    if (!node) return 0;
    
    int leftGain = max(maxPathHelper2(node->left, maxSum), 0);
    int rightGain = max(maxPathHelper2(node->right, maxSum), 0);
    
    int currentMax = node->val + leftGain + rightGain;
    maxSum = max(maxSum, currentMax);
    
    return node->val + max(leftGain, rightGain);
}
// Time: O(n), Space: O(h)
```

#### Approach 3: Iterative with Post-order
```cpp
int maxPathSum3(TreeNode* root) {
    if (!root) return 0;
    
    unordered_map<TreeNode*, int> maxDown;
    stack<TreeNode*> st;
    unordered_set<TreeNode*> visited;
    
    st.push(root);
    int maxSum = INT_MIN;
    
    while (!st.empty()) {
        TreeNode* node = st.top();
        
        bool leftProcessed = !node->left || visited.count(node->left);
        bool rightProcessed = !node->right || visited.count(node->right);
        
        if (leftProcessed && rightProcessed) {
            st.pop();
            visited.insert(node);
            
            int leftGain = node->left ? max(maxDown[node->left], 0) : 0;
            int rightGain = node->right ? max(maxDown[node->right], 0) : 0;
            
            int currentMax = node->val + leftGain + rightGain;
            maxSum = max(maxSum, currentMax);
            
            maxDown[node] = node->val + max(leftGain, rightGain);
        } else {
            if (node->left && !visited.count(node->left)) {
                st.push(node->left);
            }
            if (node->right && !visited.count(node->right)) {
                st.push(node->right);
            }
        }
    }
    
    return maxSum;
}
// Time: O(n), Space: O(n)
```

### Problem 3: Symmetric Tree

#### Approach 1: Recursive
```cpp
bool isSymmetric1(TreeNode* root) {
    return isMirror(root, root);
}

bool isMirror(TreeNode* t1, TreeNode* t2) {
    if (!t1 && !t2) return true;
    if (!t1 || !t2) return false;
    
    return (t1->val == t2->val) &&
           isMirror(t1->left, t2->right) &&
           isMirror(t1->right, t2->left);
}
// Time: O(n), Space: O(h)
```

#### Approach 2: Iterative with Queue
```cpp
bool isSymmetric2(TreeNode* root) {
    if (!root) return true;
    
    queue<TreeNode*> q;
    q.push(root->left);
    q.push(root->right);
    
    while (!q.empty()) {
        TreeNode* t1 = q.front(); q.pop();
        TreeNode* t2 = q.front(); q.pop();
        
        if (!t1 && !t2) continue;
        if (!t1 || !t2) return false;
        if (t1->val != t2->val) return false;
        
        q.push(t1->left);
        q.push(t2->right);
        q.push(t1->right);
        q.push(t2->left);
    }
    
    return true;
}
// Time: O(n), Space: O(n)
```

#### Approach 3: Iterative with Stack
```cpp
bool isSymmetric3(TreeNode* root) {
    if (!root) return true;
    
    stack<TreeNode*> st;
    st.push(root->left);
    st.push(root->right);
    
    while (!st.empty()) {
        TreeNode* t2 = st.top(); st.pop();
        TreeNode* t1 = st.top(); st.pop();
        
        if (!t1 && !t2) continue;
        if (!t1 || !t2) return false;
        if (t1->val != t2->val) return false;
        
        st.push(t1->left);
        st.push(t2->right);
        st.push(t1->right);
        st.push(t2->left);
    }
    
    return true;
}
// Time: O(n), Space: O(h)
```

## Advanced Tree Algorithms {#advanced-algorithms}

### 1. Lowest Common Ancestor (General Tree)
```cpp
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (!root || root == p || root == q) return root;
    
    TreeNode* left = lowestCommonAncestor(root->left, p, q);
    TreeNode* right = lowestCommonAncestor(root->right, p, q);
    
    if (left && right) return root;
    return left ? left : right;
}

// With parent pointers
TreeNode* lowestCommonAncestorWithParent(TreeNode* p, TreeNode* q) {
    unordered_set<TreeNode*> ancestors;
    
    // Collect all ancestors of p
    while (p) {
        ancestors.insert(p);
        p = p->parent;
    }
    
    // Find first common ancestor
    while (q) {
        if (ancestors.count(q)) return q;
        q = q->parent;
    }
    
    return nullptr;
}
```

### 2. Serialize and Deserialize Binary Tree
```cpp
class Codec {
public:
    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        string result = "";
        serializeHelper(root, result);
        return result;
    }
    
    void serializeHelper(TreeNode* node, string& result) {
        if (!node) {
            result += "null,";
            return;
        }
        
        result += to_string(node->val) + ",";
        serializeHelper(node->left, result);
        serializeHelper(node->right, result);
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        queue<string> nodes;
        stringstream ss(data);
        string item;
        
        while (getline(ss, item, ',')) {
            nodes.push(item);
        }
        
        return deserializeHelper(nodes);
    }
    
    TreeNode* deserializeHelper(queue<string>& nodes) {
        string val = nodes.front();
        nodes.pop();
        
        if (val == "null") return nullptr;
        
        TreeNode* root = new TreeNode(stoi(val));
        root->left = deserializeHelper(nodes);
        root->right = deserializeHelper(nodes);
        
        return root;
    }
};
```

### 3. Binary Tree Cameras
```cpp
class TreeCamera {
private:
    int cameras = 0;
    
    enum State {
        NOT_COVERED = 0,
        COVERED_WITHOUT_CAMERA = 1,
        COVERED_WITH_CAMERA = 2
    };
    
    int dfs(TreeNode* node) {
        if (!node) return COVERED_WITHOUT_CAMERA;
        
        int left = dfs(node->left);
        int right = dfs(node->right);
        
        // If any child is not covered, place camera here
        if (left == NOT_COVERED || right == NOT_COVERED) {
            cameras++;
            return COVERED_WITH_CAMERA;
        }
        
        // If any child has camera, this node is covered
        if (left == COVERED_WITH_CAMERA || right == COVERED_WITH_CAMERA) {
            return COVERED_WITHOUT_CAMERA;
        }
        
        // Both children are covered but without camera
        return NOT_COVERED;
    }
    
public:
    int minCameraCover(TreeNode* root) {
        if (dfs(root) == NOT_COVERED) {
            cameras++;
        }
        return cameras;
    }
};
```

## Balanced Trees {#balanced-trees}

### AVL Tree Implementation
```cpp
struct AVLNode {
    int val;
    int height;
    AVLNode* left;
    AVLNode* right;
    
    AVLNode(int x) : val(x), height(1), left(nullptr), right(nullptr) {}
};

class AVLTree {
private:
    AVLNode* root;
    
    int getHeight(AVLNode* node) {
        return node ? node->height : 0;
    }
    
    int getBalance(AVLNode* node) {
        return node ? getHeight(node->left) - getHeight(node->right) : 0;
    }
    
    void updateHeight(AVLNode* node) {
        if (node) {
            node->height = 1 + max(getHeight(node->left), getHeight(node->right));
        }
    }
    
    AVLNode* rotateRight(AVLNode* y) {
        AVLNode* x = y->left;
        AVLNode* T2 = x->right;
        
        x->right = y;
        y->left = T2;
        
        updateHeight(y);
        updateHeight(x);
        
        return x;
    }
    
    AVLNode* rotateLeft(AVLNode* x) {
        AVLNode* y = x->right;
        AVLNode* T2 = y->left;
        
        y->left = x;
        x->right = T2;
        
        updateHeight(x);
        updateHeight(y);
        
        return y;
    }
    
    AVLNode* insertHelper(AVLNode* node, int val) {
        // 1. Perform normal BST insertion
        if (!node) return new AVLNode(val);
        
        if (val < node->val) {
            node->left = insertHelper(node->left, val);
        } else if (val > node->val) {
            node->right = insertHelper(node->right, val);
        } else {
            return node; // Duplicate values not allowed
        }
        
        // 2. Update height
        updateHeight(node);
        
        // 3. Get balance factor
        int balance = getBalance(node);
        
        // 4. Perform rotations if unbalanced
        // Left Left Case
        if (balance > 1 && val < node->left->val) {
            return rotateRight(node);
        }
        
        // Right Right Case
        if (balance < -1 && val > node->right->val) {
            return rotateLeft(node);
        }
        
        // Left Right Case
        if (balance > 1 && val > node->left->val) {
            node->left = rotateLeft(node->left);
            return rotateRight(node);
        }
        
        // Right Left Case
        if (balance < -1 && val < node->right->val) {
            node->right = rotateRight(node->right);
            return rotateLeft(node);
        }
        
        return node;
    }

public:
    AVLTree() : root(nullptr) {}
    
    void insert(int val) {
        root = insertHelper(root, val);
    }
    
    bool isBalanced() {
        return isBalancedHelper(root) != -1;
    }
    
private:
    int isBalancedHelper(AVLNode* node) {
        if (!node) return 0;
        
        int leftHeight = isBalancedHelper(node->left);
        if (leftHeight == -1) return -1;
        
        int rightHeight = isBalancedHelper(node->right);
        if (rightHeight == -1) return -1;
        
        if (abs(leftHeight - rightHeight) > 1) return -1;
        
        return 1 + max(leftHeight, rightHeight);
    }
};
```

## Practice Problems {#practice-problems}

### Easy Level
1. **Maximum Depth of Binary Tree** (3 approaches)
2. **Same Tree** (Compare two trees)
3. **Invert Binary Tree** (Mirror tree)
4. **Symmetric Tree** (3 approaches)
5. **Path Sum** (Root to leaf)
6. **Minimum Depth of Binary Tree**
7. **Balanced Binary Tree**

### Medium Level
1. **Binary Tree Level Order Traversal** (All variations)
2. **Construct Binary Tree from Traversals**
3. **Binary Tree Zigzag Level Order Traversal**
4. **Validate Binary Search Tree**
5. **Kth Smallest Element in BST**
6. **Lowest Common Ancestor**
7. **Binary Tree Right Side View**
8. **Count Good Nodes in Binary Tree**
9. **Maximum Path Sum** (3 approaches)
10. **Serialize and Deserialize Binary Tree**

### Hard Level
1. **Binary Tree Maximum Path Sum**
2. **Recover Binary Search Tree**
3. **Binary Tree Cameras**
4. **Vertical Order Traversal**
5. **Binary Tree Postorder Traversal** (Iterative)
6. **Find Duplicate Subtrees**
7. **Binary Tree Preorder Traversal** (Morris)

### Advanced Challenges
1. **Threaded Binary Tree**
2. **Persistent Binary Tree**
3. **Splay Tree Implementation**
4. **Red-Black Tree Implementation**
5. **B-Tree Implementation**

## Key Takeaways

1. **Master Traversals**: Foundation for all tree algorithms
2. **Recursion is Natural**: Trees have recursive structure
3. **BST Properties**: Left < Root < Right enables efficient operations
4. **Balance Matters**: Keeps operations at O(log n)
5. **Multiple Approaches**: Recursive, iterative, Morris traversal
6. **Space-Time Tradeoffs**: Understand when to use what approach

## Next Chapter Preview

In **Chapter 11: Heaps and Priority Queues**, we'll explore:
- Min/Max heap implementation and operations
- Priority queue applications
- Heap sort algorithm
- Advanced heap variants (Fibonacci heap, Binomial heap)
- Top-K problems and their solutions

The tree foundation is solid! Time to prioritize our learning! 🏔️

---
*"Trees teach us that growth happens in many directions simultaneously, and the strongest structures have deep roots and balanced branches."*
