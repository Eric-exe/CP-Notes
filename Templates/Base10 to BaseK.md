```cpp
vector<int> toBaseK(long long val, int k, int mnSz = 0) {
	vector<int> digit;
	while (val) {
		digit.push_back(val % k);
		val /= k;
	}
	while (digit.size() < mnSz) digit.push_back(0);
	reverse(digit.begin(), digit.end());
	return digit;
}
```
