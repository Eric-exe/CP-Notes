```cpp
struct Pt {
    long long dx, dy, idx;
};

vector<vector<int>> convexHull(vector<vector<int>>& points) {
	sort(points.begin(), points.end());
	int n = points.size();
	// scan from left to right, picking the smallest slope
	// to build the bottom of the hull
	// slope should always increase, or stay the same
	stack<Pt> lowerHull;
	lowerHull.push({1, INT_MIN, 0});
	for (int i = 1; i < n; i++) {
		long long dx = points[i][0] - points[lowerHull.top().idx][0];
		long long dy = points[i][1] - points[lowerHull.top().idx][1];
		while (decreasing(lowerHull.top().dx, lowerHull.top().dy, dx, dy)) {
			lowerHull.pop();
			dx = points[i][0] - points[lowerHull.top().idx][0];
			dy = points[i][1] - points[lowerHull.top().idx][1];
		}
		lowerHull.push({dx, dy, i});
	}

	stack<Pt> upperHull;
	// slope should always increase when traversing backwards
	upperHull.push({1, INT_MIN, n - 1});
	for (int i = n - 2; i >= 0; i--) {
		long long dx = -1 * (points[i][0] - points[upperHull.top().idx][0]);
		long long dy = -1 * (points[i][1] - points[upperHull.top().idx][1]);
		while (decreasing(upperHull.top().dx, upperHull.top().dy, dx, dy)) {
			upperHull.pop();
			dx = -1 * (points[i][0] - points[upperHull.top().idx][0]);
			dy = -1 * (points[i][1] - points[upperHull.top().idx][1]);
		}
		upperHull.push({dx, dy, i});
	}

	set<int> ptsSeen;
	vector<vector<int>> res;
	while (!lowerHull.empty()) {
		int idx = lowerHull.top().idx; lowerHull.pop();
		if (ptsSeen.count(idx)) continue;
		ptsSeen.insert(idx);
		res.push_back(points[idx]);
	}
	while (!upperHull.empty()) {
		int idx = upperHull.top().idx; upperHull.pop();
		if (ptsSeen.count(idx)) continue;
		ptsSeen.insert(idx);
		res.push_back(points[idx]);
	}
	return res;
}

bool decreasing(long long dx1, long long dy1, long long dx2, long long dy2) {
	// returns true if the slope of of dy1/dx1 > dy2/dx2
	return dy1 * dx2 > dy2 * dx1;
}
```