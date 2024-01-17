#### Here are some set operations and formulas, useful for probability. See more about counting on [[Basics of Counting and Permutations]]

**Properties**: commutative, associative, distributive.

*Operations on sets*
- $A \cup B: \omega \in A \cup B \implies \omega \in A \lor \omega \in B$
-  $A \cap B: \omega \in A \cup B \implies \omega \in A \land \omega \in B$
- $A \Delta B: \omega \in A \Delta B \implies \omega \in (A \cap B^c) \cup (A^c \cap B)$

*Axioms*
- $P(\Omega) = 1$
- $\forall A \in B, P(A) \geq 0$
- if $\{A_n\}_{n\geq 1}$ are a sequence of disjoint events, $P(A_1 \cup \ldots \cup A_n) = \sum_{n=1}^{\infty} P(A_n)$


*Useful formulas*
- $(A \cap B)^c \iff A^c \cup B^c$
- $\# (2^{\Omega}) = 2^{\#\Omega}$
- $P(A^c) = 1 - P(A)$
- $A \subset B \implies P(A) \leq P(B)$
- $P(A \cup B) = P(A) + P(B) - P(A \cap B)$
