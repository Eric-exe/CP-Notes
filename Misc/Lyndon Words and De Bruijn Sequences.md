## 1. Lyndon Words

A **Lyndon word** over an alphabet is a string that is:

1. **Strictly smaller than all its nontrivial rotations** (lexicographically)  
2. **Primitive**: cannot be written as a repetition of a smaller word

### Observed Pattern for Binary Lyndon Words

A known pattern for **binary Lyndon words** is:
```
0, 1, 
01, 
001, 011, 
0001, 0011, 0111, 
00001, 00011, 00111, 01111, ...
```

- This is **not a strict formula**, but an observed pattern from generating binary Lyndon words.  
- **Characteristics:**
  - Start with **small digits first** (0, then 1)
  - Longer words often have **leading zeros followed by ones**
  - Words appear in **lexicographic order**
  - They are **primitive** and minimal among rotations

### Examples by Length (Binary)

- Length 1: `0, 1`  
- Length 2: `01`  
- Length 3: `001, 011`  
- Length 4: `0001, 0011, 0111`  
- Length 5: `00001, 00011, 00111, 01111`

### Notes

- Lyndon words can be **generated efficiently** using **Duval’s algorithm**.  
- They are crucial for **constructing the lexicographically smallest De Bruijn sequences**.

---

## 2. De Bruijn Sequences

A **De Bruijn sequence** \(B(k, n)\) is a cyclic sequence of length \(k^n\) over an alphabet of size \(k\) such that **every possible substring of length \(n\) appears exactly once**.

### Properties

- **Length**: \(k^n\)  
- **Cyclic**: wrap-around counts for substrings  
- **Uniqueness**: each n-length string appears exactly once  

### Lexicographically Smallest Construction Using Lyndon Words

To construct the **lexicographically smallest De Bruijn sequence**:

1. Generate all **Lyndon words** over the alphabet whose **length divides n**.  
2. Sort them in **lexicographic order**.  
3. **Concatenate** them.  

#### Example: Binary \(B(2,3)\)

- Lyndon words of length dividing 3: `0, 001, 011, 1`  
- Concatenate: `0 + 001 + 011 + 1 = 00010111`  
- Sequence length = \(2^3 = 8\), and contains all 3-bit substrings exactly once.

---

### Summary

- **Lyndon words**: primitive, minimal in rotations, appear in lexicographic order, follow patterns like `0, 1, 01, 001, 011, ...`  
- **De Bruijn sequences**: sequences covering all n-length strings, can be constructed efficiently by concatenating Lyndon words whose lengths divide n  
