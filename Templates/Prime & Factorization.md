### Sieve of Eratosthenes
```cpp
vector<int> sieve(int n) {
    vector<bool> isPrime(n + 1, true);
    vector<int> res;
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; i <= sqrt(n); i++) {
        if (isPrime[i]) {
            for (int j = i * i; j <= n; j += i) isPrime[j] = false;
        }
    }
    for (int i = 0; i < n; i++) {
        if (isPrime[i]) res.push_back(i);
    }
    return res;
}

vector<bool> sieve(int n) {
    vector<bool> isPrime(n + 1, true);
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; i <= sqrt(n); i++) {
        if (isPrime[i]) {
            for (int j = i * i; j <= n; j += i) isPrime[j] = false;
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
		if (p * p > n) break;
		if (p > n) break;
		while (n % p == 0) {
			res.push_back(p);
			n /= p;
		}
	}
	if (n != 1) res.push_back(n);
	return res;
}
```
#### Time complexity: 
$O(\pi(\sqrt{n}))+log(n))$
$\pi(\sqrt{n})$ is the number of primes the algorithm traverses.
$log(n$) is there for multiple of the same primes (e.g. $2^{30}$).

Using the prime number theorem: $\pi(\sqrt{n})=\frac{\sqrt{n}}{log(\sqrt{n})}$
So time complexity is really: $O(\frac{\sqrt{n}}{log(\sqrt{n})} + log(n))$.

#### Explanation of `p * p > n`:
If $p\cdot p>n$, then all possible smaller prime factors of $n$ have already been removed. If $n > 1$ at this point, then $n$ must be prime, because any composite number would have a prime factor $\leq \sqrt{n}$, and we've already checked such primes. Therefore, we can safely stop the loop and, if needed, append $n$ as the last prime factor.

This follows from the fact that there can b at most one prime factor greater than $\sqrt{n}$. If there were two such prime factors, say $p > \sqrt{n}$ and $q > \sqrt{n}$ $, then their product $pq > n$, which contradicts the assumption that both divide $n$, since their product would then have to divide $n$ as well.
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