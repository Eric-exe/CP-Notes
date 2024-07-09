* Can approximately be bound by $n^{\frac{1}{3}}$.
	* Max 1344 factors in integers up to $10^9$.
	* Max 103680 factors in integers up to $10^{18}$.

For distinct prime factors, its roughly $log(log(n))$. See [Hardy–Ramanujan theorem - Wikipedia](https://en.wikipedia.org/wiki/Hardy%E2%80%93Ramanujan_theorem)
### Factorization Algorithm
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