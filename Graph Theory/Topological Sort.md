If node $a$ has to come before node $b$, we need to output a path that is valid.

### Kahn's Algorithm
1. Create graph adjacency list and indegree.
2. Push all nodes with indegree 0 into a queue.
3. While queue is not empty
	1. Push the node value into the resulting array
	2. For each node, decrement the indegree of all nodes it points to (neighbor)
		1. If the indegree of the neighbor == 0, then push the node into the queue.
4. If the resulting array is not the size of all the nodes, there is a cycle and thus, topological sort isn't possible.
5. Else, return the array which is a valid sort.