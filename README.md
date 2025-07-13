# DSA Mnemonics: SALT HUGS FB Edition

A complete DSA reference using memorable mnemonics to ace coding interviews.

## The Three Sacred Mnemonics

**SALT HUGS FB** - Data Structures
**OF DUBS BGMT** - Algorithm Patterns  
**ACID FLUSH** - Time Complexity

## Data Structures: SALT HUGS FB

### S - Stack
LIFO operations, parentheses matching, function calls

```cpp
class Stack {
private:
    vector<int> data;
public:
    void push(int val) { data.push_back(val); }
    void pop() { if (!empty()) data.pop_back(); }
    int top() { return empty() ? -1 : data.back(); }
    bool empty() { return data.empty(); }
};
```

### A - Array
Random access, mathematical operations, sliding windows

```cpp
class DynamicArray {
private:
    vector<int> data;
public:
    void push_back(int val) { data.push_back(val); }
    int get(int index) { 
        return (index >= 0 && index < data.size()) ? data[index] : -1; 
    }
    void set(int index, int val) {
        if (index >= 0 && index < data.size()) data[index] = val;
    }
};
```

### L - Linked List
Dynamic size, frequent insertions/deletions

```cpp
struct ListNode {
    int val;
    ListNode* next;
    ListNode(int x) : val(x), next(nullptr) {}
};

class LinkedList {
private:
    ListNode* head;
public:
    LinkedList() : head(nullptr) {}
    
    void push_front(int val) {
        ListNode* newNode = new ListNode(val);
        newNode->next = head;
        head = newNode;
    }
    
    bool find(int val) {
        ListNode* curr = head;
        while (curr) {
            if (curr->val == val) return true;
            curr = curr->next;
        }
        return false;
    }
};
```

### T - Trie
Prefix matching, autocomplete, word games

```cpp
class TrieNode {
public:
    unordered_map<char, TrieNode*> children;
    bool isEndOfWord;
    TrieNode() : isEndOfWord(false) {}
};

class Trie {
private:
    TrieNode* root;
public:
    Trie() { root = new TrieNode(); }
    
    void insert(string word) {
        TrieNode* curr = root;
        for (char c : word) {
            if (curr->children.find(c) == curr->children.end()) {
                curr->children[c] = new TrieNode();
            }
            curr = curr->children[c];
        }
        curr->isEndOfWord = true;
    }
    
    bool search(string word) {
        TrieNode* curr = root;
        for (char c : word) {
            if (curr->children.find(c) == curr->children.end()) {
                return false;
            }
            curr = curr->children[c];
        }
        return curr->isEndOfWord;
    }
};
```

### H - Hash Table
Fast lookups, counting, caching

```cpp
class HashTable {
private:
    static const int BUCKET_SIZE = 100;
    vector<vector<pair<int, int>>> buckets;
    
    int hash(int key) { return abs(key) % BUCKET_SIZE; }
    
public:
    HashTable() : buckets(BUCKET_SIZE) {}
    
    void put(int key, int value) {
        int index = hash(key);
        for (auto& pair : buckets[index]) {
            if (pair.first == key) {
                pair.second = value;
                return;
            }
        }
        buckets[index].push_back({key, value});
    }
    
    int get(int key) {
        int index = hash(key);
        for (auto& pair : buckets[index]) {
            if (pair.first == key) return pair.second;
        }
        return -1;
    }
};
```

### U - Union Find
Connected components, cycle detection

```cpp
class UnionFind {
private:
    vector<int> parent, rank;
    
public:
    UnionFind(int n) : parent(n), rank(n, 0) {
        for (int i = 0; i < n; i++) parent[i] = i;
    }
    
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
    
    bool unite(int x, int y) {
        int rootX = find(x), rootY = find(y);
        if (rootX == rootY) return false;
        
        if (rank[rootX] < rank[rootY]) {
            parent[rootX] = rootY;
        } else if (rank[rootX] > rank[rootY]) {
            parent[rootY] = rootX;
        } else {
            parent[rootY] = rootX;
            rank[rootX]++;
        }
        return true;
    }
    
    bool connected(int x, int y) { return find(x) == find(y); }
};
```

### G - Graph
Relationships, paths, networks

```cpp
class Graph {
private:
    int V;
    vector<vector<int>> adjList;
    
public:
    Graph(int vertices) : V(vertices), adjList(vertices) {}
    
    void addEdge(int u, int v) {
        adjList[u].push_back(v);
        adjList[v].push_back(u);
    }
    
    void bfs(int start) {
        vector<bool> visited(V, false);
        queue<int> q;
        visited[start] = true;
        q.push(start);
        
        while (!q.empty()) {
            int vertex = q.front();
            q.pop();
            
            for (int neighbor : adjList[vertex]) {
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    q.push(neighbor);
                }
            }
        }
    }
};
```

### S - Set
Unique elements, membership testing

```cpp
class HashSet {
private:
    unordered_set<int> data;
    
public:
    void insert(int val) { data.insert(val); }
    void remove(int val) { data.erase(val); }
    bool contains(int val) { return data.find(val) != data.end(); }
    int size() { return data.size(); }
};
```

### F - Forest (Binary Tree)
Hierarchical data, searching

