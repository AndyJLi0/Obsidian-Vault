#pipeline #cpsc313 *instead of sequential execution of instructions, we can implement concurrent execution*
#### **Behold the laundry analogy:**
>In regular laundry world, washer takes 40 minutes, time to change is 20 minutes, dryer takes 60 minutes. In magic laundry word, it takes 100 minutes for a load.

- For 1 load, it 120 minutes in regular laundry world, but takes 100 minutes for magic laundry world.
- For 4 loads, I can start the washer after I put the clothes in the dryer, and cut down time! After every dryer (60 minutes), I can wash and change clothes to the dryer (80 minutes), so it would take $360$ minutes total. Diagram shown below:
![[pipelineExample.png]]


### Terminology
- **Stage:** What we used to call a 'phase' from [[Execution in Detail]]. Each piece of the pipeline is a stage. It is the unit of computation that the processor performs each clock cycle. Therefore, it takes many clock cycles to finish an instruction since we have multiple stages.
- **Pipeline Register:** The extra work we incur by pipelining. Each stage has a pipeline register that captures the set of signals  (values) on which a stage will execute. *Big* registers!
- **Hazard:** Problems we run into when naïvely pipelining.
- **In-flight:** an instruction somewhere in the pipeline.
- **Retired:** instruction has completed execution.
### How Does it Work?
There exists a pipeline register in between each phase. after the clock ticks, the current phase updates the next phase's pipeline register with the information it just computed received, or whatever it needs to continue execution (work it has done so far). Only thing we remember when the clock ticks!
#### Measuring Performance
- Sequential processor has lower latency than pipelining version.
- In a pipelined implementation, lets say FETCH is 200 ps, but DECODE is only 50 ps, we would need to spend 200 ps at each phase since we can't move forward until the *slowest* phase is done. Therefore we would multiply the slowest phase by the number of phases (plus the latency for the pipeline registers). This is really slow!
---
- Another latency is called **retirement latency**. This is the time between completion of one instruction, and the completion of the next instruction.
	- For a sequential processor, this is equal to the regular latency
		- For a pipeline processor, an instruction finishes *every tick of the clock*. so it would be (longest stage latency + pipeline register). 
---
- **Throughput** is basically how many instructions can we push through our processor in a second.
- Typically expressed in giga-instructions per second (GIPS), billion. For a sequential its ~1.75 GIPS.
- For pipeline, roughly ~4.55 GIPS.
---
From sequential to pipeline, we increased latency by 93%, but multiplied throughput by a factor of 2.6

## Takeaways
- Introduces [[Parallelism]]
- Allows multiple instructions to be executing at the same time
- **Increases** single instruction latency
- **Decreases** retirement latency
- **Increases** throughput