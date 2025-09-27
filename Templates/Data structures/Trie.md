```cpp
class Node {
public:
	// unordered_map<int, Node*> children;
	vector<Node*> children;
	bool end = false;
	Node() {
		children.resize(26, nullptr);
	}
};

void trieInsert(Node* node, string& s) {
	for (auto& c : s) {
		if (node->children[c - 'a'] == nullptr) node->children[c - 'a'] = new Node();
		node = node->children[c - 'a'];
	}
	node->end = true;
}
```