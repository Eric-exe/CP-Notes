Sometimes, matrices can be used, especially if a state is really big. 

One such example is: [Problem - E2 - Codeforces](https://codeforces.com/contest/1970/problem/E2)

The DP transitions can be written as:
$$
\text{DP}(\text{cabin}, \text{day}) = 
\left\{
\begin{aligned}
    &\text{1} && \text{if day > n (one possible path)} \\
    &\sum_{\text{new cabin}=1}^{m}ways(\text{cabin}, \text{new cabin}) &&\text{otherwise}\\
\end{aligned}
\right.
$$
where $ways(i, j)$ is the number of possible ways to get from cabin $i$ to cabin $j$ using at least one short trail. This can easily be calculated by summing up all the combinations: $$short_i \cdot short_j + short_i \cdot long_j + long_i \cdot short_j$$where $short$ and $long$ are the short and long paths from their cabins to the lake, respectively.


Normally, we can store the results in a DP table with the states $\text{cabin} \cdot \text{day}$. However, this is infeasible as $n \leq 10^9$.

Instead, we can represent the DP transitions as a matrix table and just store the cabin as a state, with the value stored in the state being the number of ways to get to that cabin. This is stored in a vector of size $1 \times m$. Because we always start at the first cabin, we can represent the initial state as:
$$
\vec{v_1}=\begin{bmatrix}
1 & 0 & 0 & \dots & 0
\end{bmatrix}
$$

Now, to handle the transition:
How does the number of ways change after each day for a certain cabin? It is the sum of all the ways of the cabins in the previous day multiplied by the number of ways from those cabins to the current cabin. As such, the DP transition matrix is:
$$\textbf{M}=
\begin{bmatrix}
ways(1, 1) & ways(1, 2) & ways(1, 3) & \cdots & ways(1, m) \\
ways(2, 1) & ways(2, 2) & ways(2, 3) & \cdots & ways(2, m) \\
ways(3, 1) & ways(3, 2) & ways(3, 3) & \cdots & ways(2, m) \\
\vdots            & \vdots            & \vdots            & \ddots & \vdots \\
ways(m, 1) & ways(m, 2) & ways(m, 3) & \cdots & ways(m, m)
\end{bmatrix}
$$

Validity check: This will output a vector of size $1\times m$ since $\vec{v}\textbf{M}=(1\times m)\times (m\times m)=1\times m$. This will be the number of ways after a day has passed.

Why does this work?
Consider a specific cabin$\textemdash$say, cabin 1. After one day, the new state vector is given by:
$$
\vec{v}_{t+1} = \vec{v}_t \, \textbf{M}
$$

To compute the value at $(\vec{v}_{t+1})_1$, we look at how each cabin on day $t$ contributes to cabin 1 on day $t+1$:
$$
\begin{align}
(\vec{v}_{t+1})_1 &= (\vec{v}_t)_1 \cdot \text{ways}(1, 1) + (\vec{v}_t)_2 \cdot \text{ways}(2, 1) + \dots + (\vec{v}_t)_m \cdot \text{ways}(m, 1) \\
&= \sum_{i=1}^{m} (\vec{v}_t)_i \cdot \text{ways}(i, 1)
\end{align}
$$

In other words, the number of ways to reach cabin 1 on day $t+1$ is the sum over all cabins $i$ of:
* the number of ways to be at cabin $i$ on day $t$,
* multiplied by the number of ways to go from cabin $i$ to cabin 1 in one step.

To solve the problem for $n$ then, we have to sum of the values at $\vec{v_n}$. Thus, we need to evaluate:
$$\vec{v_1}\textbf{M}\textbf{M}\textbf{M}\dots$$
where $\vec{v_1}$ is multiplied by $\textbf{M}$ $n$ times, or:
$$\vec{v_1}\textbf{M}^n$$

Now, to speed up this calculation, we can use [[Fast Pow (Binary Exponentiation)]] trick on the matrix $\textbf{M}$ and we can calculate the results from there! 