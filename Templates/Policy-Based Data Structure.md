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
cout << *it << endl; // 9
it = s.find_by_order(1);
cout << *it << endl; // 3 
it = s.find_by_order(0);
cout << *it << endl; // 2
```
Time Complexity: O($\log n$)
##### `order_of_key(int)`
Returns the index of the given element. If the element does not appear in the set, it returns the index of where it would have been.
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
### Indexed Multiset using Indexed Set
```cpp
#include <ext/pb_ds/assoc_container.hpp>
#include <ext/pb_ds/tree_policy.hpp>
using namespace __gnu_pbds;

template <typename T> class IndexedMultiset {
public:
    IndexedMultiset() : id(0) {}
    void insert(T x) { ms.insert({x, ++id}); }
    void erase(T x) { auto it = ms.lower_bound({x, 0}); if (it != ms.end() && it->first == x) ms.erase(it); }
    T find_by_order(int k) { return (k < 0 || k >= ms.size()) ? throw out_of_range("out of range") : ms.find_by_order(k)->first; }
    int order_of_key(T x) { return ms.order_of_key({x, 0}); }
    int lower_bound(T x) { return order_of_key(x); }
    int upper_bound(T x) { return order_of_key(x + 1); }
    bool empty() { return ms.empty(); }
    int size() { return ms.size(); }
    void clear() { ms.clear(); id = 0; }
    friend ostream& operator<<(ostream& os, const IndexedMultiset& m) { for (const auto& item : m.ms) os << item.first << " "; return os; }
private:
    tree<pair<T, int>, null_type, less<pair<T, int>>, rb_tree_tag, tree_order_statistics_node_update> ms;
    int id = 0;         
};
```

### Faster than Unordered map
```cpp
struct chash {
	const uint64_t C = uint64_t(2e18 * PI) + 71;
	const uint32_t RANDOM = chrono::steady_clock::now().time_since_epoch().count();
	size_t operator()(uint64_t x) const {
		return __builtin_bswap64((x ^ RANDOM) * C);
	}
};

#include <ext/pb_ds/assoc_container.hpp>
using namespace __gnu_pbds;

gp_hash_table<int, int, chash> f;
```

Usage is the same as a map.
