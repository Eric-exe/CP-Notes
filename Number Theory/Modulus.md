$$(a+ b) \bmod m = (a \bmod m + b \bmod m) \bmod m$$$$(a-b)\bmod m = (a\bmod m - b \bmod m) \bmod m$$
$$(a\cdot b)\bmod m = (a\bmod m \cdot b \bmod m) \bmod m$$
### Inverse Mod
Using Fermat's Little Theorem (only if $m$ is a relatively huge prime, ie, bigger than input) or the base $a$ and $m$ are co-primes:
$$\frac{b}{a}\bmod m = ((b\bmod m) \cdot (a^{m-2} \bmod m)) \bmod m$$

---
### Misc
* $((a + b) \bmod a) = b$ for all $0 \leq b < a$