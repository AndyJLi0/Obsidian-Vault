A class of algorithmic techniques that divides the problem into smaller subproblems, solves each part recursively, and then combines the solutions to these subproblems into an overall solution.

### $T(\cdot) = 2T(n/2) + cn$
Examples include Merge sort. For these problems. We can give an asymptotic upper bound of $O(n\log(n))$. We have $\log(n)$ amount of levels in the tree, each level summing up to $n$, so we have a total of $n\log(n)$. This is intuitive.

### $T(\cdot) = qT(n/2) + cn$, $q > 2$
In this case, at each level, has a runtime greater than $n$ since we have more subproblems. The amount of levels is still $\log(n$). Any problems like these are bounded by $O(n^{\log_2(q)}$).

In the special case where $q = 1$, the problem is bounded by $O(n)$.

### $f(n)$
Let the $+ cn$ part be $f(n)$. The runtime of this would just be $O(n).$ If $f(n) = qn^2, then $f(n) \in O(n^2).$


## Master Theorem
Let a recurrence relation take the form $$T(n) = a T\left(\frac{n}{b}\right) = f(n)$$
- $n$ is the size of the input problem
- $b$ is the factor by which the subproblem is reduced by)
- $c_{crit} = \log_b(a) = \log(\text{\#subproblems}) / \log(\text{relative subproblem size})$

### Cases of Recurrence Relationship
#### Case 1
| Case | Description | Condition on $f(n)$ in relation to $c_{crit}$ | Master Theorem bound |
| ---- | ---- | ---- | ---- |
| 1 | Work to split/recombine a problem is dwarfed by subproblems.<br><br>i.e. the recursion tree is leaf-heavy | upper-bounded by a lesser exponent polynomial. When $f(n) = O(n^c)$, $c < c_{crit}$.  | Then $T(n) = \Theta(n^{c_{crit}})$. Splitting term does not appear; the recursive tree structure dominates. |
| 2 | Work to split/recombine a problem is comparable to subproblems. | rangebound by the critical-exponent polynomial. $f(n) = \Theta (n^{c_{crit}} \log^k(n))$ | Then $T(n) = \Theta(n^{c_{crit}}log^{k+1}n)$<br><br>(The bound is the splitting term, where the log is augmented by a single power.) |
| 3 | Work to split/recombine a problem dominates subproblems.<br><br>i.e. the recursion tree is root-heavy. | $f(n) = \Omega(n^c)$, $c > c_{crit}$. <br><br>(lower-bounded by a greater-exponent polynomial) | If $af\left( \frac{n}{b}\right) \leq kf(n)$ for some constant $k < 1$, and sufficiently large $n$ (**regularity condition**), then $T(n) = \Theta (f(n))$. |