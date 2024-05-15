### Code
Also see: [[Fast Pow (Binary Exponentiation)]]

```cpp
int MOD = 1e9+7;

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
    return ((b % MOD) * (fastPow(a, MOD - 2)) % MOD) % MOD;
}
```
### Intuition
Our goal is to solve for:
$$\frac{b}{a} (\bmod p) $$
#### Theorem
This uses Fermat's Little Theorem which states that
$$a^p \equiv a (\bmod p)$$
where $p$ is a prime number. If $a$ is a coprime of $p$ (coprime: $\gcd(a, b)=1$), then 
$$a^{p - 1} \equiv 1 (\bmod p)$$
#### Modular Division
We need to solve for 
$$\frac{b}{a} (\bmod p)$$
where $p$ is a big prime number and $a$ is a coprime of $p$. However, we can't just mod $b$ by $p$ and mod $a$ by $p$. To do this, instead, we rewrite the function:
$$\frac{b}{a} (\bmod p) \equiv b (\bmod p) \cdot a^{-1} (\bmod p)$$
To get $a^{-1} \; (\bmod\; p)$, we can change Fermat's Little Theorem (with the property of $a$ being a coprime of $p$) by dividing both sides by $a$:
$$\frac{a^{p - 1}}{a} \equiv \frac{1}{a} (\bmod p)$$
$$a^{p - 2}  \equiv a^{-1} (\bmod p)$$
So, modular division is:
$$\frac{b}{a} (\bmod p) \equiv b (\bmod p) \cdot a^{p-2} (\bmod p)$$
