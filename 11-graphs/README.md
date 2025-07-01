# Chapter 11: Graphs - The Network Masters

## Table of Contents
- [Introduction to Graphs](#introduction)
- [Graph Representations](#graph-representations)
- [Graph Traversal Algorithms](#graph-traversal)
- [Shortest Path Algorithms](#shortest-path)
- [Minimum Spanning Tree](#minimum-spanning-tree)
- [Topological Sorting](#topological-sorting)
- [Graph Coloring and Bipartite Graphs](#graph-coloring)
- [Advanced Graph Algorithms](#advanced-algorithms)
- [Classic Problems with Three Approaches](#classic-problems)
- [Practice Problems](#practice-problems)

## Introduction to Graphs {#introduction}

Graphs are non-linear data structures consisting of vertices (nodes) connected by edges. They model relationships and connections in real-world scenarios.

### Why Graphs Matter in DSA
- **Social Networks**: Friend connections, influence propagation
- **Transportation**: Road networks, flight routes, GPS navigation
- **Computer Networks**: Internet topology, routing protocols
- **Dependencies**: Task scheduling, build systems
- **Optimization**: Resource allocation, matching problems
- **AI/ML**: Neural networks, recommendation systems

### Graph Terminology
```cpp
// Basic graph concepts:
// - Vertex/Node: Individual element in graph
// - Edge: Connection between two vertices  
// - Degree: Number of edges connected to a vertex
// - Path: Sequence of vertices connected by edges
// - Cycle: Path that starts and ends at same vertex
// - Connected: Every pair of vertices has a path
// - Weighted: Edges have associated weights/costs
// - Directed: Edges have direction (one-way)
// - Undirected: Edges are bidirectional

// Graph Classifications:
// 1. Directed vs Undirected
// 2. Weighted vs Unweighted  
// 3. Connected vs Disconnected
// 4. Cyclic vs Acyclic (DAG - Directed Acyclic Graph)
// 5. Dense vs Sparse
```

## Graph Representations {#graph-representations}

### 1. Adjacency Matrix
```cpp
class GraphMatrix {
private:
    vector<vector<int>> adj;
    int numVertices;
    bool isDirected;

public:
    GraphMatrix(int n, bool directed = false) 
        : numVertices(n), isDirected(directed) {
        adj.resize(n, vector<int>(n, 0));
    }
    
    // Add edge - O(1)
    void addEdge(int u, int v, int weight = 1) {
        if (u >= numVertices || v >= numVertices) return;
        
        adj[u][v] = weight;
        if (!isDirected) {
            adj[v][u] = weight;
        }
    }
    
    // Remove edge - O(1)
    void removeEdge(int u, int v) {
        if (u >= numVertices || v >= numVertices) return;
        
        adj[u][v] = 0;
        if (!isDirected) {
            adj[v][u] = 0;
        }
    }
    
    // Check if edge exists - O(1)
    bool hasEdge(int u, int v) {
        if (u >= numVertices || v >= numVertices) return false;
        return adj[u][v] != 0;
    }
    
    // Get all neighbors - O(V)
    vector<int> getNeighbors(int u) {
        vector<int> neighbors;
        for (int v = 0; v < numVertices; v++) {
            if (adj[u][v] != 0) {
                neighbors.push_back(v);
            }
        }
        return neighbors;
    }
    
    // Display graph - O(V²)
    void display() {
        for (int i = 0; i < numVertices; i++) {
            for (int j = 0; j < numVertices; j++) {
                cout << adj[i][j] << " ";
            }
            cout << "\n";
        }
    }
    
    // Space: O(V²), Good for dense graphs
};
```

### 2. Adjacency List
```cpp
class GraphList {
private:
    vector<vector<pair<int, int>>> adj; // {neighbor, weight}
    int numVertices;
    bool isDirected;

public:
    GraphList(int n, bool directed = false) 
        : numVertices(n), isDirected(directed) {
        adj.resize(n);
    }
    
    // Add edge - O(1)
    void addEdge(int u, int v, int weight = 1) {
        if (u >= numVertices || v >= numVertices) return;
        
        adj[u].push_back({v, weight});
        if (!isDirected) {
            adj[v].push_back({u, weight});
        }
    }
    
    // Remove edge - O(degree)
    void removeEdge(int u, int v) {
        if (u >= numVertices || v >= numVertices) return;
        
        adj[u].erase(
            remove_if(adj[u].begin(), adj[u].end(),
                     [v](const pair<int, int>& p) { return p.first == v; }),
            adj[u].end()
        );
        
        if (!isDirected) {
            adj[v].erase(
                remove_if(adj[v].begin(), adj[v].end(),
                         [u](const pair<int, int>& p) { return p.first == u; }),
                adj[v].end()
            );
        }
    }
    
    // Check if edge exists - O(degree)
    bool hasEdge(int u, int v) {
        if (u >= numVertices || v >= numVertices) return false;
        
        for (auto& edge : adj[u]) {
            if (edge.first == v) return true;
        }
        return false;
    }
    
    // Get all neighbors - O(1) to return reference
    const vector<pair<int, int>>& getNeighbors(int u) {
        return adj[u];
    }
    
    // Display graph - O(V + E)
    void display() {
        for (int i = 0; i < numVertices; i++) {
            cout << i << ": ";
            for (auto& edge : adj[i]) {
                cout << "(" << edge.first << "," << edge.second << ") ";
            }
            cout << "\n";
        }
    }
    
    // Space: O(V + E), Good for sparse graphs
};
```

### 3. Edge List
```cpp
struct Edge {
    int u, v, weight;
    Edge(int u, int v, int w = 1) : u(u), v(v), weight(w) {}
    
    bool operator<(const Edge& other) const {
        return weight < other.weight;
    }
};

class GraphEdgeList {
private:
    vector<Edge> edges;
    int numVertices;
    bool isDirected;

public:
    GraphEdgeList(int n, bool directed = false) 
        : numVertices(n), isDirected(directed) {}
    
    void addEdge(int u, int v, int weight = 1) {
        edges.push_back(Edge(u, v, weight));
        if (!isDirected) {
            edges.push_back(Edge(v, u, weight));
        }
    }
    
    const vector<Edge>& getEdges() {
        return edges;
    }
    
    void sortEdgesByWeight() {
        sort(edges.begin(), edges.end());
    }
    
    // Space: O(E), Good for algorithms that process all edges
};
```

## Graph Traversal Algorithms {#graph-traversal}

### Depth-First Search (DFS)
```cpp
class DFS {
public:
    // Recursive DFS
    void dfsRecursive(GraphList& graph, int start, vector<bool>& visited) {
        visited[start] = true;
        cout << start << " ";
        
        for (auto& edge : graph.getNeighbors(start)) {
            int neighbor = edge.first;
            if (!visited[neighbor]) {
                dfsRecursive(graph, neighbor, visited);
            }
        }
    }
    
    // Iterative DFS using Stack
    void dfsIterative(GraphList& graph, int start) {
        vector<bool> visited(graph.numVertices, false);
        stack<int> st;
        
        st.push(start);
        
        while (!st.empty()) {
            int vertex = st.top();
            st.pop();
            
            if (!visited[vertex]) {
                visited[vertex] = true;
                cout << vertex << " ";
                
                // Add neighbors to stack (reverse order for same traversal as recursive)
                for (auto it = graph.getNeighbors(vertex).rbegin(); 
                     it != graph.getNeighbors(vertex).rend(); ++it) {
                    if (!visited[it->first]) {
                        st.push(it->first);
                    }
                }
            }
        }
    }
    
    // DFS with path tracking
    bool dfsPath(GraphList& graph, int start, int target, vector<int>& path) {
        vector<bool> visited(graph.numVertices, false);
        return dfsPathHelper(graph, start, target, visited, path);
    }
    
private:
    bool dfsPathHelper(GraphList& graph, int current, int target, 
                      vector<bool>& visited, vector<int>& path) {
        visited[current] = true;
        path.push_back(current);
        
        if (current == target) return true;
        
        for (auto& edge : graph.getNeighbors(current)) {
            int neighbor = edge.first;
            if (!visited[neighbor]) {
                if (dfsPathHelper(graph, neighbor, target, visited, path)) {
                    return true;
                }
            }
        }
        
        path.pop_back(); // Backtrack
        return false;
    }
};
```

### Breadth-First Search (BFS)
```cpp
class BFS {
public:
    // Standard BFS
    void bfs(GraphList& graph, int start) {
        vector<bool> visited(graph.numVertices, false);
        queue<int> q;
        
        visited[start] = true;
        q.push(start);
        
        while (!q.empty()) {
            int vertex = q.front();
            q.pop();
            cout << vertex << " ";
            
            for (auto& edge : graph.getNeighbors(vertex)) {
                int neighbor = edge.first;
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    q.push(neighbor);
                }
            }
        }
    }
    
    // BFS with level tracking
    vector<vector<int>> bfsLevels(GraphList& graph, int start) {
        vector<vector<int>> levels;
        vector<bool> visited(graph.numVertices, false);
        queue<int> q;
        
        visited[start] = true;
        q.push(start);
        
        while (!q.empty()) {
            int levelSize = q.size();
            vector<int> currentLevel;
            
            for (int i = 0; i < levelSize; i++) {
                int vertex = q.front();
                q.pop();
                currentLevel.push_back(vertex);
                
                for (auto& edge : graph.getNeighbors(vertex)) {
                    int neighbor = edge.first;
                    if (!visited[neighbor]) {
                        visited[neighbor] = true;
                        q.push(neighbor);
                    }
                }
            }
            
            levels.push_back(currentLevel);
        }
        
        return levels;
    }
    
    // Shortest path in unweighted graph
    vector<int> shortestPath(GraphList& graph, int start, int target) {
        vector<bool> visited(graph.numVertices, false);
        vector<int> parent(graph.numVertices, -1);
        queue<int> q;
        
        visited[start] = true;
        q.push(start);
        
        while (!q.empty()) {
            int vertex = q.front();
            q.pop();
            
            if (vertex == target) {
                // Reconstruct path
                vector<int> path;
                int current = target;
                while (current != -1) {
                    path.push_back(current);
                    current = parent[current];
                }
                reverse(path.begin(), path.end());
                return path;
            }
            
            for (auto& edge : graph.getNeighbors(vertex)) {
                int neighbor = edge.first;
                if (!visited[neighbor]) {
                    visited[neighbor] = true;
                    parent[neighbor] = vertex;
                    q.push(neighbor);
                }
            }
        }
        
        return {}; // No path found
    }
};
```

## Shortest Path Algorithms {#shortest-path}

### Dijkstra's Algorithm (Single Source)
```cpp
class Dijkstra {
public:
    vector<int> dijkstra(GraphList& graph, int start) {
        vector<int> dist(graph.numVertices, INT_MAX);
        vector<bool> visited(graph.numVertices, false);
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        
        dist[start] = 0;
        pq.push({0, start});
        
        while (!pq.empty()) {
            int u = pq.top().second;
            pq.pop();
            
            if (visited[u]) continue;
            visited[u] = true;
            
            for (auto& edge : graph.getNeighbors(u)) {
                int v = edge.first;
                int weight = edge.second;
                
                if (!visited[v] && dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                    pq.push({dist[v], v});
                }
            }
        }
        
        return dist;
    }
    
    // Dijkstra with path reconstruction
    pair<vector<int>, vector<int>> dijkstraWithPath(GraphList& graph, int start) {
        vector<int> dist(graph.numVertices, INT_MAX);
        vector<int> parent(graph.numVertices, -1);
        vector<bool> visited(graph.numVertices, false);
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        
        dist[start] = 0;
        pq.push({0, start});
        
        while (!pq.empty()) {
            int u = pq.top().second;
            pq.pop();
            
            if (visited[u]) continue;
            visited[u] = true;
            
            for (auto& edge : graph.getNeighbors(u)) {
                int v = edge.first;
                int weight = edge.second;
                
                if (!visited[v] && dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                    parent[v] = u;
                    pq.push({dist[v], v});
                }
            }
        }
        
        return {dist, parent};
    }
    
    vector<int> getPath(const vector<int>& parent, int start, int target) {
        vector<int> path;
        int current = target;
        
        while (current != -1) {
            path.push_back(current);
            current = parent[current];
        }
        
        reverse(path.begin(), path.end());
        return path[0] == start ? path : vector<int>{}; // Return empty if no path
    }
};
```

### Bellman-Ford Algorithm (Handles Negative Weights)
```cpp
class BellmanFord {
public:
    pair<vector<int>, bool> bellmanFord(GraphEdgeList& graph, int start) {
        vector<int> dist(graph.numVertices, INT_MAX);
        dist[start] = 0;
        
        // Relax edges V-1 times
        for (int i = 0; i < graph.numVertices - 1; i++) {
            for (const Edge& edge : graph.getEdges()) {
                int u = edge.u, v = edge.v, weight = edge.weight;
                if (dist[u] != INT_MAX && dist[u] + weight < dist[v]) {
                    dist[v] = dist[u] + weight;
                }
            }
        }
        
        // Check for negative cycles
        bool hasNegativeCycle = false;
        for (const Edge& edge : graph.getEdges()) {
            int u = edge.u, v = edge.v, weight = edge.weight;
            if (dist[u] != INT_MAX && dist[u] + weight < dist[v]) {
                hasNegativeCycle = true;
                break;
            }
        }
        
        return {dist, hasNegativeCycle};
    }
};
```

### Floyd-Warshall Algorithm (All Pairs)
```cpp
class FloydWarshall {
public:
    vector<vector<int>> floydWarshall(GraphMatrix& graph) {
        vector<vector<int>> dist = graph.adj;
        int n = graph.numVertices;
        
        // Initialize distances
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (i == j) {
                    dist[i][j] = 0;
                } else if (dist[i][j] == 0) {
                    dist[i][j] = INT_MAX;
                }
            }
        }
        
        // Floyd-Warshall algorithm
        for (int k = 0; k < n; k++) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (dist[i][k] != INT_MAX && dist[k][j] != INT_MAX) {
                        dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
                    }
                }
            }
        }
        
        return dist;
    }
    
    // With path reconstruction
    pair<vector<vector<int>>, vector<vector<int>>> floydWarshallWithPath(GraphMatrix& graph) {
        vector<vector<int>> dist = graph.adj;
        vector<vector<int>> next(graph.numVertices, vector<int>(graph.numVertices, -1));
        int n = graph.numVertices;
        
        // Initialize
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (i == j) {
                    dist[i][j] = 0;
                } else if (dist[i][j] != 0) {
                    next[i][j] = j;
                } else {
                    dist[i][j] = INT_MAX;
                }
            }
        }
        
        // Floyd-Warshall with path tracking
        for (int k = 0; k < n; k++) {
            for (int i = 0; i < n; i++) {
                for (int j = 0; j < n; j++) {
                    if (dist[i][k] != INT_MAX && dist[k][j] != INT_MAX &&
                        dist[i][k] + dist[k][j] < dist[i][j]) {
                        dist[i][j] = dist[i][k] + dist[k][j];
                        next[i][j] = next[i][k];
                    }
                }
            }
        }
        
        return {dist, next};
    }
};
```

## Minimum Spanning Tree {#minimum-spanning-tree}

### Kruskal's Algorithm
```cpp
class UnionFind {
public:
    vector<int> parent, rank;
    
    UnionFind(int n) : parent(n), rank(n, 0) {
        for (int i = 0; i < n; i++) {
            parent[i] = i;
        }
    }
    
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]); // Path compression
        }
        return parent[x];
    }
    
    bool unite(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return false;
        
        // Union by rank
        if (rank[px] < rank[py]) {
            parent[px] = py;
        } else if (rank[px] > rank[py]) {
            parent[py] = px;
        } else {
            parent[py] = px;
            rank[px]++;
        }
        return true;
    }
};

class Kruskal {
public:
    vector<Edge> kruskalMST(GraphEdgeList& graph) {
        vector<Edge> mst;
        graph.sortEdgesByWeight();
        UnionFind uf(graph.numVertices);
        
        for (const Edge& edge : graph.getEdges()) {
            if (uf.unite(edge.u, edge.v)) {
                mst.push_back(edge);
                if (mst.size() == graph.numVertices - 1) {
                    break; // MST is complete
                }
            }
        }
        
        return mst;
    }
};
```

### Prim's Algorithm
```cpp
class Prim {
public:
    vector<Edge> primMST(GraphList& graph, int start = 0) {
        vector<Edge> mst;
        vector<bool> inMST(graph.numVertices, false);
        priority_queue<Edge, vector<Edge>, greater<Edge>> pq;
        
        inMST[start] = true;
        
        // Add all edges from start vertex
        for (auto& edge : graph.getNeighbors(start)) {
            pq.push(Edge(start, edge.first, edge.second));
        }
        
        while (!pq.empty() && mst.size() < graph.numVertices - 1) {
            Edge minEdge = pq.top();
            pq.pop();
            
            int u = minEdge.u, v = minEdge.v;
            
            // Skip if both vertices are already in MST
            if (inMST[u] && inMST[v]) continue;
            
            mst.push_back(minEdge);
            
            // Add the new vertex to MST
            int newVertex = inMST[u] ? v : u;
            inMST[newVertex] = true;
            
            // Add all edges from new vertex
            for (auto& edge : graph.getNeighbors(newVertex)) {
                if (!inMST[edge.first]) {
                    pq.push(Edge(newVertex, edge.first, edge.second));
                }
            }
        }
        
        return mst;
    }
};
```

## Topological Sorting {#topological-sorting}

### Kahn's Algorithm (BFS-based)
```cpp
class TopologicalSort {
public:
    vector<int> kahnTopologicalSort(GraphList& graph) {
        vector<int> inDegree(graph.numVertices, 0);
        vector<int> result;
        
        // Calculate in-degrees
        for (int u = 0; u < graph.numVertices; u++) {
            for (auto& edge : graph.getNeighbors(u)) {
                inDegree[edge.first]++;
            }
        }
        
        // Find vertices with 0 in-degree
        queue<int> q;
        for (int i = 0; i < graph.numVertices; i++) {
            if (inDegree[i] == 0) {
                q.push(i);
            }
        }
        
        // Process vertices
        while (!q.empty()) {
            int u = q.front();
            q.pop();
            result.push_back(u);
            
            // Reduce in-degree of neighbors
            for (auto& edge : graph.getNeighbors(u)) {
                int v = edge.first;
                inDegree[v]--;
                if (inDegree[v] == 0) {
                    q.push(v);
                }
            }
        }
        
        // Check for cycle
        if (result.size() != graph.numVertices) {
            return {}; // Cycle exists, no topological order
        }
        
        return result;
    }
    
    // DFS-based topological sort
    vector<int> dfsTopologicalSort(GraphList& graph) {
        vector<bool> visited(graph.numVertices, false);
        stack<int> st;
        
        for (int i = 0; i < graph.numVertices; i++) {
            if (!visited[i]) {
                dfsHelper(graph, i, visited, st);
            }
        }
        
        vector<int> result;
        while (!st.empty()) {
            result.push_back(st.top());
            st.pop();
        }
        
        return result;
    }
    
private:
    void dfsHelper(GraphList& graph, int u, vector<bool>& visited, stack<int>& st) {
        visited[u] = true;
        
        for (auto& edge : graph.getNeighbors(u)) {
            if (!visited[edge.first]) {
                dfsHelper(graph, edge.first, visited, st);
            }
        }
        
        st.push(u);
    }
};
```

## Classic Problems with Three Approaches {#classic-problems}

### Problem 1: Number of Islands

#### Approach 1: DFS (Recursive)
```cpp
int numIslands1(vector<vector<char>>& grid) {
    if (grid.empty()) return 0;
    
    int rows = grid.size(), cols = grid[0].size();
    int islands = 0;
    
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (grid[i][j] == '1') {
                islands++;
                dfsFlood(grid, i, j);
            }
        }
    }
    
    return islands;
}

void dfsFlood(vector<vector<char>>& grid, int i, int j) {
    if (i < 0 || i >= grid.size() || j < 0 || j >= grid[0].size() || 
        grid[i][j] != '1') {
        return;
    }
    
    grid[i][j] = '0'; // Mark as visited
    
    // Explore 4 directions
    dfsFlood(grid, i + 1, j);
    dfsFlood(grid, i - 1, j);
    dfsFlood(grid, i, j + 1);
    dfsFlood(grid, i, j - 1);
}
// Time: O(m*n), Space: O(m*n) in worst case due to recursion
```

#### Approach 2: BFS (Iterative)
```cpp
int numIslands2(vector<vector<char>>& grid) {
    if (grid.empty()) return 0;
    
    int rows = grid.size(), cols = grid[0].size();
    int islands = 0;
    vector<pair<int, int>> directions = {{1,0}, {-1,0}, {0,1}, {0,-1}};
    
    for (int i = 0; i < rows; i++) {
        for (int j = 0; j < cols; j++) {
            if (grid[i][j] == '1') {
                islands++;
                
                queue<pair<int, int>> q;
                q.push({i, j});
                grid[i][j] = '0';
                
                while (!q.empty()) {
                    auto [x, y] = q.front();
                    q.pop();
                    
                    for (auto [dx, dy] : directions) {
                        int nx = x + dx, ny = y + dy;
                        if (nx >= 0 && nx < rows && ny >= 0 && ny < cols && 
                            grid[nx][ny] == '1') {
                            grid[nx][ny] = '0';
                            q.push({nx, ny});
                        }
                    }
                }
            }
        }
    }
    
    return islands;
}
// Time: O(m*n), Space: O(min(m,n)) for queue
```

#### Approach 3: Union-Find
```cpp
class UnionFindGrid {
public:
    vector<int> parent, rank;
    int count;
    
    UnionFindGrid(vector<vector<char>>& grid) {
        int m = grid.size(), n = grid[0].size();
        parent.resize(m * n);
        rank.resize(m * n, 0);
        count = 0;
        
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (grid[i][j] == '1') {
                    int id = i * n + j;
                    parent[id] = id;
                    count++;
                }
            }
        }
    }
    
    int find(int x) {
        if (parent[x] != x) {
            parent[x] = find(parent[x]);
        }
        return parent[x];
    }
    
    void unite(int x, int y) {
        int px = find(x), py = find(y);
        if (px == py) return;
        
        if (rank[px] < rank[py]) {
            parent[px] = py;
        } else if (rank[px] > rank[py]) {
            parent[py] = px;
        } else {
            parent[py] = px;
            rank[px]++;
        }
        count--;
    }
};

int numIslands3(vector<vector<char>>& grid) {
    if (grid.empty()) return 0;
    
    int m = grid.size(), n = grid[0].size();
    UnionFindGrid uf(grid);
    vector<pair<int, int>> directions = {{1,0}, {0,1}};
    
    for (int i = 0; i < m; i++) {
        for (int j = 0; j < n; j++) {
            if (grid[i][j] == '1') {
                for (auto [di, dj] : directions) {
                    int ni = i + di, nj = j + dj;
                    if (ni < m && nj < n && grid[ni][nj] == '1') {
                        uf.unite(i * n + j, ni * n + nj);
                    }
                }
            }
        }
    }
    
    return uf.count;
}
// Time: O(m*n*α(m*n)), Space: O(m*n)
```

### Problem 2: Course Schedule (Cycle Detection)

#### Approach 1: DFS with Color States
```cpp
bool canFinish1(int numCourses, vector<vector<int>>& prerequisites) {
    vector<vector<int>> graph(numCourses);
    
    // Build adjacency list
    for (auto& pre : prerequisites) {
        graph[pre[1]].push_back(pre[0]);
    }
    
    vector<int> color(numCourses, 0); // 0: white, 1: gray, 2: black
    
    for (int i = 0; i < numCourses; i++) {
        if (color[i] == 0 && hasCycleDFS(graph, i, color)) {
            return false;
        }
    }
    
    return true;
}

bool hasCycleDFS(vector<vector<int>>& graph, int node, vector<int>& color) {
    color[node] = 1; // Mark as visiting (gray)
    
    for (int neighbor : graph[node]) {
        if (color[neighbor] == 1) return true; // Back edge found
        if (color[neighbor] == 0 && hasCycleDFS(graph, neighbor, color)) {
            return true;
        }
    }
    
    color[node] = 2; // Mark as visited (black)
    return false;
}
// Time: O(V + E), Space: O(V)
```

#### Approach 2: BFS with Kahn's Algorithm
```cpp
bool canFinish2(int numCourses, vector<vector<int>>& prerequisites) {
    vector<vector<int>> graph(numCourses);
    vector<int> inDegree(numCourses, 0);
    
    // Build graph and calculate in-degrees
    for (auto& pre : prerequisites) {
        graph[pre[1]].push_back(pre[0]);
        inDegree[pre[0]]++;
    }
    
    queue<int> q;
    for (int i = 0; i < numCourses; i++) {
        if (inDegree[i] == 0) {
            q.push(i);
        }
    }
    
    int processed = 0;
    while (!q.empty()) {
        int course = q.front();
        q.pop();
        processed++;
        
        for (int next : graph[course]) {
            inDegree[next]--;
            if (inDegree[next] == 0) {
                q.push(next);
            }
        }
    }
    
    return processed == numCourses;
}
// Time: O(V + E), Space: O(V)
```

#### Approach 3: Union-Find (Modified)
```cpp
bool canFinish3(int numCourses, vector<vector<int>>& prerequisites) {
    // This approach is less natural for cycle detection in directed graphs
    // but can be adapted using path compression and cycle detection
    
    vector<vector<int>> graph(numCourses);
    for (auto& pre : prerequisites) {
        graph[pre[1]].push_back(pre[0]);
    }
    
    vector<bool> visited(numCourses, false);
    vector<bool> recStack(numCourses, false);
    
    for (int i = 0; i < numCourses; i++) {
        if (!visited[i] && hasCycleUtil(graph, i, visited, recStack)) {
            return false;
        }
    }
    
    return true;
}

bool hasCycleUtil(vector<vector<int>>& graph, int node, 
                  vector<bool>& visited, vector<bool>& recStack) {
    visited[node] = true;
    recStack[node] = true;
    
    for (int neighbor : graph[node]) {
        if (!visited[neighbor]) {
            if (hasCycleUtil(graph, neighbor, visited, recStack)) {
                return true;
            }
        } else if (recStack[neighbor]) {
            return true;
        }
    }
    
    recStack[node] = false;
    return false;
}
// Time: O(V + E), Space: O(V)
```

## Advanced Graph Algorithms {#advanced-algorithms}

### Strongly Connected Components (Kosaraju's Algorithm)
```cpp
class StronglyConnectedComponents {
public:
    vector<vector<int>> findSCCs(GraphList& graph) {
        stack<int> finishOrder;
        vector<bool> visited(graph.numVertices, false);
        
        // Step 1: Fill vertices in stack according to their finishing times
        for (int i = 0; i < graph.numVertices; i++) {
            if (!visited[i]) {
                dfs1(graph, i, visited, finishOrder);
            }
        }
        
        // Step 2: Create transpose graph
        GraphList transpose = getTranspose(graph);
        
        // Step 3: Process vertices in order defined by stack
        fill(visited.begin(), visited.end(), false);
        vector<vector<int>> sccs;
        
        while (!finishOrder.empty()) {
            int v = finishOrder.top();
            finishOrder.pop();
            
            if (!visited[v]) {
                vector<int> component;
                dfs2(transpose, v, visited, component);
                sccs.push_back(component);
            }
        }
        
        return sccs;
    }
    
private:
    void dfs1(GraphList& graph, int v, vector<bool>& visited, stack<int>& finishOrder) {
        visited[v] = true;
        
        for (auto& edge : graph.getNeighbors(v)) {
            if (!visited[edge.first]) {
                dfs1(graph, edge.first, visited, finishOrder);
            }
        }
        
        finishOrder.push(v);
    }
    
    void dfs2(GraphList& graph, int v, vector<bool>& visited, vector<int>& component) {
        visited[v] = true;
        component.push_back(v);
        
        for (auto& edge : graph.getNeighbors(v)) {
            if (!visited[edge.first]) {
                dfs2(graph, edge.first, visited, component);
            }
        }
    }
    
    GraphList getTranspose(GraphList& graph) {
        GraphList transpose(graph.numVertices, true);
        
        for (int v = 0; v < graph.numVertices; v++) {
            for (auto& edge : graph.getNeighbors(v)) {
                transpose.addEdge(edge.first, v, edge.second);
            }
        }
        
        return transpose;
    }
};
```

### Articulation Points and Bridges (Tarjan's Algorithm)
```cpp
class TarjanAlgorithm {
private:
    vector<bool> visited;
    vector<int> disc, low, parent;
    vector<bool> articulationPoints;
    vector<pair<int, int>> bridges;
    int timer;

public:
    vector<bool> findArticulationPoints(GraphList& graph) {
        int n = graph.numVertices;
        visited.assign(n, false);
        disc.assign(n, -1);
        low.assign(n, -1);
        parent.assign(n, -1);
        articulationPoints.assign(n, false);
        timer = 0;
        
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                articulationPointsUtil(graph, i);
            }
        }
        
        return articulationPoints;
    }
    
    vector<pair<int, int>> findBridges(GraphList& graph) {
        int n = graph.numVertices;
        visited.assign(n, false);
        disc.assign(n, -1);
        low.assign(n, -1);
        parent.assign(n, -1);
        bridges.clear();
        timer = 0;
        
        for (int i = 0; i < n; i++) {
            if (!visited[i]) {
                bridgesUtil(graph, i);
            }
        }
        
        return bridges;
    }
    
private:
    void articulationPointsUtil(GraphList& graph, int u) {
        int children = 0;
        visited[u] = true;
        disc[u] = low[u] = ++timer;
        
        for (auto& edge : graph.getNeighbors(u)) {
            int v = edge.first;
            
            if (!visited[v]) {
                children++;
                parent[v] = u;
                articulationPointsUtil(graph, v);
                
                low[u] = min(low[u], low[v]);
                
                // Root is articulation point if it has more than one child
                if (parent[u] == -1 && children > 1) {
                    articulationPoints[u] = true;
                }
                
                // Non-root is articulation point if low value of child >= disc value of u
                if (parent[u] != -1 && low[v] >= disc[u]) {
                    articulationPoints[u] = true;
                }
            } else if (v != parent[u]) {
                low[u] = min(low[u], disc[v]);
            }
        }
    }
    
    void bridgesUtil(GraphList& graph, int u) {
        visited[u] = true;
        disc[u] = low[u] = ++timer;
        
        for (auto& edge : graph.getNeighbors(u)) {
            int v = edge.first;
            
            if (!visited[v]) {
                parent[v] = u;
                bridgesUtil(graph, v);
                
                low[u] = min(low[u], low[v]);
                
                // If low value of v is more than discovery value of u, then u-v is a bridge
                if (low[v] > disc[u]) {
                    bridges.push_back({u, v});
                }
            } else if (v != parent[u]) {
                low[u] = min(low[u], disc[v]);
            }
        }
    }
};
```

## Practice Problems {#practice-problems}

### Easy Level
1. **Find if Path Exists in Graph** (DFS/BFS/Union-Find)
2. **Number of Connected Components** (3 approaches)
3. **Valid Tree** (Cycle detection + connectivity)
4. **Clone Graph** (DFS/BFS)
5. **Find Center of Star Graph**

### Medium Level
1. **Number of Islands** (3 approaches covered)
2. **Course Schedule** (3 approaches covered)
3. **Pacific Atlantic Water Flow** (DFS/BFS from borders)
4. **Surrounded Regions** (Border DFS/BFS)
5. **Word Ladder** (BFS shortest path)
6. **Network Delay Time** (Dijkstra)
7. **Cheapest Flights Within K Stops** (Modified Dijkstra/Bellman-Ford)
8. **Redundant Connection** (Union-Find)
9. **Accounts Merge** (Union-Find/DFS)
10. **Critical Connections in Network** (Tarjan's bridges)

### Hard Level
1. **Word Ladder II** (BFS + DFS)
2. **Alien Dictionary** (Topological sort)
3. **Minimum Cost to Make Valid Path** (Dijkstra variant)
4. **Swim in Rising Water** (Binary search + DFS/Union-Find)
5. **Optimize Water Distribution** (MST)
6. **Critical Connections** (Tarjan's algorithm)
7. **Strongly Connected Components** (Kosaraju/Tarjan)

### Advanced Competitive Programming
1. **Maximum Flow** (Ford-Fulkerson, Dinic's algorithm)
2. **Minimum Cut** (Max-flow min-cut theorem)
3. **Hungarian Algorithm** (Assignment problem)
4. **Traveling Salesman Problem** (DP + bitmask)
5. **Graph Coloring** (Backtracking/Greedy)
6. **Hamiltonian Path** (DP + bitmask)

## Key Takeaways

1. **Choose Right Representation**: Adjacency list for sparse, matrix for dense graphs
2. **Master Traversals**: DFS for connectivity/cycles, BFS for shortest paths
3. **Union-Find**: Excellent for connectivity problems and MST
4. **Shortest Paths**: Dijkstra for non-negative, Bellman-Ford for negative weights
5. **Topological Sort**: Essential for DAGs and dependency resolution
6. **Advanced Algorithms**: Tarjan's for bridges/articulation points, strongly connected components

## Next Chapter Preview

In **Chapter 12: Dynamic Programming**, we'll explore:
- DP fundamentals and problem identification
- Memoization vs tabulation approaches
- Classic DP patterns (knapsack, LIS, edit distance)
- Advanced DP techniques (digit DP, bitmask DP)
- Optimization strategies and space complexity reduction

The graph connections are complete! Time to optimize our way through dynamic programming! 🚀

---
*"Graphs teach us that everything is connected, and the shortest path to understanding often requires exploring multiple routes before finding the optimal solution."*
