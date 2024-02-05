#pipeline  #cpsc313
The other type of hazard is called a [[Control Hazard]]. Also see [[Pipeline Registers and Pipeline Stalling#Other Control Flow Hazards]]

**Data Hazard:** I don't know what the value is because it hasn't been set yet.

## Example with ADDQ

Suppose we have the instructions
```apl
mrmovq 8(%rbp), %rax
addq %rax, %rbx
jmp 0x2000
```
the value inside register %rax is not ready for the second instruction addq if this was a *pipelined* version of our processor. In a *sequential* version, it would be fine. The value of %rax is not ready until **THE VERY END OF THE WRITEBACK STAGE** (the tick where the instruction leaves the writeback stage). Therefore, the decode stage cannot be executed for the second instruction. It would need **NOP** instructions. Specifically **3**.
```apl
mrmovq 8(%rbp), %rax
nop # mrmovq at decode
nop # mrmovq at execute
nop # mrmovq at memory
addq %rax, %rbx # mrmovq at writeback, addq at fetch
jmp 0x2000 # %rax info finally updated, adddq at decode and no hazard!
```

## Example with PUSHq
```apl
Ln_0:	divq   %r13, %r10
Ln_1:	mrmovq   0x1154(%rsi), %r12
Ln_2:	pushq   %rbx
Ln_3:	pushq   %r11
Ln_4:	addq   %r11, %rbx
```
pushq %r11 doesn't have the correct stack pointer value when pushing since the previous pushq instruction hasn't modified %rsp yet. Therefore, we would need **3 nop instructions** to mitigate this data hazard.
## Hazard Handling.

Consider the following:
```apl
addq %rax, %rbx
subq %rbx, %rcx
```
### Pure Software

### Stalling
- See [[Pipeline Registers and Pipeline Stalling#Pipeline Stalling]]

 ![[y86 stalling.png]]
 We can see we stall **3 times**, analogous to having 3 nops. The subq instruction can grab the correct values from the registers since we have written them back already from addq. Stalling is nice since we don't need to write the nops ourselves, and the processor does it for us. 

### Value/Data Forwarding in Hardware
- Address data hazards. We can avoid stalling and make our processor more efficient if we can forward the values already computed (lets say in execute), but aren't in the register yet (not until writeback). In the image in the the *stalling* section, we see that the values aren't ready until **W**, however are technically computed in **E**. We as ourselves these questions:
	- By what point does subq need the value of %rbx? (right at the beginning of Decode)
	- At what point does addq have the value of %rbx? (at the end of Execute, in e_valE **NOT E_valE**)

Now lets say we have an instruction between addq and subq
```
addq %rax, %rbx
irmovq $DEADBEEF %rdi
subq %rbx, %rcx
```
Now to answer the questions above:
- subq still needs the value of %rbx in the decode stage (decode pipeline register, therefore pass it to FETCH)
- addq has the value of %rbx now in (w_valE).

Lets say we always forward the signal of (M_valE) into the decode register. We would need 2 separate instructions between 2 instructions that use the same register, since between M and F there are 2 instructions! 	
>[!Why 2 instructions and not 1]
>
>The second ALU instruction is will actually be in the fetch stage because if it was decode, we would miss the signal coming from M_ValE and it would be too late! We need to forward it to *decode* before the second instruction gets there!
### Branch Prediction
- Addresses (some) control hazards
- See [[Pipeline Registers and Pipeline Stalling#Branch Prediction]]