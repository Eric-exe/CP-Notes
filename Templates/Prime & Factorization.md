### Sieve of Eratosthenes
```cpp
vector<int> sieve(int n) {
    vector<bool> isPrime(n, true);
    vector<int> res;
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; i <= sqrt(n); i++) {
        if (isPrime[i]) {
            for (int j = i * 2; j <= n; j += i) isPrime[j] = false;
        }
    }
    for (int i = 0; i < n; i++) {
        if (isPrime[i]) res.push_back(i);
    }
    return res;
}
```

### Prime Factorization
Run sieve first
```cpp
vector<int> toPrime(int n) {
	vector<int> res;
	for (auto& p : primes) {
		if (n % p == 0) {
			while (n % p == 0) {
				res.push_back(p);
				n /= p;
			}
		}
		else if (p > n) return res;
	}
	if (n != 1) res.push_back(n);
	return res;
}
```
### Uses
* grouping numbers by GCD
	* prime factorize $\rightarrow$ union primes and map the current number to a prime