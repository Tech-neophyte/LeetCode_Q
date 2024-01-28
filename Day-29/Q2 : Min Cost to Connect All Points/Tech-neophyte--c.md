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
