```cpp
long long fastPow(long long num, long long exp) {
	if (exp == 0) return 1;
	if (exp == 1) return num % MOD;
	if (exp % 2 == 0) {
		long long tmp = fastPow(num, exp / 2);
		return (tmp * tmp) % MOD;
	}
	else {
		long long tmp = fastPow(num, exp / 2);
		tmp = (tmp * tmp) % MOD;
		return (tmp * (num % MOD)) % MOD;
	}
}
```
