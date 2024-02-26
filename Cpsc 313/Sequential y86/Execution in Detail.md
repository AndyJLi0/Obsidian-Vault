#sequential #cpsc313*What must happen in each phase for each instruction*
#### rrmovq in three ways
- R[rB] <- R[rA]
- From implementation, we send rA into ValA, 0 into ValB, valE = valA. Then write valA into register rB.
- by specification,
	- Fetch: icode:ifun = M[PC], rA:rB =M[PC+1], valP = PC + 2
	- Decode: valA = R[rA]
	- Execute: valE = valA + 0
	- Memory: nothing
	- Writeback: R[rB] = valE