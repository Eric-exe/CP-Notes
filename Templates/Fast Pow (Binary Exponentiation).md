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
		if (exp & 1) res = (res * base) % MOD;
		base = (base * base) % MOD;
		exp >>= 1;
	}
	return res;
}
```

#### Intuition On Iterative
Represent the exponent as a binary number.
For example, $3^{11}$:
We represent the exponent as `1011` (least significant bit on the right). Now, the least significant bit just represents $3^1$ and $3^{11} = 3^1 \cdot 3^2 \cdot 3^8$. 

We can iterate through the binary version by using bit shifts and updating the base ($3^1, 3^2$, etc...). 

The logic then is:
1. Iterate through the bits from the least to most significant.
	1. If the current bit is 1, then we have to multiply our result by the current value associated with that bit. The value is the base raised to the power of the position of the bit (a bit at position 1 (1-indexed) from the right is $base^1 = base$). 
		1. We don't start at the 0th index because then, we just multiply the value by 1 which does nothing.
		2. **IMPORTANT**: We do **NOT** need to calculate the value of the associated bit with pow(base, position) because the value of the associated bit is the value of the previous bit (from the right) multiplied by itself. $3^2 = 3^1 \cdot 3^1$, $3^4 = 3^2 \cdot 3^2$, $3^8=3^4 \cdot 3^4$. 

Another version of the iterative code (with no bit manipulation):
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