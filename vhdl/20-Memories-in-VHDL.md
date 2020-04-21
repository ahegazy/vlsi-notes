# Memories in VHDL 
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## Registers and storage
- Registers are not suitable for large scale storage in asics and fpgas, instead we have to declare memories in a way that allows the synthesizer to know we are talking about memories instead of registers

## Memories in FPGAs and ASICs
- Two types of memories: roms and rams
- the syntax allows the synthesizer to know realise we are talking about memories, what it does with memories depends on the platform
    - in FPGAs it usually implements it as block rams (specialized cells in fpgas that performs mass storage efficiently)
    - in asics srams and embedded memories usually implemented using a separate memory compiler and the synthesizer will often ignore the parts where we declare memories and leave them to the memory compiler, otherwise the synthesizer might implement it as a large register file
- when the synthesizer see the initialization of the memory contents it knows we are declaring a rom as its contents exist at compilation time
- in case of a large rom, we declare the contents of the rom usually using file io
- we can remove the effect of the enable by hardwiring it when we instantiate the rom
  
## Rams  

- what to do with data out bus when writing 
- read first the last value from the location to the output before writing to it
- latch the output ,read the last value of the data out port regardless of what was written on this cycle
- write first, the value of data out port takes the new value
  
## Multiport memory
- mutli port memory, multiple address buses 002,004
- shared_variable is a variable declared in the architecture part, and exists between multiple processes 
- we have mutliple processes a process for every independent bus, they share the memory contents, so we need to share the memory between them
- if you use a signal to declare the memory and writing to it by the two processes, you'll get an error because a signal cann't be written to by two processes, this will create a contention on the node and it's not allowed.
- the only way to create multiple processes writing to the same node is to use a shared variable.

