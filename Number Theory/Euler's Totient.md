$\phi(n)$ is the number of co-primes from $[1,n]$ .
For example: $\phi(6)=2$ because $1$ and $5$ are co-primes of $6$, but not $2,3,4$ or $6$. 
### Formula:
$$\phi(n)=n\prod_{p|n}(1-\frac{1}{p})$$
where $p$ is a prime.
For example:
$12=2^2*3$
$\phi(12)=12\cdot\left(1-\frac{1}{2}\right)\cdot\left(1-\frac{1}{3}\right)=4$

For each prime factor $p$ that divides $n$, it removes multiples of the prime from $[1,n]$ since there are $\frac{n}{p}$ numbers that are not coprime to $n$. Note that this doesn't remove extra numbers because the multiplication corrects for over-removal. For instance, after removing multiples of 2, when removing multiples of 3, only half of them remain to be removed, since the other half were already excluded as multiples of 2.

### Properties: 
- $\phi(n)=\phi(x)\cdot\phi(y)$ if $x\cdot y = n$ and $\gcd(x, y)=1$.

### Code:
```cpp
vector<ll> phi(1e6 + 1);
for (int i = 1; i <= 1e6; i++) phi[i] = i;
for (int i = 2; i <= 1e6; i++) {
	if (phi[i] != i) continue;
	for (int j = i; j <= 1e6; j += i) phi[j] -= phi[j] / i;
}
```
It's a sieve, with the current value $x$ being multiplied by $(1-\frac{1}{i})$ for all its factors $i$. Subtraction is used since $\phi(n)(1-\frac{1}{p})=\phi(n)-\frac{\phi(n)}{i}$.  