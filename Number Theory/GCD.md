* $\gcd(a, 0) = a$
* $\gcd(a,b) = \gcd(a - b, b)$ for all $a \geq b$
* $\gcd(a, b)=gcd(a, a+b)$
* $\gcd(a, b) \leq a$
* $\gcd(a, b) \leq b$
* $\gcd(a, b) \leq \min(a, b)$

---
### Euclidean Algorithm
```cpp
int gcd(int a, int b) {
    if (a == 0) return b;
    return gcd(b % a, a);
}
```
It uses the idea $\gcd(a,b) = \gcd(a - b, b)$ for all $a \geq b$ with a slight variation. Instead of continuously subtracting $b$ from $a$, it optimizes it by instead using modulus, which is, in essence, what the idea is doing.