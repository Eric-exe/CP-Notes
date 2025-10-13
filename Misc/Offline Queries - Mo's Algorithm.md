Split each query into a block of $\sqrt{n}$ and sort the right bound of each block non-decreasing. The right pointer for each block does at most $n$ operations per block, and the left pointer does at most $\sqrt{n}$ operations per query.

There are $\sqrt{n}$ blocks, so the right pointer moves $n\sqrt{n}$ times.
There are $q$ queries, so the left pointer moves $q\sqrt{n}$ times.

Then, all the queries will be answered in $O(q\sqrt{n} + n\sqrt{n})$