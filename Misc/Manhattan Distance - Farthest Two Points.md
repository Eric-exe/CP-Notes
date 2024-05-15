# Problem
Find the max Manhattan distance given an array of points.
```
Input: points = [[3,10],[5,15],[10,2],[4,4]]
Output: 18
```

# Algorithm

### Code
```cpp
/*
Input: points
Return: {max_dist, point1, point2}
*/
vector<int> getMxDistance(int skip) {
	vector<int> add = {INT_MIN, INT_MAX}, addIdx = {0, 0};
	vector<int> sub = {INT_MIN, INT_MAX}, subIdx = {0, 0};

	for (int i = 0; i < points.size(); i++) {
		if (i == skip) continue;

		int currAdd = points[i][0] + points[i][1];
		if (currAdd > add[0]) { add[0] = currAdd; addIdx[0] = i; }
		if (currAdd < add[1]) { add[1] = currAdd; addIdx[1] = i; }

		int currSub = points[i][0] - points[i][1];
		if (currSub > sub[0]) { sub[0] = currSub; subIdx[0] = i; }
		if (currSub < sub[1]) { sub[1] = currSub; subIdx[1] = i; }
	}

	int addDiff = add[0] - add[1], subDiff = sub[0] - sub[1];
	if (addDiff > subDiff) return {addDiff, addIdx[0], addIdx[1]};
	return {subDiff, subIdx[0], subIdx[1]};
}
```
### Analysis
Time Complexity: $O(N)$
Space Complexity: $O(1)$
# Proof
The max Manhattan Distance is 
$$max(max(x_i+y_i) - min(x_j+y_j), max(x_i-y_i)-min(x_j-y_j))$$
for all points $i$ and $j$ ($i$ can be equal to $j$).

---

Manhattan Distance is $|x_i - x_j|+|y_i-y_j|$
$$|x_i-x_j|+|y_i-y_j| = max \begin{cases}(x_i-x_j) + (y_i - y_j) & \\ (x_i-x_j) + (y_j - y_i) & \\ (x_j-x_i) + (y_i - y_j) & \\  (x_j-x_i) + (y_j - y_i) \end{cases}$$
Rewriting it as:
$$|x_i-x_j|+|y_i-y_j| = max \begin{cases} (x_i+y_i) - (x_j+y_j) & \\ (x_i-y_i) - (x_j-y_j) & \\ (x_j-y_j) - (x_i-y_i) & \\ (x_j+y_j) - (x_i+y_i) \end{cases}$$
To maximize the Manhattan distance for all points, then:
$$max(|x_i-x_j|+|y_i-y_j|) = max \begin{cases} max(x_i+y_i) - min(x_j+y_j) & \\ max(x_i-y_i) - min(x_j-y_j) & \\ max(x_j-y_j) - min(x_i-y_i) & \\ max(x_j+y_j) - min(x_i+y_i) \end{cases}$$
for all points $i$ and $j$.

### Problems
[Minimize Manhattan Distances - LeetCode](https://leetcode.com/problems/minimize-manhattan-distances/)