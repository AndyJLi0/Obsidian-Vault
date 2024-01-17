**Essence of this chapter:** How do we calculate probability for equally likely events?

Denote subset of sample space $\Omega$ by $A$. 
$$P(A) = \frac{\text{number of elements of }A} {\text{number of elements in 
}  \Omega}$$
**Basic Principle**:
- step 1 has $n_1$ possible outcomes
- step 2 has $n_2$ possible outcomes.
then: $$\text{total number of outcomes}  = n_1 \times  n_2$$
(assuming the outcome of the first event does not effect the outcome of the second event)
# Permutations

**[[Permutation]]**: A permutation of a set is an arrangement of its elements in a linear order.
Consider the set $A = \{1, 2, 3, 4, 5\}$
Permutations include: $( 5, 4, 3, 2, 1), ( 2, 3, 3, 5,1)$, etc.

*Counting Permutations:* Lets say size of $A$ is $n$. How many different permutations of $A$ are there?
- $n$ choices for first...
- $n -1$ choices for second...
- total is therefore (for distinct elements)
- $$\text{different permutations} = n!$$

*What if there aren't distinct elements*?
- Lets say we have **10 players**, 4 Russian, 3 USA, 2 Argentina, 1 Brazil. How many permutations are there now?
- If they were distinct, it would be $10!$ However We have $4$ different ways to permute the same nationality Russia, 3 for USA, etc.
- Therefore, our answer would be $$ \text{number of arrangements} = \frac{10!}{4!3!2!1!} = 12600$$

*Outcome an Argentinian player is ranked first?*
- there are 2 Argentinian, so 2 ways for them to be ranked first
- there are $9!$ more ways of arrange the rest of the players. However we only see nationalities! 
-  $$\text{\# of favourable outcomes} = \frac{2 \times 9!}{4!3!2!}$$
- We divide our favourable outcome by the total possible outcomes to get the probability.
- ***Simpler approach:*** we just see that there are $10$ players, $2$ are Argentinian, each have equal chance of winning, so probability would be $\tfrac {2}{10}$
