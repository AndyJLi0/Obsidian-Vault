#### Resgisters
The y86 has 15 registers, important registers include %rsp (the stack pointer), %ra (where we put our return value), and %rbp (the base pointer of a stack frame).  Registers store 8 bytes maximum, with the PC counter (a special resgister), being able to store 10 bytes. It stores the immediate next instrcution to execute.
#### Condition Codes and Status Registers
- Zero Flag: when the last ALU operation is 0.
- Sign Flag: when the last ALU operation is negative.
- Overflow Flag: when the last ALU operation resulted in overflow.
	- Operations that can set the Condition codes include all OPq operations. It is the ALU itself that actually sets the condition code (OPcode == 6)
---
- Status registers indicate normal operation or an error in operateion
	- AOK - operation is good.
	- HLT - halt!
	- ADR - bad address.
	- INS - invalid instruction.



#### Stack Operations
- These include **PUSHq, POPq, CALL, RET**
- When used, ALU-A (see [[y86 Implementation Details]]) uses +8 (dealloc) for pop and ret, and -8 (allloc) for push and call.


- CALL **Dest**: decrement %rsp by 8 (allocating space) and put the address of the next instruction after we return from Dest. This is so when we call **ret**, we pop the address of the next instruction off the stack, and we seamlessly go back to where the function was called and continue execution. 
	- In [[y86 Buffer Overflow]], we change the value of this value in the stack, so we can change where we return to after the function is called!
- 