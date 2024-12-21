```cpp
int BITS = 31;

class BitTrie {
public:
    BitTrie* child[2];
    int num = -1;
    BitTrie() { child[0] = child[1] = NULL; };

    void insert(int num) {
        BitTrie* node = this;
        for (int i = BITS; i >= 0; i--) {
            bool bit = num & (1 << i);
            if (!node->child[bit]) node->child[bit] = new BitTrie();
            node = node->child[bit];
        }
        node->num = num;
    }

    void remove(int num) {
        BitTrie* node = this;
        for (int i = BITS; i >= 0; i--) {
            bool bit = num & (1 << i);
            node = node->child[bit];
        }
        node->num = -1;
    }

    int getMaxXor(int num) {
        BitTrie* node = this;
        int res = -1;
        for (int i = BITS; i >= 0 && node; i--) {
            bool bit = num & (1 << i);
            if (node->child[1 - bit]) node = node->child[1 - bit];
            else node = node->child[bit];
            if (node && node->num != -1) res = max(res, num ^ node->num);
        }
        return res;
    }

    int getMinXor(int num) {
        BitTrie* node = this;
        int res = -1;
        for (int i = BITS; i >= 0 && node; i--) {
            bool bit = num & (1 << i);
            if (node->child[bit]) node = node->child[bit];
            else node = node->child[1 - bit];
            if (node && node->num != -1) res = max(res, num ^ node->num);
        }
        return res;
    }
};

```