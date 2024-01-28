## Cpp code: Using Krushkal's algorithm:
```
class Solution {
public:
    int minCostConnectPoints(vector<vector<int>>& points) {
         int n = points.size();
    vector<pair<int, pair<int, int>>> edges; // {cost, {point1, point2}}
    for (int i = 0; i < n; ++i) {
        for (int j = i + 1; j < n; ++j) {
            int cost = abs(points[i][0] - points[j][0]) + abs(points[i][1] - points[j][1]);
            edges.push_back({cost, {i, j}});
        }
    }
    sort(edges.begin(), edges.end());

    vector<int> parent(n);
    vector<int> rank(n, 1);

    for (int i = 0; i < n; ++i) {
        parent[i] = i;
    }

    int minCost = 0;
    int numEdges = 0;

    // Kruskal's algorithm
    for (auto& edge : edges) {
        int cost = edge.first;
        int u = edge.second.first;
        int v = edge.second.second;

        // Find the roots of the components containing u and v.
        int rootU = u, rootV = v;
        while (rootU != parent[rootU]) {
            rootU = parent[rootU];
        }
        while (rootV != parent[rootV]) {
            rootV = parent[rootV];
        }

        if (rootU != rootV) {
            // Unite the components and add the cost to the minimum cost.
            if (rank[rootU] < rank[rootV]) {
                swap(rootU, rootV);
            }
            parent[rootV] = rootU;
            if (rank[rootU] == rank[rootV]) {
                rank[rootU]++;
            }
            minCost += cost;
            numEdges++;
            if (numEdges == n - 1) {
                break; // MST is complete.
            }
        }
    }

    return minCost;
}

};
```
## Using Prim's algorithm:
```
class Solution {
public:
    int distance(vector<int>a,vector<int>b){
        return abs(a[1]-b[1]) + abs(a[0]-b[0]);
    }
    int minCostConnectPoints(vector<vector<int>>& points) {
        vector<pair<int,int>>adj[points.size()];
        for(int i=0;i<points.size()-1;i++){
            for(int j=i+1;j<points.size();j++){
                int w = (distance(points[i],points[j]));
                adj[i].push_back({w,j});
                adj[j].push_back({w,i});
            }
        }
        map<int,bool>visited;
        for(int i=0;i<points.size();i++){
            visited[i]=false;
        }
        int minpath=0;
        priority_queue<pair<int,int> ,vector<pair<int,int>>, greater<pair<int,int>>>pq;
        for(auto x:adj[0]){ // Sửa lỗi ở đây
            pq.push(x);
        }
        visited[0]=true;
        while(!pq.empty()){
            int u=pq.top().second;
            int w=pq.top().first;
            pq.pop();
            if(visited[u]==false){
                minpath+=w;
                visited[u]=true;
                for(auto x:adj[u]){
                    if(visited[x.second]==false){
                        pq.push(x);
                    }
                }
            }
        }
        return minpath;
    }
};
```
## Python code:
class Solution:
    def minCostConnectPoints(self, points: List[List[int]]) -> int:
        n = len(points)
        if n <= 1:
            return 0
        
        # Initialize the total cost and a set to track visited nodes
        total_cost = 0
        visited = set()
        
        # Initialize distances with a maximum value for each point
        distances = [float('inf')] * n
        
        # Choose the starting node 
        start_node = 0
        distances[start_node] = 0
        
        min_heap = [(0, start_node)]  # Priority queue to select the minimum distance
        
        while min_heap:
            dist, current = heapq.heappop(min_heap)
            
            if current in visited:
                continue
            
            visited.add(current)
            total_cost += dist
            
            for i in range(n):
                if i not in visited:
                    # Calculate Manhattan distance between points
                    manhattan_dist = abs(points[current][0] - points[i][0]) + abs(points[current][1] - points[i][1])
                    
                    if manhattan_dist < distances[i]:
                        distances[i] = manhattan_dist
                        heapq.heappush(min_heap, (manhattan_dist, i))
        
        return total_cost
