### Pipeline Registers
In the pipeline implementation of the y86 processor, we have a series of pipeline registers that can hold basically the entire instruction (if it wanted to) in the middle of every stage. Like a latch, it saves the signals from the previous clock cycle and releases it when it takes in another. As we move through stages, we only need to pass through certain key signals that can keep each stage knowing what to execute:
![[y86 rearranged.png]]

### Pipeline Stalling
A problem that we need to address when trying to implement a pipeline processor is the fact that information in registers/ memory may not be available to us when the instruction is executed because the it relies on information altered by the previous or close to previous instruction. If we have two **ADDQ** instructions back to back and the second one uses the register from the first, we will use the wrong value.

With **pipeline stalling** the processor will stall the second **addq** instruction at the decode stage and keep the first one going until it reaches the writeback stage. *Bubbles* are added in the middle which are essentially **nop** instructions. The processor waits until the data is the in the right place before continuing again to ensure no wrong values are used.


#### Other Control Flow Hazards
Instructions like *conditional jump* and *ret* don't use the value of "valP" for the next instruction to execute. In fact, for **ret**, we would need to stall for exactly 3 cycles. For conditional jumps, we would need to wait all the way until the we get to the execute stage where we have the necessary logic block to figure out if:
- we take the jump (valC)
- or not (valP)

The logic is from the ifun, icode, and condition code.

#### Branch Prediction
Branching is very predictable and we can predict whether we always take a branch, never, sometimes, etc. We assume our prediction is right. If it isn't right, we would need to fix it!