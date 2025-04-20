There are balls of $n$ different colors; the number of balls of the $i$-th color is $a_i$.

The balls can be combined into groups. Each group should contain at most $2$ balls, and no more than $1$ ball of each color.

What's the minimum number of groups required?

$$max(\lceil{\frac{total}{2}}\rceil, mx)$$
If the max frequency is greater than the sum of all the others balls, it only makes sense to pair all other ball colors to the max frequency. 

However, if the max frequency is smaller than the sum of all the other ball colors, we can continuously pair every other color with another color not the most frequent color until the total is less than or the sum of all the other ball colors. Then, we can pair the result of the colors with the color with max frequency. The total pairs will be $\lceil{\frac{total}{2}}\rceil$ as every element except potentially 1 is paired.

### Another Intuition
If the biggest element is $\leq$ half of the pile size, (when $mx \leq sum-mx$) we can mark each individual element (say $i$) and pair it with $i + \lfloor\frac{sz}{2}\rfloor$ since it's always going to be in a different group. Thus, every ball in the first half can be paired with another ball in the same half. Only edge case is if there is an odd number of balls.