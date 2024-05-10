```cpp
// n is the size of the string
int BASE = 265;
int MOD = 1e9+9;

pows.resize(n, 1);
for (int i = 1; i < n; i++) pows[i] = (1LL * pows[i - 1] * BASE) % (int)(MOD);

string possible(int m, string& s) {
	if (m == 0) return "";
	unordered_map<int, vector<int>> hash;
	int i;
	long long currHash = 0;
	for (i = 0; i < m; i++) currHash = ((currHash * BASE) % MOD + (s[i] - 'a')) % MOD;
	hash[currHash] = {0};

	for (i = m; i < s.length(); i++) {
		currHash = ((currHash - (long long)pows[m - 1] * (s[i - m] - 'a')) % MOD + MOD) % MOD;
		currHash = (currHash * BASE + (s[i] - 'a')) % MOD;

		// check if currHash and hash2 exist in our map
		if (hash.find(currHash) == hash.end()) hash[currHash] = {i - m + 1};
		else {
			for (auto it : hash[currHash]) {
				if (s.substr(it, m) == s.substr(i - m + 1, m)) return s.substr(it, m);
			}
			hash[currHash].push_back(i - m + 1);
		}
	}

	return "";
}
```