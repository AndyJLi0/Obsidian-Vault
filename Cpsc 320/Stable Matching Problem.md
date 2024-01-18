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
- We will solve this by [[Reduction]]!