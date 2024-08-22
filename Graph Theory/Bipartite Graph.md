A graph in graph theory where the vertices can be divided into two disjoint sets, and every edge connects a vertex in one set to a vertex in the other set.

Properties:
* Contains **NO** odd cycles

Bipartite Check
Choose any vertex and include it in one set (can be marked as $0$ or $1$). Adjacent vertices are forced to go to the other set. If an adjacent vertex is in the same set, the graph is not bipartite.