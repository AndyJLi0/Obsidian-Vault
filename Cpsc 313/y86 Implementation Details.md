![[y86 implementation.png]]
- **icode/ opcode** tells you how long the instruction is. (1,2,9,10 bytes?)
- The **oval** values are just signals on the wire. 
- the dark blue **blocks** are logic blocks that determine which value (signal) to route through at any moment.
- We don't have instructions like "*add offset to value m[%r4] TO %r4*", since there's no wire from **valM**  to the **ALU**. We would need the ALU twice in 1 instruction for this to happen!
- The **ALU** can only make 1 computation per instruction.
- any **constants** can be implemented by connecting the correct bit to POWER, with all other bits set to GROUND.
### y86 Logic Bocks

#### ALU A 
The inputs for ALU A include:
- **valA**: for any of the ALU operations, **OPq**, and **rrmovq**.
- **valC**: for the offsets for **rmmovq** and **mrmovq**, and **irmovq**(adds zero to this constant).
- **+/- 8**: for **push**, **pop**, **call**, ret (**see** [[y86 Instructions]])
The output is just a value that goes into ALU A. Below is the set of statements that determine what the output for ALU-A is:
```C
if (icode == I_OPQ || icode == I_RRMVXX)  
	return valA;  
else if (icode == I_IRMOVQ || icode == I_RMMOVQ || icode == I_MRMOVQ)  
	return valC;  
else if (icode == I_PUSHQ || icode == I_CALL)  
	return -8;  
else  
	return +8;
	
//below is implemented with a switch statement:

uint64_t aluA(y86_icode_t icode, uint64_t valC, uint64_t valA){
    switch(icode) {
        case I_OPQ:
        case I_RRMVXX:
            return valA;
        case I_IRMOVQ:
        case I_RMMOVQ:
        case I_MRMOVQ:
            return valC;
        case I_CALL:
        case I_PUSHQ:
            return -8;
        case I_RET:
        case I_POPQ:
        case I_HALT:
        case I_NOP:
        case I_JXX:
        default:
            return 8;
    }
    
}
```

#### AluB
This computes the value for the second operand of the ALU. Note that all instructions either don't use the ALU, or take in valB into the ALU (stack pointer, the register which to add, etc.) In the special case of **irmovq**, we add 0 to the constant. for **rrmovq**, we also add 0 to the offset of our *read* register, and our *write* register (rB) is pasted to **destE**. (again, however not used in valB)!
#### NextPC
The inputs for nextPC include:
- **valC:** the 8 byte value from the instruction.
- **valM**: the value fetched from the memory stage of execution.
- **valP**: the value obtained from passing PC into incrementPC (the *expected* nextPC).
- **cond**: the condition code. 
```c
uint64_t nextPc(uint64_t valC, uint64_t valM, uint64_t valP, int cond, y86_icode_t icode){
    switch (icode) {
        case I_RET:
            return valM;
        case I_CALL:
            return valC;
        case I_JXX:
            return cond == 1 ? valC : valP;
        default:
            return valP;
    }
}
```

#### Addr
Addr computes the address which the processor will read/write from in memory. Note that **valB** is  a value that does not have any ALU operations done to it, while **valE** is after ALU operations. Since we need to change the stack pointer by 8 for **call** and **pushq**, we return **valE**, but since **ret** and **popq** use the stack pointer itself before the nextpc operation, we will use **valB**
```c
uint64_t addr(uint64_t valB, uint64_t valE, y86_icode_t icode)
{
    switch (icode) {
        
        case I_RMMOVQ:
        case I_MRMOVQ:
        case I_PUSHQ:
        case I_CALL:
            return valE;
        case I_POPQ:
        case I_RET:
            return valB;
        case I_OPQ:
        case I_HALT:
        case I_NOP:
        case I_JXX:
        default:
            return 0xF;
        
    }
}
```


#### Cond
Here ifun is passed as a parameter an cond produces a non-zero value if the condition codes indicate that the operation encoded by ifun is true.
```C
int cond(y86_condition_t ifun, int OF, int SF, int ZF)
{
    switch(ifun) {
        case C_LE:
            return ZF | SF ? 1 : 0;
        case C_L:
            return SF ? 1 : 0;
        case C_E:
            return ZF? 1 : 0;
        case C_NE:
            return !ZF? 1 : 0;
        case C_GE:
            return !SF? 1 : 0;
        case C_G:
            return !ZF & !SF? 1 : 0;
        case C_NC:
            return !OF? 1 : 0;
    }
}
```
