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

vector<bool> sieve(int n) {
    vector<bool> isPrime(n, true);
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; i <= sqrt(n); i++) {
        if (isPrime[i]) {
            for (int j = i * 2; j <= n; j += i) isPrime[j] = false;
        }
    }
    return isPrime;
}

vector<bool> primes = sieve(100);
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

### Normal Factorization (Not in Order)
```cpp
vector<int> factorize(int n) {
    vector<int> factors;
    for (int i = 1; i * i <= n; i++) {
        if (n % i) continue;
        factors.push_back(i);
        if (i * i != n) factors.push_back(n / i);
    }
    return factors;
}

// TC: O(sqrtN)
```
### Uses
* grouping numbers by GCD
	* prime factorize $\rightarrow$ union primes and map the current number to a prime