### Code
Also see: [[Fast Pow (Binary Exponentiation)]]

```cpp
int MOD = 1e9+7;

long long fastPow(long long base, long long exp) {
	long long res = 1;
	while (exp) {
		if (exp % 2) res = (res * (base % MOD)) % MOD;
		base = ((base % MOD) * (base % MOD)) % MOD;
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
This uses Fermat's Little Theorem which states that if $p$ is a prime number, then
$$a^p \equiv a(\bmod p)$$
See: 
* [Fermat's Little Theorem ← Number Theory (youtube.com)](https://www.youtube.com/watch?v=w0ZQvZLx2KA)
* [Fermat's little theorem - Wikipedia](https://en.wikipedia.org/wiki/Fermat%27s_little_theorem)

#### Modular Division
We need to solve for 
$$\frac{b}{a} (\bmod p)$$
where $p$ is a big prime number and p does not divide $a$ (since then $a(\bmod p)=0$) and we would be dividing by $0$. However, we can't just mod $b$ by $p$ and mod $a$ by $p$. To do this, instead, we rewrite the function:
$$\frac{b}{a} (\bmod p) \equiv b (\bmod p) \cdot a^{-1} (\bmod p)$$
To get $a^{-1} \; (\bmod\; p)$, we can change Fermat's Little Theorem by dividing both sides by $a^2$:
$$\frac{a^p}{a^2} \equiv\frac{a}{a^2}(\bmod p)\rightarrow a^{p - 2}  \equiv a^{-1} (\bmod p)$$

So, modular division is:
$$\frac{b}{a} (\bmod p) \equiv (b(\bmod p) \cdot a^{-1} (\bmod p))(\bmod p)$$
or:
$$(b (\bmod p) \cdot a^{p-2} (\bmod p)) (\bmod p)$$