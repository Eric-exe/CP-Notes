Purpose: Similar to a set but we can access it like an array.

```cpp
#include <ext/pb_ds/assoc_container.hpp>
using namespace __gnu_pbds;

typedef tree<int,null_type,less<int>,rb_tree_tag,
tree_order_statistics_node_update> indexed_set;
```

---
### Usage
```cpp
indexed_set s;
s.insert(2);
s.insert(3);
s.insert(7);
s.insert(9);
```
#### `find_by_order(int)`
Returns an iterator to the given element.
Example:
```cpp
auto it = s.find_by_order(7);
cout << *it << endl; // 2
```
Time Complexity: O($\log n$)
#### `order_of_key(int)`
Returns the index of the given element. If the element does not appear in the set, it returns the index of where it would have been
Example:
```cpp
cout << s.order_of_key(7) << endl; // 2

cout << s.order_of_key(6) << endl; // 2
cout << s.order_of_key(8) << endl; // 3
```
Time Complexity: O($\log n$)
