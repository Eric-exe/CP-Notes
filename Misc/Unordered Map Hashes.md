https://codeforces.com/blog/entry/21853

```cpp
/* https://codeforces.com/blog/entry/21853 */
struct HASH{
  size_t operator()(const pair<int,int>&x)const{
    return hash<long long>()(((long long)x.first)^(((long long)x.second)<<32));
  }
};
unordered_map<pair<int,int>,int,HASH>mp;
```