- call **Dest**: decrement %rsp by 8 (allocating space) and put the address of the next instruction after we return from Dest. This is so when we call **ret**, we pop the address of the next instruction off the stack, and we seamlessly go back to where the function was called and continue execution. 
	- In [[y86 Buffer Overflow]], we change the value of this value in the stack, so we can change where we return to after the function is called!
- 