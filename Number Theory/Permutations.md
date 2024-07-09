### Distinct
> Order matters.

Permutations assuming every element is unique:
$$n!$$
where $n$ is the number of elements.

---
### Non-Distinct
If $n$ doesn't contain distinct elements, then:
If there are $n$ elements in a set and $r1$​​ are alike, $r2$ are alike, $r3$​​ are alike, and so on through $rk$​​, the number of permutations can be found by:
$$\frac{n!}{r1!\cdot r2! \cdot r3! \cdot ... \cdot rk!}$$
Example: How many ways can I rearrange 5 $x$ and 3 $y$? Keep in mind that switching an $x$ with another $x$ doesn't result in a new permutation.
Solution:
$$\frac{(5+3)!}{5!\cdot 3!}$$

---
### Inversions
Inversions: pairs $i, j$ where $a[i] > a[j]$ but $i < j$.
This value can be counted by using a merge sort.

Fact:
* The number of inversions is the minimum number of swaps needed to sort the permutation if we can only swap adjacent elements.
* Swapping two elements in a permutation always changes the parity of the inversions in the permutation.
	* Rough proof (write it out): $a < b < c$ has 0 inversions. But if we swap $a$ and $c$, ($c,a,b$), inversions between $a$ and $c$ adds/removes 0 or +2 per element. 0 is 
