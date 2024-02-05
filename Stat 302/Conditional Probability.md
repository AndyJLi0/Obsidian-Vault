Instead of checking *favourable outcomes* out of all the sample space, we will check the *favourable outcomes* in our *conditional event*. Let $A, B \subset \Omega$ be two events with $P(B) >0$. The **conditional probability** of $A$ given $B$ is:
$$ P(A | B) = \frac{(P(A\cap B)} {P(B)} $$
#### Multiplicative Property
From this, we can define a probability function $Q(\cdot) = P(\cdot | B)$ defined on the collection of all events $B$. This function of course satisfies the 3 axioms of probability theory [[Set Operations, Axioms, and Formulas#*Axioms*]]. From conditional probability, we have the important **multiplicative property:**
$$ P(A_1) >0, \text{then: \hspace{1em}}P(A_1 \cap A_2) =  \frac{P(A_1 \cap A_2)}{P(A_1)} P(A_1) = P(A_2| A_1) P(A_1)$$


#### Total Probability Formula
From conditional probability, we also get the following **Total Probability Formula:**
- Let $B_1, \ldots, B_n$ be a partition of $\Omega$. They must be disjoint, and cover the entire sample space
	- For any $A \in \Omega$, we have $$P(A) = \sum_{i=1}^n P(A|B_i)P(B_i)$$
#### Bayes Formula
Continuing on from the total probability formula, suppose we have the same partition $B_1 , \ldots, B_n$. Let $A$ be an event with non-zero probability. For each $i = 1, \ldots, n$ we have:
$$P(B_i|A) = \frac{P(A|B_i)P(B_i)}{\sum_{j=1}^n P(A|B_j)P(B_j)} = \frac{P(A|B_i)P(B_i)}{P(A)}.$$
The numerator is basically the region $A\cap B_i$, and the denominator is the total probability of $A$. This is just a way of writing the conditional probability formula.

### Prior and Posterior Probabilities
- In many applications, we have prior knowledge about the chances of $B_1, \ldots, B_n$.
	- We quantify the chances of these $P(B_1), \ldots, P(B_n)$ as **prior probabilities.**
- Either by experiment or observation, we find a relevant event $A$ has occurred
	- We want to now instead of basing the probability of $B_i$ on $\Omega$, we want them to update to be based on the probability of $A$.
	- The conditional probabilities $P(B_1|A), \ldots, P(B_n|A)$ are **posterior probabilities**
### Independence
- Events $A$ and $B$ are independent if 
$$ P(A\cap B) = P(A)P(B)$$
>[!info]
>Note that is also true that $P(A \cap B^c) = P(A)P(B^c)$ and $P(A^c \cap B) = P(A^c)P(B)$
- If $P(B) > 0$, independence implies that $$P(A|B) = P(A)$$
- Therefore when $A$ and $B$ are independent, the knowledge of $B$ occurring does not change the probability of $A$. This is biconditional! They are "independent events".
- **Theorem:** If $A$ and $B$ are independent, then so are $A^c$ and $B$. Explicitly:
$$P(A|B) = P(A) \implies P(A^c |B) = P(A^c)$$

#### Golden Standard
- When trying to prove independence, We always just want to show whether or not:
$$ P(A \cap B) = P(A)P(B)$$

### Conditional Independence

**Definition:** Events $T_1, T_2, \ldots, T_n$ are conditionally independent given event $B$ if $$P(T_{i1} T_{i2} \dots T_{ik} | B) = P(T_{i1}|B)P(T_{i2}|B) \dots P(T_{ik}|B)$$
>[!info]
>Note in additional the the above note, is also true that conditional independence of $T_1 T_2$ given $B$, we have that: $P(T_1^c T_2 | B) = P(T_1^c| B) P(T_2| B)$ and vice versa with $T_1 , T_2^c$.
- Conditional independence doesn't imply unconditional independence and vice versa.
- Conditional independence given $B$ doesn't imply conditional independence given $B^c$.