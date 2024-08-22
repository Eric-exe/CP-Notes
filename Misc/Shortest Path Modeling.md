[Shortest Path Modelling Tutorial - Codeforces](https://codeforces.com/blog/entry/45897)

Intuition: Say we want to find the shortest path between two cities. However, there is a fuel requirement and each city sells fuels at $x$ price a unit. Traditional Dijkstra's will NOT work here. Instead, create finite state machines (FSM) storing the state of a node. In this example, the states will be city ID and the current amount of fuel.

To solve:
Create a graph of states (similar to a DP memo table) and run standard Dijkstra, handling state changes (again, like DP). In this example, it will be a 2d array of `[city][fuel]`.

The answer from the origin to the end node one of `(end node, state1, state2,...)` usually the minimum.

Example: [Problem - G - Codeforces](https://codeforces.com/contest/1915/problem/G): we can store the fastest bike.
