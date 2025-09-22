# Dijkstra-Algorithm
C++ implementation of Dijkstra's Algorithm
#include <iostream>
#include <vector>
#include <queue>
#include <stack>
#include <climits>

using namespace std;

// Structure to store a graph edge
struct Edge {
    int to;
    int weight;
};

// Function to implement Dijkstra's Algorithm
void dijkstra(int start, int end, vector<vector<Edge>>& graph) {
    int n = graph.size();
    vector<int> dist(n, INT_MAX);      // Distance from start node
    vector<int> prev(n, -1);           // To store the path
    dist[start] = 0;

    // Min-heap priority queue: (distance, node)
    priority_queue<pair<int,int>, vector<pair<int,int>>, greater<>> pq;
    pq.push({0, start});

    while (!pq.empty()) {
        int currDist = pq.top().first;
        int u = pq.top().second;
        pq.pop();

        if (currDist > dist[u])
            continue;

        for (auto edge : graph[u]) {
            int v = edge.to;
            int weight = edge.weight;
            if (dist[u] + weight < dist[v]) {
                dist[v] = dist[u] + weight;
                prev[v] = u;
                pq.push({dist[v], v});
            }
        }
    }

    // Print shortest path and cost
    if (dist[end] == INT_MAX) {
        cout << "No path exists from node " << start << " to node " << end << ".\n";
        return;
    }

    cout << "Shortest path cost: " << dist[end] << "\n";
    cout << "Shortest path: ";

    stack<int> path;
    for (int v = end; v != -1; v = prev[v])
        path.push(v);

    while (!path.empty()) {
        cout << path.top();
        path.pop();
        if (!path.empty()) cout << " -> ";
    }
    cout << endl;
}

int main() {
    // Example graph with 6 nodes (0 to 5)
    vector<vector<Edge>> graph(6);

    // Hard-coded edges (undirected graph example)
    graph[0].push_back({1, 7});
    graph[0].push_back({2, 9});
    graph[0].push_back({5, 14});
    graph[1].push_back({0, 7});
    graph[1].push_back({2, 10});
    graph[1].push_back({3, 15});
    graph[2].push_back({0, 9});
    graph[2].push_back({1, 10});
    graph[2].push_back({3, 11});
    graph[2].push_back({5, 2});
    graph[3].push_back({1, 15});
    graph[3].push_back({2, 11});
    graph[3].push_back({4, 6});
    graph[4].push_back({3, 6});
    graph[4].push_back({5, 9});
    graph[5].push_back({0, 14});
    graph[5].push_back({2, 2});
    graph[5].push_back({4, 9});

    int start, end;
    cout << "Enter starting node (0-5): ";
    cin >> start;
    cout << "Enter ending node (0-5): ";
    cin >> end;

    dijkstra(start, end, graph);

    return 0;
}
