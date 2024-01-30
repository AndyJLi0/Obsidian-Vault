 Here are some set operations and formulas, useful for probability. See more about counting on [[Basics of Counting and Permutations]]

**Properties**: commutative, associative, distributive.

#### *Operations on sets*
- $A \cup B: \omega \in A \cup B \implies \omega \in A \lor \omega \in B$
-  $A \cap B: \omega \in A \cup B \implies \omega \in A \land \omega \in B$
- $A \Delta B: \omega \in A \Delta B \implies \omega \in (A \cap B^c) \cup (A^c \cap B)$

#### *Axioms*
- $P(\Omega) = 1$
- $\forall A \in B, P(A) \geq 0$
- if $\{A_n\}_{n\geq 1}$ are a sequence of disjoint events, $P(A_1 \cup \ldots \cup A_n) = \sum_{n=1}^{\infty} P(A_n)$


#### *Useful formulas*
- $(A \cap B)^c \iff A^c \cup B^c$
- $\# (2^{\Omega}) = 2^{\#\Omega}$
- $P(A^c) = 1 - P(A)$
- $A \subset B \implies P(A) \leq P(B)$
- $P(A \cup B) = P(A) + P(B) - P(A \cap B)$
- $P(A\\B) = P(A) - P(B)



#### How to break down a Union
- for $n= 2$, we have : $$P (A_1 ∪ A_2) = P (A_1) + P (A_2) − P (A_1 ∩ A_2)$$
- for $n = 3$: $$P (A_1 ∪ A_2 ∪ A_3) = P (A_1) + P (A_2) + P (A_3)$$$$- P (A_1 ∩ A_2) − P (A_1 ∩ A_3) − P (A_2 ∩ A_3) + P (A_1 ∩ A_2 ∩ A_3)$$
- Alternate between inclusion and exclusion!
- For a special case where the probability is all the same for intersecting $n$ subsets, we have $$p_n = P(A_1\cap A_2 \cap \dots A_n)$$
	- We then have for our special case (look at [[Combinations]])$$P (A_1 ∪ A_2 ∪ \dots ∪ A_n) = \sum_{k=1}^n(-1)^{k-1}{n\choose k} p_k$$
#### Example
>One version of the story: n couples attend a party. At one moment, each lady randomly picks a gentleman to dance. What is the probability that none of them pick their partner?

*Denote the event*
- $A_i$ = the $i$th lady dances with her partner. for $i = 1, 2, \ldots$.
- event of interest is $\{A_1 A_2\ldots A_n\}^c$ = $A$. Therefore we need to find $P(A)$.
- We isolate event $A_1$, it is clear that $P(A_1) =1/n$. 
- For event $A_1\cap A_2$, we have $P(A_1 \cap A_2) = \frac{1}{n(n-1)}$.
- For $P(A_1A_2 \ldots A_k) = \frac{1}{k!} = \frac{(n-k)!}{n!}$.
- We want to find the union, and we know the formula for the intersect probability, so we can use the inclusion-exclusion formula:
	- $$P(A_1 \cup A_2 \cup \dots \cup A_n) = \sum_{k=1}^n(-1)^{k-1}{n\choose k} \frac{(n-k)!}{n!} =\sum_{k=1}^n(-1)^{k-1} \frac{1}{k!} $$
	- We can use the complement formula to get $$1 - P(A_1 \cup A_2 \cup \dots \cup A_n) = \sum_{k=1}^n(-1)^k \frac{1}{k!} $$
	- As $n \rightarrow \infty$  we have our probability equal to $\frac{1}{e}$.