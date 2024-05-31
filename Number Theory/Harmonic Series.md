Suppose you need 
$$n + \frac{n}{2}+\frac{n}{3}+ \frac{n}{4} +...$$
operations. This is equivalent to:
$$n(1+\frac{1}{2}+\frac{1}{3}+\frac{1}{4}+.. +\frac{1}{n})$$
Time complexity wise:
$$\sum_{i=1}^{n}\frac{1}{i}= \log(n)$$
so calculating harmonic series over $n$ is $O(n\log n)$. 

---
### Problems
[F - We Were Both Children (Codeforces)](https://codeforces.com/contest/1850/problem/F)
[Find the Number of Good Pairs II (LeetCode)](https://leetcode.com/problems/find-the-number-of-good-pairs-ii/description/)
