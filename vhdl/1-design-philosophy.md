# Design philosophy
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## Hardware design philosophy
- VHDL describes hardware, it contains alot of syntax that looks alot like programming
- It’s not a programming paradigm, it’s a description of hardware connections, where it’s and where it lies

## Programmnig vs hardware wires connection
- `T=A;A=B;B=T` VS `T<=A;A<=B;B<=T`
- In programming this implemented sequentially
- In hdl this describes a short circuits between three nodes A,B,T, the three statements are implemented concurrently

## Sequence
- Sequence in VHDL is the exception not the rule, used  to create registers, memories, and we deal with it very carefully
- Everything is implemented in hierarchical fashion in vhdl, and using (instantiating) modules multiple times are very important parts of the design philosophy

## Design wrapper
- The highest level of the design (wrapper) should include subdesigns or modules only, it should contain instantiations of big modules or big blocks only.
   - A glue logic (wrapper contains subsystems and random small logic between them) is a bad design.
   - A better approach is to gather the glue logic on its own subsystem, or to fold it in the subsystems.

## Synthesizability and good code/design
   - Synthesizable code is code that passes through the synthesis without errors
   - Alot of the vhdl syntax is unsynthesizable, it’s easy to pinpoint and tools catch it
   - The problem is not with synthesizability, the problem is with the bad code that passes through the synthesis and gives you bad results
   - Good code is not only code that’ll pass synthesis, but also will produce predictable results after synthesis
   - When we write VHDL the main thing to think about is do we know what kind of hardware would result from the code we are writing? If we can guess then it's a good code.
   - Even Though the syntax allows you to code hardware you don’t know you shouldn’t do it

## up next
Next modules are about understanding how to use the syntax safely and efficiently.