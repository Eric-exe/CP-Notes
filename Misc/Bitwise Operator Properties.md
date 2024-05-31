```
=> a & (a | b) = a

=> a | (a & b) = a

=> a & (b | c) = (a & b) | (a & c)
	=> a & (b | c | d | ... | z) = (a & b) | (a & c) | (a & d) | ... | (a & z)
	
=> a & (b ^ c) = (a & b) ^ (a & c)
    => a & (b ^ c ^ d ^ ... ^ z) = (a & b) ^ (a & c) ^ (a & d) ^ ... ^ (a & z)
    
=> a | (b & c) = (a | b) & (a | c)

=> ~(a & b) = ~a | ~b

=> ~(a | b) = ~a & ~b
```
---
### XOR Operations
If $x_1$ ^ $x_2$ ^ $x_3$ ^ ... ^ $x_k$ = 0, then we can ALWAYS split the array into two segments with at least one element in each segment such that the two segments are equal to each other when XORed by themselves.