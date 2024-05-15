We have a number $n$. We need to break it down in a way such that $x+y=n$ but $x \cdot y$ is maximized.
To do this, $x$ and $y$ has to be as close as possible.

---
What about $a_1 + a_2 + a_3 + ... + a_i = n$, with the same goal of maximizing the product. To do this, we need to make the numbers as close to each other as possible. What number however, if we can basically break them down all into 1s? 

We want to break down the number into as many 3s as possible with the remainder being broken down into 2s. That is to say:
$$ 3 + 3 + 3 + 1 \rightarrow 3 + 3 + 2 + 2 $$
This will maximize the product if the sum of the values add up to $n$.
### Proof
$n\rightarrow$ number given.
$x\rightarrow$ the number for the number $n$ to be broken down into such that the product of the numbers are maximized. Solve for:
$$ \frac{d}{dx}x^{\frac{n}{x}} $$
and graph it.