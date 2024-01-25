*Some quick examples on permutations with (and without) duplicates*

#### Example 1
- Using the word "*Agnus*", there are $5!$ ways to permute this
- Using the word "*Virgil*", there are 6! ways to arrange these letters, but some are *duplicates*
	- Since there are two *i*'s, we will divide our total arrangement count by $2!$ to get $6! / 2!$

#### Slightly harder
- We have the word "Mississippi". 11 letters total, but we need to account for the duplicates
	- 4 ways to arrange the *i's* without changing the word. *e.g* we have permutation "msssssppiiii", but for the *i's*, there are $4!$ ways to arrange them.
	- 4 ways to arrange the *s's* without changing the word.
	- 2 *p's*
- Our total amount of permutations would be : $\frac{11!}{4! \times 4! \times 2!}$
#### Resident Hospital Problem
- For the original *[[Stable Matching Problem]]*, we had a total of $n!$ different matchings with $n$ being the number of applicants/employers
- For RHP, $S_h$, the number of slots available for hospital $h$, is basically a way to store duplicates. If we have 2 slots open for hospital $h$, $r_1$ and $r_2$ have 2 ways of arranging into those slots, but they are considered the 'same' matching. Therefore, the number of different matchings would be: $$\text{\# of matchings for RHP} = \frac{n!}{S_1! \times S_2! \times \ldots \S_m !}$$