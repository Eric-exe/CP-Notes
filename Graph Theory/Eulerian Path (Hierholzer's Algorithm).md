#graph-theory #graph-traversal
### Problem
Given a set of edges, return a path that traverses through every edge exactly once.
```
Input: [[1,2],[2,4],[2,3],[3,2],[4,5]]
Output: [1,2,3,24,5]
```
#### Intuition
We need to traverse each path at most once.
### Algorithm
Requirements:
* graph $g$ -  hash table denoting the graph
* $in$ - hash table or array denoting the indegree of node $i$ for all nodes in the graph
* $out$ - hash table or array denoting the outdegree of node $i$ for all nodes in the graph
---
First, we need to check if the graph contains an Eulerian path. The requirements are:
* $abs(indegree(node_i) - outdegree(node_i))<=1$ for all nodes in graph
* There is exactly one node with $indegree(node_i) = outdegree(node_i) + 1$ and exactly one node with $outdegree(node_j) = indegree(node_j) + 1$ or
* $indegree(node_i)=outdegree(node_i)$ for all nodes in graph

```cpp
bool hasEulerian(unordered_map<int, vector<int>>& g, 
				 unordered_map<int, int>& in,
				 unordered_map<int, int>& out) {
	int startNodes = 0, endNodes = 0;
	for (auto& node : g) {
		if (abs(in[node] - out[node]) > 1) return false;
		else if (in[node] == out[node] + 1) startNodes++;
		else if (out[node] == in[node] + 1) endNodes++;
	}
	return ((startNodes == 0 && endNodes == 0) || 
			(startNodes == 1 && endNodes == 1));
}
```
---

Now, we can traverse. To choose the starting node, if we have a node with 

 
Idea: 

Create graph and indegree and outdegree. Start with a node with exactly 1 more outdegree than indegree if possible. 

Then traverse all possible path from node i, exhausting all possible options. Finally, append node i to the resulting array.

```cpp
void dfs(int node, vector<int>& v) {
	while (!g[node].empty()) {
		auto next = g[node].front(); g[node].pop();
		dfs(next, v);
	}
	v.push_back(node);
}
```


Finally, reverse and return the resulting array.

[Valid Arrangement of Pairs - LeetCode](https://leetcode.com/problems/valid-arrangement-of-pairs/submissions/1217926826/)
[Reconstruct Itinerary - LeetCode](https://leetcode.com/problems/reconstruct-itinerary/submissions/1217934081/)

https://www.youtube.com/watch?v=8MpoO2zA2l4
