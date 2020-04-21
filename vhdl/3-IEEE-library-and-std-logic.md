# IEEE library and std logic
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## Libraries
- Any vhdl design is dependent on the presence of libraries.
- libraries are collections of entities, components and functions that we have already added to the design 
- Any library contains a number of packages, each package contains functions, types, subtypes. 

### Two important libraries
   - Work library, used by default, no need to indicate using it, it’s the library that contains everything that you have designed so far
   - IEEE library, almost always used, it contains a set of data types, functions, operations that are very commonly used and very useful 

### IEEE library
- IEEE library contains five major packages.
- most important package ieee.std_logic_1164, 
   - this package opens up the standard logic data types for use
   - A value of 0 and 1 indicate that the node is strongly driven towards ground or supply from a low impedance path of an uncontested logic gate
   - Allows us to define different values for signals other than 0,1: X, U, Z, -, H, L, W
   - It’s extremely important to remember that std logic can have more values then 0 and 1, cause when we design we are mostly concerned with 0 and 1, cause they are meaningful values, but the synthesizer doesn't really know that they are just 0 and 1, it knows that they can be something else.
   - If we forget this fact, we can end up adding latches where we don’t want them to exist.

### Other packages 
   - std_logic_arith, contains description of all arithmetic operators, a redefinition of them which are defined by default in vhdl in bit type
   - std_logic_signed, std_logic_unsiged mutually exclusive, you cannot declare using both because these two packages declare that the numbers in the registers are either signed two’s complement numbers or unsigned positive numbers, so they augment the arithmetic package but indicating the kind of numbers in the regs
   - std_logic_textio allows us to use files inputs and outputs to write and read data from files, four functions and data types.
