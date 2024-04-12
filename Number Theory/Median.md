#greedy

Intuition: 
- Minimum operations to make array uni-value (singular value).
- Choose a value with minimum distance from all values when summed up

The median element in an array is the best element with value $x$ to choose such that the 

$$
\sum_{i=0}^{n-1}|arr[i]-x|
$$
is minimized.

```
a, b, c, d, e
Choose c: dis(a) + dis(b) + dis(d) + dis(e) = 2 + 1 + 1 + 2 = 6
Choose b: dis(a) + dis(c) + dis(d) + dis(e) = 1 + 1 + 2 + 3 = 7
Choose d: dis(a) + dis(b) + dis(c) + dis(e) = 3 + 2 + 1 + 1 = 7
```

In a scenario where the array length is even, then both of the median candidates are valid.
For example:
```
1, 100, 101, 200
Choose 100: dist(1) + dist(101) + dist(200) = 99 + 1 + 100
Choose 101: dist(1) + dist(100) + dist(200) = 100 + 1 + 99

1, 150, 250, 300
Choose 150: dist(1) + dist(250) + dist(300) = 149 + 100 + 150 = 399
Choose 250: dist(1) + dist(150) + dist(300) = 249 + 100 + 50 = 399
```
Why not use mean?
Mean can be impacted by outliers. For example
```
1, 1, 1, 100000000000000000000000000000
The mean wants to be super close to the big number meaning we need to do 3 * (big number) to set all the other values.

The median wants to change the big number to 1 which takes (big number) ops.
```