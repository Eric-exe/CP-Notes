Notice that the bits are always alternating in a pattern:
```
00000
00001
00010
00011
00100
00101
00110
00111
01000
01001
```
The pattern is as such (from the right):
* First bit alternates as: `0101010101010`
* Second bit as: `0011001100110011`
* Third bit as: `0000111100001111`
The alternation pattern increases by a power of 2 each time.
Note: We need to handle remainders of a cycle. 

First, we count the number of numbers in our input (which is our current number $N+1$ ). Then, for each bit $i$, starting with 0 as the bit with the least significance, the formula for counting the $i$-th bit is:
$$bitCount = \left\lfloor{\frac{N+1}{2^i}}\right\rfloor\cdot \frac{2^i}{2}+max(0, (N + 1) \; \bmod\; 2^i - \frac{2^i}{2} )$$

### Code
```cpp
pair<long long, vector<long long>> getBits(long long num) {
	num++;
	vector<long long> freq(48);
	long long res = 0;
	long long div = 2;
	int idx = 0;
	while (div < num) {
		freq[idx] += (num / div) * div / 2 + max(0LL, (num % div) - (div / 2));
		res += freq[idx++];
		div *= 2;
	}
	freq[idx] += max(num - div / 2, 0LL);
	res += freq[idx];
	return {res, freq};
}
```
