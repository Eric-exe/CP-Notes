[Shortest Path Modelling Tutorial - Codeforces](https://codeforces.com/blog/entry/45897)

Intuition: Say we want to find the shortest path between two cities. However, there is a fuel requirement and each city sells fuels at $x$ price a unit. Traditional Dijkstra's will NOT work here. Instead, create finite state machines (FSM) storing the state of a node. In this example, the states will be city ID and the current amount of fuel.

Example: [Problem - G - Codeforces](https://codeforces.com/contest/1915/problem/G): we can store the fastest bike
