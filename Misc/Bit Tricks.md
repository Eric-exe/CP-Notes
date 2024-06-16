### Array Addition
Intuition:
If we want to add $x$ to an array of elements in faster than $n$ time where $n$ is the number of , we can use bit manipulation. For example, if we have an array [1, 4, 5, 7] and we want to add 2 to every element, we can represent the array as:
```
Positions: 9876543210
Bitset:    0010110010
Addition:  1011001000
```

A bit shift does 64 bits at once so the time it will take will be $\frac{n}{64}$. 

In C++, we can use a bitset to hold arrays with elements of size more than 64. If we want to remove bits, we can either shift bits and undo the shifts so that the bits are reset to 0 or use AND (though this might not work when we try to create a number bigger than a long long).

Usage:
```cpp
bitset<100000> bits;
bits |= 1;
bits <<= 2;
bits >>= 3;
```
