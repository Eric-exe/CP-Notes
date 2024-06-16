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
    for (int i = 1; i <= FACT; i++) factorial.push_back((factorial.back() * i) % MOD);
    return factorial;
}

vector<long long> factorial = generateFactorial(FACT);

int C(int n, int r) {
    return invMod(factorial[n], (factorial[n - r] * factorial[r]) % MOD);
}
```