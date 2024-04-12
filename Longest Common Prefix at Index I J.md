```cpp
memset(pre, 0, sizeof(pre));
for (int i = n - 1; i >= 0; i--) {
	for (int j = n - 1; j >= 0; j--) {
		if (num[i] == num[j]) pre[i][j] = 1 + pre[i + 1][j + 1];
	}
}
```
