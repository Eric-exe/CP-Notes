### Code
Supports calculating with modulus:
```cpp
int MOD = 1e9+7;
int FACT = 20000;

long long fastPow(long long base, long long exp) {
	long long res = 1;
	while (exp) {
		if (exp % 2) res = (res * base) % MOD;
		base = (base * base) % MOD;
		exp /= 2;
	}
	return res;
}

long long invMod(long long b, long long a) {
    return ((b % MOD) * (fastPow(a, MOD - 2) % MOD)) % MOD;
}

vector<long long> generateFactorial(int n) {
    vector<long long> factorial = {1};
    for (int i = 1; i <= n; i++) factorial.push_back((factorial.back() * i) % MOD);
    return factorial;
}

vector<long long> factorial = generateFactorial(FACT);

int C(int n, int r) {
    return invMod(factorial[n], (factorial[n - r] * factorial[r]) % MOD);
}
```

### Pascal's Triangle (For a lot of frequent, small combinations)
```cpp
vector<vector<ll>> pascals(int n, int mod) {
    vector<vector<ll>> dp(n+1, vector<ll>(n+1, 0));
    for (int i = 0; i <= n; i++) {
        dp[i][0] = 1;
        for (int j = 1; j <= i; j++) {
            dp[i][j] = (dp[i-1][j-1] + dp[i-1][j]) % mod;
        }
    }
    return dp;
}
// Returns a combination table C[n][r]
```
[Proof: Recursive Identity for Binomial Coefficients | Combinatorics](https://www.youtube.com/watch?v=PZ-3d7u_TU0)

### Intuition
> Order doesn't matter

How many ways can I choose $r$ items from a selection of $n$ items?
$$\binom{n}{r}\equiv\frac{n!}{r!(n-r)!}$$

---
What if there is an infinite selection of $n$ items and we need to pick $r$ items? In the first scenario, once we picked an item, we can't pick it again. However, we can pick again in this case:
$$\binom{n+r-1}{r-1}\equiv\frac{(n+r-1)!}{(r-1)!(n-(r-1))!}$$

---

### Combinatorics
$$\sum_{x=1}^{n}\binom{n}{x} = 2^n-1$$
#### Intuition
How many ways can we choose $n$ items such that we pick at least 1 item. That is $\binom{n}{1} + \binom{n}{2} + ... + \binom{n}{n}$ as we have to take into account each way to choose $x$ items. However, if we look at it a different way and instead consider the choice at each item (choose/not choose), we have $2^n$ choices. However, this includes not choosing any item at all so we subtract 1.


$$\binom{n}{2} = \frac{n(n - 1)}{2}$$
To choose 2 items, we would need to pick the first item and then the second item. However, since order doesn't matter, picking $(a, b)$ is the same as picking $(b, a)$ so divide by 2.

For 3 items:
$$\binom{n}{3} = \frac{n(n - 1)(n-2)}{6}$$