### Problem
Given an array of $n$ positive integers, we can decrement the array using one of these operations every time:
- Decrement 1 element
- Decrement 2 elements (you cannot choose the same element)

What is the minimum number of decrements to the all the integers to 0?

### Solution
Sort the array. Notice that the last element is the biggest and as such, we can apply operation 2 on this element and any other element. Now, two case arises:
#### Case 1
The last element is greater than or equal to the sum of the rest of the elements. That means that every element can be decremented and the biggest element can still have something left. Denote $mx$ as the biggest element and $left$ as the sum of all other elements. 
$$ops = left + (mx-left)$$
This is because we can decrement all of $left$ and we may still have $mx-left$ leftover.

#### Case 2
The last element is smaller than the sum of the rest of the elements. That means we can decrement all of $mx$ and still have $left$ leftover. That means:
$$ops = mx + \left \lfloor{\frac{left - mx}{2}}\right \rfloor + (left - mx) \% 2$$
or, if we have another variable $sum=mx+left$, then
$$ops = \left \lfloor{\frac{sum}{2}}\right \rfloor + sum \% 2$$
because we can pair everything. The only thing we need to check is the parity.