*A combination of size $m$ is a subset of $m$ items taken from a larger set with $n \geq m$ items*

- the order of the elements does not matter (a set is not the same as a sequence).

#### Notations and Formula
- the number of combinations of size $m$ out of the set is a size $n \geq m$ is denoted by: $$ _nC_m = {n\choose m} = C^n _m = \frac{n!}{m!(n-m)!}$$
	- $m$: is the size of the combinations
	- $n$: size of the set from which combinations are drawn from.
	- *e.g.* set of size $n= 5$, we can make ${5 \choose 3} = 10$ different combinations of size 3.

#### Connecting [[Permutations]] and Combinations
- How many distinct spellings can we get from rearranging the letters "mamma"?
	- using **Permutations**, we have 2 dupes of *a*, and 3 dupes of *m*. so we get $\frac{5!}{2!3!}$
	- using **Combinations**, we can think of it as a set of $n = 5$, and we want to choose $m = 2$ of them from the set, so ${ 5 \choose 2} = \frac{5!}{2!3!}$ . We get the same value!
		- We treat the letters as distinct: $\{m_1, m_2, m_3, a_1, a_2\}$. We 
#### Proof of Correctness
- Take a permutation of the $n$ items in the set
- Keep the first $m$ terms of the permutation, and discard the remaining $(n-m)$ terms.
- Turns out since order doesn't matter for combinations, **permutations sharing the same first $m$ values and last $(n-m)$ gives the same combinations**.
	- Each **permutation** generates a **combination**, with many permutations generating the same.
- For a specific permutation of length $n$, we have $m!$ ways to re-order its first $m$ elements, and $(n-m)!$ ways to re-order its last $n-m$ elements. Thus the total number of permutations giving the same combination is $$m! \times (n-m)!$$ Another way of thinking about it" each combination corresponds to $m! \times (n-m)!$ permutations.
- Since there are $n!$ distinct length-$n$ permutations, therefore **number of combinations** is $$\frac{\text{\# of distinct permutations}}{\text{\# of permutations giving duplicate combinations}} = \frac{n!}{m!\times (n - m)!} = {n \choose m}$$
#### Examples
> Player chooses 6 integers b/w 0 and 49. Dealer randomly selects 6 integers b/w 0 and 49.Prize depends on the # of matches between them. What is the probability that the number of matches is $k$?
- **Solution:** think of a box of 50 balls numbered 0 to 49. we randomly paint 6 balls red. Dealer picks 6 balls at random. How many red balls in dealer's draw is the number of matches between dealer and player.
	-  The number of possible outcomes of dealer's draw is $\# \Omega = { 50 \choose 6}$.
	- Number of **favourable dealer's draw is** $$\# D_k = {6 \choose k} \times {44 \choose 6-k}$$
	- Therefore probability of the number of matches is $k$ equals: $$ P(A_k) = \frac{\#D_k}{\# \Omega}$$