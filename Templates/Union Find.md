```cpp
class UnionFind {
public:
	int components;
	vector<int> parent;
	vector<int> rank;
	UnionFind(int n) {
		rank.resize(n, 1);
		components = n;
		parent.resize(n);
		for (int i = 0; i < n; i++) parent[i] = i;
	}

	int getParent(int i) {
		if (parent[i] != i) return parent[i] = getParent(parent[i]);
		return i;
	}

	bool connect(int i, int j) {
		int iParent = getParent(i);
		int jParent = getParent(j);
		if (iParent == jParent) return false;
        if (rank[iParent] < rank[jParent]) swap(iParent, jParent);
        rank[iParent] += rank[jParent];
        parent[jParent] = iParent;
		components--;
		return true;
	}

	bool isConnected(int i, int j) {
		return getParent(i) == getParent(j);
	}

	int getSize(int i) {
		return rank[getParent(i)];
	}
};
``````