```cpp
struct TreeNode {
    int val;
    TreeNode* left;
    TreeNode* right;
    TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
};

class BinaryTree {
private:
    TreeNode* root;
    
public:
    BinaryTree() : root(nullptr) {}
    
    TreeNode* insert(TreeNode* node, int val) {
        if (!node) return new TreeNode(val);
        if (val < node->val) {
            node->left = insert(node->left, val);
        } else {
            node->right = insert(node->right, val);
        }
        return node;
    }
    
    void insert(int val) { root = insert(root, val); }
};
```

### B - Binary Heap
Priority queues, finding min/max

```cpp
class MinHeap {
private:
    vector<int> heap;
    
    void heapifyUp(int index) {
        if (index == 0) return;
        int parent = (index - 1) / 2;
        if (heap[index] < heap[parent]) {
            swap(heap[index], heap[parent]);
            heapifyUp(parent);
        }
    }
    
public:
    void push(int val) {
        heap.push_back(val);
        heapifyUp(heap.size() - 1);
    }
    
    int top() { return heap.empty() ? -1 : heap[0]; }
    bool empty() { return heap.empty(); }
};
```

## Algorithm Patterns: OF DUBS BGMT

### O - Two Pointers
Brute Force: "Check every pair with nested loops"
Two Pointers: "Slide through in O(n), efficiency queen"

```cpp
int left = 0, right = n - 1;
while (left < right) {
    if (condition_met) return result;
    else if (need_smaller) left++;
    else right--;
}
```

### F - Sliding Window
Brute Force: "Check every subarray separately"
Sliding Window: "One pass, keep moving, stay efficient"

```cpp
// Fixed window
int windowSum = 0;
for (int i = 0; i < k; i++) windowSum += arr[i];
int maxSum = windowSum;

for (int i = k; i < n; i++) {
    windowSum = windowSum - arr[i-k] + arr[i];
    maxSum = max(maxSum, windowSum);
}
```

### D - DFS
Brute Force: "Explore randomly without strategy"
DFS: "Go deep, backtrack smart, find solutions"

```cpp
void dfs(node, visited) {
    if (base_case) return;
    visited[node] = true;
    
    for (neighbor : node.neighbors) {
        if (!visited[neighbor]) {
            dfs(neighbor, visited);
        }
    }
}
```

### U - BFS
Brute Force: "Random exploration without levels"
BFS: "Level by level, shortest path guaranteed"

```cpp
queue<Node> q;
set<Node> visited;
q.push(start);

while (!q.empty()) {
    Node current = q.front();
    q.pop();
    
    for (neighbor : current.neighbors) {
        if (!visited.count(neighbor)) {
            visited.insert(neighbor);
            q.push(neighbor);
        }
    }
}
```

### B - Binary Search
Brute Force: "Check every element linearly"
Binary Search: "Divide and conquer, logarithmic legend"

```cpp
int left = 0, right = n - 1;
while (left <= right) {
    int mid = left + (right - left) / 2;
    if (arr[mid] == target) return mid;
    else if (arr[mid] < target) left = mid + 1;
    else right = mid - 1;
}
```

### G - Greedy
Brute Force: "Try every combination for optimal"
Greedy: "Best choice each step, trust the process"

```cpp
sort(input.begin(), input.end());
for (auto item : input) {
    if (can_take(item)) {
        result.add(item);
    }
}
```

### M - Dynamic Programming
Brute Force: "Solve same subproblems repeatedly"
DP: "Memoize like a pro, solve once, reuse forever"

```cpp
unordered_map<string, int> memo;

int dp(state) {
    if (base_case) return base_value;
    if (memo.count(state)) return memo[state];
    
    int result = 0;
    for (choice : choices) {
        result = max(result, choice + dp(new_state));
    }
    return memo[state] = result;
}
```

### T - Backtracking
Brute Force: "Generate everything without pruning"
Backtracking: "Try, fail fast, backtrack smart"

```cpp
void backtrack(current_state, result) {
    if (is_solution(current_state)) {
        result.push_back(current_state);
        return;
    }
    
    for (choice : available_choices) {
        current_state.add(choice);
        if (is_valid(current_state)) {
            backtrack(current_state, result);
        }
        current_state.remove(choice);
    }
}
```

## Time Complexity: ACID FLUSH

Quick Reference:
- Access: O(1) for arrays, O(n) for linked lists
- Create: Usually O(1) or O(n) for initialization
- Insert: O(1) to O(n) depending on structure
- Delete: O(1) to O(n) depending on structure
- Find: O(1) for hash, O(log n) for trees, O(n) for lists
- Lookup: Similar to access
- Update: Similar to access
- Search: O(log n) for sorted, O(n) for unsorted

## Bit Manipulation Hacks

Check power of 2:
```cpp
bool isPowerOfTwo(int n) {
    return n > 0 && (n & (n - 1)) == 0;
}
```

Get/Set/Clear bit:
```cpp
bool getBit(int num, int i) { return (num & (1 << i)) != 0; }
int setBit(int num, int i) { return num | (1 << i); }
int clearBit(int num, int i) { return num & ~(1 << i); }
```

Find unique element:
```cpp
int findUnique(vector<int>& nums) {
    int result = 0;
    for (int num : nums) result ^= num;
    return result;
}
```

## Interview Strategy

1. Read Problem → SALT HUGS FB → Pick data structure
2. Analyze Pattern → OF DUBS BGMT → Choose algorithm
3. Verify Efficiency → ACID FLUSH → Check complexity

Remember: SALT HUGS FB, OF DUBS BGMT, ACID FLUSH!
