# Variables and signals
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.


## Signals vs variables
   - Signals are real physical electrical nodes in a circuit
   - Variable is a temporary storage location that exists only within a process

## variable usage
   - Useful in multiport memory, loops, counters
   - Should be careful about how they are used, and should be aware of what they synthesize into
   - They can be initialized to an initial value in the first process call, then they update their value each process call
   - There is also global variables
   - Unlike signals variables update immediately after they are assigned, they don’t get scheduled to update at the end of the process, there is no distinction between transactions and events in variables
   - A signal assignment is a transaction, and it becomes an actual change or an event when we meet a wait statement
