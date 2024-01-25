*The first topic for CPSC 320*
## The problem
- We have $n$ applicants and $n$ employers. Each has a full preference list of the other. How can we match the applicants with employers such the matching is stable?
	- For a stable matching, there must be no unstable matches. An unstable match is when applicant $a$ and employer $e$ are not matched with each other, but both prefer each other over their current match.
- Brute forcing this problem wouldn't work. We have $n!$ possible list of matchings and we need someway to check if it is stable or not.
#### Gale-Shapley Algorithm
*here is the pseudo code for the G-S algorithm. It is guaranteed that there is always a stable matching.*
```python
function stableMatching(A, E)
    Initialize all a ∈ A and e ∈ E to free
    while ∃ some e who hasn't made an offer to every applicant do
       a = highest ranked applicant e hasn't offered
       if a is free
         hire(a, e) 
       else some pair (a, e') already exists
         if a prefers e to e'
	        unhire(a,e')
	        hire(a, e)
	    endif
    endwhile
endfunction
```

## Resident Hospital Problem
*A spin on the original SMP problem.*
> [!info] See [[Permutations]] for the brute force approach
- We will solve this by [[Reduction]]!
#### Small Examples
| hospitals | empty | residents |
| ---- | ---- | ---- |
| h1(1): r1 r2 r3 | - - - - | r1: h1 h2 |
| h2(2): r2 r1 r3 | - - - - | r2: h1 h2 |
|  | \ _ _ _  | r3: h1 h2 |
- Given this, we can treat each 'slot' of a hospital as a separate employer. we can keep h1 the same, but for h2 with 2 slots, we create 2 duplicate copies and thus we have 3 hospitals and 3 residents, each with 1 slot. This is a reduction to our original stable matching problem!
- Now our matching looks like:

|hospitals|empty|residents|
|---|---|---|
|h1: r1 r2 r3 |- - - - |r1: h1 h2|
|h2: r2 r1 r3 |- - - - |r2: h1 h2|
|h2: r2 r1 r3 |- - - - |r3: h1 h2|
- Now since we split up h2 into 2 separate copies, to change this instance of SMP back to RHP, we combine the h2 back into 1 hospital. Thus we have successfully completed our reduction.
#### Proof of Correctness
- Want to prove that if $B$'s solution is stable, $RHP$'s solution is stable, or the contrapositive: if $RHP$'s solution is unstable, then $B$'s solution is unstable.

$Proof$.
- Suppose for contradiction that $(h_j, r_j)$ is an instability for $M_a$. Then $r_j$ prefers $h_i$ for some $i$ over $h_j$. and $h_j$ prefers $r_i$ for some $i$ over *any* of its residents. 
- So, in our instance of $M_b$, it is true that $(h_j^k, r_j)$ is an instability as well since $r_j$ prefers not just another $h_j^l$, but some $h_i^k$ for some $k$ and some $i$ (the subscript is different). since $h_j \rightarrow h_j^k$ in $M_b$, then $h_j^k$ prefers a different resident than $r_j$ and therefore there is an instability in $M_b$. $\square$

