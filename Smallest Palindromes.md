Loop starting from 1.
All odd palindrome digits are smaller than even palindrome digits from $[i,i*10)$. In other words,
odd palindromes from $0$ to $9$ are smaller than even palindromes from $0$ to $9$,
odd palindromes from $10$ to $99$ are smaller than even palindromes from $10$ to $99$, etc...

```cpp
long long makePalin(long long val, bool odd) {
	long long res = val;
	long long tmp = res;
	if (odd) tmp /= 10;
	while (tmp) {
		res = res * 10 + tmp % 10;
		tmp /= 10;
	}
	return res;
}
```

#### Generate the K smallest palindromes
```cpp
int digit = 1;
while (k) {
	for (long long i = digit; i < digit * 10 && k; i++) {
		long long palin = makePalin(i, true);
		cout << palin << endl;
		k--;
	}

	for (long long i = digit; i < digit * 10 && k; i++) {
		long long palin = makePalin(i, false);
		cout << palin << endl;
		k--;
	}
	digit *= 10;
}
```
