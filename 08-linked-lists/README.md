# Chapter 8: Linked Lists - The Dynamic Memory Masters

## Table of Contents
- [Introduction to Linked Lists](#introduction)
- [Types of Linked Lists](#types)
- [Basic Operations](#basic-operations)
- [Advanced Techniques](#advanced-techniques)
- [Classic Problems](#classic-problems)
- [Three-Approach Methodology](#three-approach)
- [Practice Problems](#practice-problems)

## Introduction to Linked Lists {#introduction}

Linked Lists are dynamic data structures where elements (nodes) are stored in sequence, but not necessarily in contiguous memory locations. Each node contains data and a reference (pointer) to the next node.

### Why Linked Lists Matter in DSA
- **Dynamic Size**: Unlike arrays, linked lists can grow/shrink during runtime
- **Efficient Insertion/Deletion**: O(1) at known positions
- **Interview Favorites**: Extremely common in coding interviews
- **Foundation for Advanced Structures**: Trees, graphs, and other structures

### Memory Representation
```cpp
// Node structure
struct ListNode {
    int val;
    ListNode* next;
    ListNode() : val(0), next(nullptr) {}
    ListNode(int x) : val(x), next(nullptr) {}
    ListNode(int x, ListNode* next) : val(x), next(next) {}
};
```

## Types of Linked Lists {#types}

### 1. Singly Linked List
```cpp
class SinglyLinkedList {
private:
    ListNode* head;
    int size;

public:
    SinglyLinkedList() : head(nullptr), size(0) {}
    
    // Insert at beginning - O(1)
    void insertAtHead(int val) {
        ListNode* newNode = new ListNode(val);
        newNode->next = head;
        head = newNode;
        size++;
    }
    
    // Insert at end - O(n)
    void insertAtTail(int val) {
        ListNode* newNode = new ListNode(val);
        if (!head) {
            head = newNode;
        } else {
            ListNode* curr = head;
            while (curr->next) {
                curr = curr->next;
            }
            curr->next = newNode;
        }
        size++;
    }
    
    // Insert at position - O(n)
    void insertAtPosition(int pos, int val) {
        if (pos < 0 || pos > size) return;
        if (pos == 0) {
            insertAtHead(val);
            return;
        }
        
        ListNode* newNode = new ListNode(val);
        ListNode* curr = head;
        for (int i = 0; i < pos - 1; i++) {
            curr = curr->next;
        }
        newNode->next = curr->next;
        curr->next = newNode;
        size++;
    }
    
    // Delete by value - O(n)
    void deleteByValue(int val) {
        if (!head) return;
        
        if (head->val == val) {
            ListNode* temp = head;
            head = head->next;
            delete temp;
            size--;
            return;
        }
        
        ListNode* curr = head;
        while (curr->next && curr->next->val != val) {
            curr = curr->next;
        }
        
        if (curr->next) {
            ListNode* temp = curr->next;
            curr->next = curr->next->next;
            delete temp;
            size--;
        }
    }
    
    // Search - O(n)
    bool search(int val) {
        ListNode* curr = head;
        while (curr) {
            if (curr->val == val) return true;
            curr = curr->next;
        }
        return false;
    }
    
    // Display - O(n)
    void display() {
        ListNode* curr = head;
        while (curr) {
            cout << curr->val << " -> ";
            curr = curr->next;
        }
        cout << "NULL\n";
    }
    
    // Get size - O(1)
    int getSize() { return size; }
    
    // Destructor
    ~SinglyLinkedList() {
        while (head) {
            ListNode* temp = head;
            head = head->next;
            delete temp;
        }
    }
};
```

### 2. Doubly Linked List
```cpp
struct DoublyListNode {
    int val;
    DoublyListNode* next;
    DoublyListNode* prev;
    DoublyListNode() : val(0), next(nullptr), prev(nullptr) {}
    DoublyListNode(int x) : val(x), next(nullptr), prev(nullptr) {}
};

class DoublyLinkedList {
private:
    DoublyListNode* head;
    DoublyListNode* tail;
    int size;

public:
    DoublyLinkedList() : head(nullptr), tail(nullptr), size(0) {}
    
    // Insert at head - O(1)
    void insertAtHead(int val) {
        DoublyListNode* newNode = new DoublyListNode(val);
        if (!head) {
            head = tail = newNode;
        } else {
            newNode->next = head;
            head->prev = newNode;
            head = newNode;
        }
        size++;
    }
    
    // Insert at tail - O(1)
    void insertAtTail(int val) {
        DoublyListNode* newNode = new DoublyListNode(val);
        if (!tail) {
            head = tail = newNode;
        } else {
            tail->next = newNode;
            newNode->prev = tail;
            tail = newNode;
        }
        size++;
    }
    
    // Delete from head - O(1)
    void deleteFromHead() {
        if (!head) return;
        
        if (head == tail) {
            delete head;
            head = tail = nullptr;
        } else {
            DoublyListNode* temp = head;
            head = head->next;
            head->prev = nullptr;
            delete temp;
        }
        size--;
    }
    
    // Delete from tail - O(1)
    void deleteFromTail() {
        if (!tail) return;
        
        if (head == tail) {
            delete tail;
            head = tail = nullptr;
        } else {
            DoublyListNode* temp = tail;
            tail = tail->prev;
            tail->next = nullptr;
            delete temp;
        }
        size--;
    }
    
    // Display forward
    void displayForward() {
        DoublyListNode* curr = head;
        while (curr) {
            cout << curr->val << " <-> ";
            curr = curr->next;
        }
        cout << "NULL\n";
    }
    
    // Display backward
    void displayBackward() {
        DoublyListNode* curr = tail;
        while (curr) {
            cout << curr->val << " <-> ";
            curr = curr->prev;
        }
        cout << "NULL\n";
    }
};
```

### 3. Circular Linked List
```cpp
class CircularLinkedList {
private:
    ListNode* tail; // Points to last node
    int size;

public:
    CircularLinkedList() : tail(nullptr), size(0) {}
    
    void insert(int val) {
        ListNode* newNode = new ListNode(val);
        if (!tail) {
            tail = newNode;
            tail->next = tail; // Points to itself
        } else {
            newNode->next = tail->next; // Point to head
            tail->next = newNode;
            tail = newNode; // Update tail
        }
        size++;
    }
    
    void display() {
        if (!tail) return;
        
        ListNode* curr = tail->next; // Start from head
        do {
            cout << curr->val << " -> ";
            curr = curr->next;
        } while (curr != tail->next);
        cout << "(back to start)\n";
    }
};
```

## Advanced Techniques {#advanced-techniques}

### 1. Two Pointer Technique
```cpp
// Fast and Slow Pointers (Floyd's Cycle Detection)
bool hasCycle(ListNode* head) {
    if (!head || !head->next) return false;
    
    ListNode* slow = head;
    ListNode* fast = head->next;
    
    while (fast && fast->next) {
        if (slow == fast) return true;
        slow = slow->next;
        fast = fast->next->next;
    }
    return false;
}

// Find middle of linked list
ListNode* findMiddle(ListNode* head) {
    if (!head) return nullptr;
    
    ListNode* slow = head;
    ListNode* fast = head;
    
    while (fast->next && fast->next->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    return slow;
}
```

### 2. Reversal Techniques
```cpp
// Iterative reversal
ListNode* reverseIterative(ListNode* head) {
    ListNode* prev = nullptr;
    ListNode* curr = head;
    
    while (curr) {
        ListNode* next = curr->next;
        curr->next = prev;
        prev = curr;
        curr = next;
    }
    return prev;
}

// Recursive reversal
ListNode* reverseRecursive(ListNode* head) {
    if (!head || !head->next) return head;
    
    ListNode* newHead = reverseRecursive(head->next);
    head->next->next = head;
    head->next = nullptr;
    return newHead;
}

// Reverse in groups of k
ListNode* reverseKGroup(ListNode* head, int k) {
    // Count nodes
    ListNode* curr = head;
    int count = 0;
    while (curr && count < k) {
        curr = curr->next;
        count++;
    }
    
    if (count == k) {
        curr = reverseKGroup(curr, k); // Reverse rest
        
        // Reverse current group
        while (count > 0) {
            ListNode* temp = head->next;
            head->next = curr;
            curr = head;
            head = temp;
            count--;
        }
        head = curr;
    }
    return head;
}
```

### 3. Merge Techniques
```cpp
// Merge two sorted lists
ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
    ListNode dummy(0);
    ListNode* curr = &dummy;
    
    while (l1 && l2) {
        if (l1->val <= l2->val) {
            curr->next = l1;
            l1 = l1->next;
        } else {
            curr->next = l2;
            l2 = l2->next;
        }
        curr = curr->next;
    }
    
    curr->next = l1 ? l1 : l2;
    return dummy.next;
}

// Merge k sorted lists
ListNode* mergeKLists(vector<ListNode*>& lists) {
    if (lists.empty()) return nullptr;
    
    while (lists.size() > 1) {
        vector<ListNode*> mergedLists;
        for (int i = 0; i < lists.size(); i += 2) {
            ListNode* l1 = lists[i];
            ListNode* l2 = (i + 1 < lists.size()) ? lists[i + 1] : nullptr;
            mergedLists.push_back(mergeTwoLists(l1, l2));
        }
        lists = mergedLists;
    }
    return lists[0];
}
```

## Classic Problems {#classic-problems}

### Problem 1: Remove Nth Node from End

**Three Approaches:**

#### Approach 1: Two Pass (Brute Force)
```cpp
ListNode* removeNthFromEnd1(ListNode* head, int n) {
    // First pass: count total nodes
    int length = 0;
    ListNode* curr = head;
    while (curr) {
        length++;
        curr = curr->next;
    }
    
    // Handle edge case
    if (n == length) {
        ListNode* newHead = head->next;
        delete head;
        return newHead;
    }
    
    // Second pass: find (length - n)th node
    curr = head;
    for (int i = 0; i < length - n - 1; i++) {
        curr = curr->next;
    }
    
    ListNode* nodeToDelete = curr->next;
    curr->next = curr->next->next;
    delete nodeToDelete;
    
    return head;
}
// Time: O(L), Space: O(1), where L is length
```

#### Approach 2: One Pass with Two Pointers
```cpp
ListNode* removeNthFromEnd2(ListNode* head, int n) {
    ListNode dummy(0);
    dummy.next = head;
    ListNode* first = &dummy;
    ListNode* second = &dummy;
    
    // Move first pointer n+1 steps ahead
    for (int i = 0; i <= n; i++) {
        first = first->next;
    }
    
    // Move both pointers until first reaches end
    while (first) {
        first = first->next;
        second = second->next;
    }
    
    // Remove the nth node from end
    ListNode* nodeToDelete = second->next;
    second->next = second->next->next;
    delete nodeToDelete;
    
    return dummy.next;
}
// Time: O(L), Space: O(1) - Single pass!
```

#### Approach 3: Recursive (Advanced)
```cpp
class Solution {
    int counter = 0;
public:
    ListNode* removeNthFromEnd3(ListNode* head, int n) {
        if (!head) return nullptr;
        
        head->next = removeNthFromEnd3(head->next, n);
        counter++;
        
        if (counter == n) {
            ListNode* nodeToDelete = head;
            head = head->next;
            delete nodeToDelete;
        }
        
        return head;
    }
};
// Time: O(L), Space: O(L) due to recursion stack
```

### Problem 2: Detect and Find Start of Cycle

#### Approach 1: Hash Set
```cpp
ListNode* detectCycle1(ListNode* head) {
    unordered_set<ListNode*> visited;
    ListNode* curr = head;
    
    while (curr) {
        if (visited.count(curr)) {
            return curr; // Cycle start found
        }
        visited.insert(curr);
        curr = curr->next;
    }
    return nullptr; // No cycle
}
// Time: O(n), Space: O(n)
```

#### Approach 2: Floyd's Cycle Detection
```cpp
ListNode* detectCycle2(ListNode* head) {
    if (!head || !head->next) return nullptr;
    
    // Phase 1: Detect if cycle exists
    ListNode* slow = head;
    ListNode* fast = head;
    
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) break;
    }
    
    if (!fast || !fast->next) return nullptr; // No cycle
    
    // Phase 2: Find cycle start
    ListNode* start = head;
    while (start != slow) {
        start = start->next;
        slow = slow->next;
    }
    
    return start;
}
// Time: O(n), Space: O(1) - Optimal!
```

#### Approach 3: Marking Nodes (Destructive)
```cpp
ListNode* detectCycle3(ListNode* head) {
    ListNode* curr = head;
    
    while (curr && curr->next) {
        if (curr->next == curr) {
            return curr; // Self loop
        }
        
        ListNode* next = curr->next;
        curr->next = curr; // Mark as visited
        curr = next;
    }
    
    return nullptr; // No cycle
}
// Time: O(n), Space: O(1) but destroys original list
```

## Three-Approach Methodology {#three-approach}

For every linked list problem, consider these three patterns:

### 1. **Brute Force Pattern**
- Usually involves multiple passes
- Higher time complexity but easier to understand
- Good for interviews to show thought process

### 2. **Optimized Pattern**
- Single pass with clever techniques
- Two pointers, dummy nodes, or mathematical insights
- Optimal time complexity

### 3. **Alternative Pattern**
- Recursive solutions
- Different data structures (hash maps, stacks)
- Trade-offs between time and space

## Advanced Problem Patterns

### 1. Palindrome Linked List
```cpp
// Three approaches to check if linked list is palindrome

// Approach 1: Use extra space (Array/Stack)
bool isPalindrome1(ListNode* head) {
    vector<int> vals;
    while (head) {
        vals.push_back(head->val);
        head = head->next;
    }
    
    int left = 0, right = vals.size() - 1;
    while (left < right) {
        if (vals[left] != vals[right]) return false;
        left++;
        right--;
    }
    return true;
}

// Approach 2: Reverse second half
bool isPalindrome2(ListNode* head) {
    if (!head || !head->next) return true;
    
    // Find middle
    ListNode* slow = head;
    ListNode* fast = head;
    while (fast->next && fast->next->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    
    // Reverse second half
    ListNode* secondHalf = reverseList(slow->next);
    
    // Compare
    ListNode* firstHalf = head;
    while (secondHalf) {
        if (firstHalf->val != secondHalf->val) return false;
        firstHalf = firstHalf->next;
        secondHalf = secondHalf->next;
    }
    
    return true;
}

// Approach 3: Recursive
bool isPalindromeHelper(ListNode*& left, ListNode* right) {
    if (!right) return true;
    
    bool result = isPalindromeHelper(left, right->next);
    if (!result) return false;
    
    result = (left->val == right->val);
    left = left->next;
    return result;
}

bool isPalindrome3(ListNode* head) {
    return isPalindromeHelper(head, head);
}
```

### 2. Copy List with Random Pointer
```cpp
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = nullptr;
        random = nullptr;
    }
};

// Approach 1: Hash Map
Node* copyRandomList1(Node* head) {
    if (!head) return nullptr;
    
    unordered_map<Node*, Node*> oldToNew;
    
    // First pass: create all nodes
    Node* curr = head;
    while (curr) {
        oldToNew[curr] = new Node(curr->val);
        curr = curr->next;
    }
    
    // Second pass: set next and random pointers
    curr = head;
    while (curr) {
        if (curr->next) {
            oldToNew[curr]->next = oldToNew[curr->next];
        }
        if (curr->random) {
            oldToNew[curr]->random = oldToNew[curr->random];
        }
        curr = curr->next;
    }
    
    return oldToNew[head];
}

// Approach 2: Interweaving nodes (Space optimized)
Node* copyRandomList2(Node* head) {
    if (!head) return nullptr;
    
    // Step 1: Create copy nodes and interweave
    Node* curr = head;
    while (curr) {
        Node* copy = new Node(curr->val);
        copy->next = curr->next;
        curr->next = copy;
        curr = copy->next;
    }
    
    // Step 2: Set random pointers
    curr = head;
    while (curr) {
        if (curr->random) {
            curr->next->random = curr->random->next;
        }
        curr = curr->next->next;
    }
    
    // Step 3: Separate the lists
    Node* dummy = new Node(0);
    Node* copyCurr = dummy;
    curr = head;
    
    while (curr) {
        copyCurr->next = curr->next;
        curr->next = curr->next->next;
        copyCurr = copyCurr->next;
        curr = curr->next;
    }
    
    Node* result = dummy->next;
    delete dummy;
    return result;
}
```

## Practice Problems {#practice-problems}

### Easy Level
1. **Reverse Linked List** (3 approaches: iterative, recursive, stack)
2. **Merge Two Sorted Lists**
3. **Remove Duplicates from Sorted List**
4. **Middle of Linked List**
5. **Intersection of Two Linked Lists**

### Medium Level
1. **Add Two Numbers** (Given as linked lists)
2. **Remove Nth Node from End**
3. **Swap Nodes in Pairs**
4. **Rotate List**
5. **Partition List**
6. **Sort List** (Merge sort on linked list)

### Hard Level
1. **Merge k Sorted Lists**
2. **Reverse Nodes in k-Group**
3. **Copy List with Random Pointer**
4. **LRU Cache Implementation**
5. **Flatten Multilevel Doubly Linked List**

### Competitive Programming Challenges
1. **Memory Management**: Implement your own memory pool
2. **Lock-Free Lists**: Concurrent linked list operations
3. **Skip Lists**: Probabilistic data structure
4. **XOR Linked Lists**: Memory-efficient doubly linked list

## Key Takeaways

1. **Master the Basics**: Insertion, deletion, traversal
2. **Two Pointers**: Most elegant solutions use this technique
3. **Dummy Nodes**: Simplify edge cases
4. **Recursive Thinking**: Many problems have elegant recursive solutions
5. **Memory Management**: Always clean up in C++
6. **Edge Cases**: Empty list, single node, cycles

## Next Chapter Preview

In **Chapter 9: Stacks and Queues**, we'll explore:
- Stack and Queue implementations
- Applications in expression evaluation
- Monotonic stack/queue techniques
- Deque and priority queue mastery
- Advanced problems like largest rectangle in histogram

The journey to becoming a DSA superhuman continues! 🚀

---
*"Linked Lists teach us that sometimes the most powerful connections are the ones we can't see directly, but follow through a chain of trust."*
