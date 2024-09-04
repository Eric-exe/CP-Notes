Purpose: Similar to a set but we can access it like an array.

```cpp
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
using namespace __gnu_pbds;

typedef tree<int,null_type,less<int>,rb_tree_tag,
tree_order_statistics_node_update> indexed_set;

typedef tree<int,null_type,less_equal<int>,rb_tree_tag,
tree_order_statistics_node_update> indexed_multiset;
```

---
### Usage
#### Indexed Set
```cpp
indexed_set s;
s.insert(2);
s.insert(3);
s.insert(7);
s.insert(9);
```
##### `find_by_order(int)`
Returns an iterator to the given element.
Example:
```cpp
auto it = s.find_by_order(3);
cout << *it << endl; // 2
it = s.find_by_order(7);
cout << *it << endl; // 3
```
Time Complexity: O($\log n$)
##### `order_of_key(int)`
Returns the index of the given element. If the element does not appear in the set, it returns the index of where it would have been
Example:
```cpp
cout << s.order_of_key(7) << endl; // 2

cout << s.order_of_key(6) << endl; // 2
cout << s.order_of_key(8) << endl; // 3
```
Time Complexity: O($\log n$)

#### Indexed Multiset
```cpp
indexed_multiset s;
s.insert(5);
```
##### `s.erase(s.find_by_order(s.order_of_key(x)));`
Erase one occurrence of $x$.

##### `s.order_of_key(x);`
Number of elements strictly less than $x$.

