```cpp
vector<int> toBaseK(long long val, int k) {
	vector<int> digit;
	while (val) {
		digit.push_back(val % k);
		val /= k;
	}
	reverse(digit.begin(), digit.end());
	return digit;
}
```
