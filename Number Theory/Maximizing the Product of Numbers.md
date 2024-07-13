We have a number $n$. We need to break it down in a way such that $x+y=n$ but $x \cdot y$ is maximized.
To do this, $x$ and $y$ has to be as close as possible.

---
What about $a_1 + a_2 + a_3 + ... + a_i = n$, with the same goal of maximizing the product? To do this, we need to make the numbers as close to each other as possible. What number however, if we can basically break them down all into 1s? 

We want to break down the number into as many 3s as possible with the remainder being broken down into 2s. That is to say:
$$3 + 3 + 3 + 1 \rightarrow 3 + 3 + 2 + 2$$
This will maximize the product if the sum of the values add up to $n$. 
### Proof
$n\rightarrow$ number given.
$x\rightarrow$ the number for the number $n$ to be broken down into such that the product of the numbers are maximized. Graph:
$$f(x)=x^{\frac{n}{x}}$$
Notice that it is maximized at $e$ or 2.718. 3 is closer to $e$ than 2 so it is optimal to break the numbers into as many 3s as possible.

If after 3s, we have a 1, its better to to break it into a values x 2 x 2 instead of a values 3 x 1.