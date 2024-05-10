### Recursive
```cpp
long long fastPow(long long base, long long exp) {
    if (exp == 0) return 1;
    if (exp == 1) return base;

    if (exp % 2 == 0) {
        long long res = fastPow(base, exp / 2);
        return (res * res) % MOD;
    }
    
    else return (fastPow(base, exp - 1) * base) % MOD;
}
```
### Iterative
```cpp
long long fastPow(long long base, long long exp) {
	long long res = 1;
	while (exp) {
		if (exp % 2) res = (res * base) % MOD;
		base = (base * base) % MOD;
		exp /= 2;
	}
	return res;
}
```
