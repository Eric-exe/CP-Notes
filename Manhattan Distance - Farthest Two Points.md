# Problem
Find the max Manhattan distance given a array of points.
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
vector<int> getMaxDist(vector<vector<int>>& points) {
	vector<int> sum = {INT_MIN, INT_MAX};
	vector<int> sumIdx = {0, 0};
	vector<int> diff = {INT_MIN, INT_MAX};
	vector<int> diffIdx = {0, 0};

	int n = points.size();
	for (int i = 0; i < n; i++) {
		int currSum = points[i][0] + points[i][1];
		if (currSum > sum[0]) {
			sum[0] = currSum;
			sumIdx[0] = i;
		}
		if (currSum < sum[1]) {
			sum[1] = currSum;
			sumIdx[1] = i;
		}

		int currDiff = points[i][0] - points[i][0];
		if (currDiff > diff[0]) {
			diff[0] = currDiff;
			diffIdx[0] = i;
		}
		if (currDiff < diff[1]) {
			diff[1] = currDiff;
			diffIdx[1] = i;
		}
	}

	if (sum[0] - sum[1] > diff[0] - diff[1])
		return {sum[0] - sum[1], sumIdx[0], sumIdx[1]};
	return {diff[0] - diff[1], diffIdx[0], diffIdx[1]};
}
```
### Analysis
Time Complexity: $O(N)$
Space Complexity: $O(N)$
# Proof
The max Manhattan Distance is 
$$
max(max(x_i+y_i) - min(x_j+y_j), max(x_i-y_i)-min(x_j-y_j))
$$
for all points $i$ and $j$ ($i$ can be equal to $j$).

---

Manhattan Distance is $|x_i - x_j|+|y_i-y_j|$
$$
|x_i-x_j|+|y_i-y_j| = max
\begin{cases}
(x_i-x_j) + (y_i - y_j) & \\
(x_i-x_j) + (y_j - y_i) & \\
(x_j-x_i) + (y_i - y_j) & \\
(x_j-x_i) + (y_j - y_i)
\end{cases}
$$
Rewriting it as:
$$
|x_i-x_j|+|y_i-y_j| = max
\begin{cases}
(x_i+y_i) - (x_j+y_j) & \\
(x_i-y_i) - (x_j-y_j) & \\
(x_j-y_j) - (x_i-y_i) & \\
(x_j+y_j) - (x_i+y_i)
\end{cases}
$$
To maximize the Manhattan distance for all points, then:
$$max(|x_i-x_j|+|y_i-y_j|) = max
\begin{cases}
max(x_i+y_i) - min(x_j+y_j) & \\
max(x_i-y_i) - min(x_j-y_j) & \\
max(x_j-y_j) - min(x_i-y_i) & \\
max(x_j+y_j) - min(x_i+y_i)
\end{cases}$$
for all points $i$ and $j$.

### Problems
[Minimize Manhattan Distances - LeetCode](https://leetcode.com/problems/minimize-manhattan-distances/)