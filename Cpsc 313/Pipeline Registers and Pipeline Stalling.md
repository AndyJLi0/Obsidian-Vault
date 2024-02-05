#pipeline #cpsc313
### Pipeline Registers
In the pipeline implementation of the y86 processor, we have a series of pipeline registers that can hold basically the entire instruction (if it wanted to) in the middle of every stage. Like a latch, it saves the signals from the previous clock cycle and releases it when it takes in another. As we move through stages, we only need to pass through certain key signals that can keep each stage knowing what to execute:
![[y86 rearranged.png]]

### Pipeline Stalling 
A problem that we need to address when trying to implement a pipeline processor is the fact that information in registers/ memory may not be available to us when the instruction is executed because the it relies on information altered by the previous or close to previous instruction. If we have two **ADDQ** instructions back to back and the second one uses the register from the first, we will use the wrong value. This is called a [[Data Hazard]]

With **pipeline stalling** the processor will stall the second **addq** instruction at the decode stage and keep the first one going until it reaches the writeback stage. *Bubbles* are added in the middle which are essentially **nop** instructions. The processor waits until the data is the in the right place before continuing again to ensure no wrong values are used.


## Control Flow Hazards
Instructions like *conditional jump* and *ret* don't use the value of "valP" for the next instruction to execute. In fact, for **ret**, we would need to **stall for exactly 3 cycles.** For conditional jumps, we would need to wait all the way until the we get to the execute stage where we have the necessary logic block to figure out if:
- we take the jump (valC)
- or not (valP)

The logic is from the ifun, icode, and condition code.

#### Branch Prediction
*See [[Data Hazard#Hazard Handling.]] for other solutions*.
Branching is very predictable and we can predict whether we always take a branch, never, sometimes, etc. We assume our prediction is right. If it isn't right, we would need to fix it!
- Always Taken (assume that the nextPC is going to be valC)
- Never Taken (assume that the nextPC is going to be valP)
Lets say we have this snippet of code:
```C
// CODE A
j = 1;
do {
	j++;
} while j < 10;
```
Into y86:
```apl
	irmovq 1, %rax
	irmovq 1, %rsi

Loop:
	addq %rax, %rsi
	irmovq 10, %rdi
	subq %rsi, rdi
	jg Loop
```

And this error handling:
```C
# CODE B
if (p == NULL) {
	//handle error
	return -1;
}
```
In y86:
```apl
# assume p is in #rbp
	andq %rbp, %rbp
	je error
# rest of the function here
error: 
	irmovq -1, %rax
	ret
```

For **CODE A**:
- We should predict that we always take the jump. From a human perspective, we can see that we are in loop and more often than not, we are going to loop multiple times. (ALWAYS TAKE IN A LOOP)
- "We take the loop more often than not, so therefore if we implelment branch prediction, we should always take."
For **CODE B:**
- We can see that in the if statement, we are *handling an error*. Things mostly go right, but sometimes go wrong! We are writing an if statement for each small thing, and with a bunch of if statements, we would need to only have 1 *if* go wrong. 
- Therefore, we would predict that we (NEVER TAKE IN AN IF STATEMENT).

**Takeaway**:
- If the branch takes you to a greater address, predict that we *never take*.
- If the branch takes you backwards to a previous address, predict that we *always take*.
---
- Backward Taken; Forward not taken. (need extra circuitry, Current PC and valC but very dooable).

#### What if its wrong?
- The data we need to determine if our branch prediction is wrong, is in the execute phase. Our condition codes are there and since it takes 2 instructions from fetch to execute, the processor will start executing 2 non-needed instructions before squashing them.