# Good design practices in vhdl 
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## Good practices in code
- Synthesizable
- Predictable the kind of hardware that results from synthesizing the code is something you can foresee in advance
- When sometimes it’s safe to let the synthesizer figure out how to implement the behavioural code you write, sometimes you really wouldn’t like to see what comes out on the other end
- Readability
- Portability among different platforms
- Scalability adaptable bus width
## Synchronous pipeline
- Use single clock and only the positive edge of the clock
- Don’t ever depend on the two edges of a clock
- If you need to use multiple clocks which are not harmonics of each other, then you need to use some synchronization techniques to allow data to pass between clock domains
- Use registers, don’t use latches
- You can use latches to design high speed latch loops, but that should only be used in small circuits and only on ASICs not FPGAs
- Once you finished your code, you should look for implicit latches and remove them, they usually show on the warning report of the synthesis
- DSP blocks between registers should be written using concurrent statements, you can also use processes which have complete sensitivity list and shouldn’t any unspecified conditions for conditional statements 
- Separate sequential processes from combinational logic processes
- For all registers have a global asynchronous reset 
- Registers only used for pipelining, any need for mass storage use memories rams
- Every single number that you use like bus widths, all numbers have to be written in terms of generics
- When you declare a component and use a port map for it be sure to use a generic map and a port map, and make the connection explicitly
- Keep using generics in all levels of instantiations till you reach the top wrapper module 
- The wrapper module shouldn’t have any logic in it, only instantiations of all big modules and define all the generics and all the ports of the top level design
## libraries
- Should include IEEE library and std_logic_1164 package
- Declare  the use of std_logic_arith package which open the use of operators
- Declare either the std_logic_signed or unsigned with std_arith package
## Things that might cause issues during synthesis 
- Functions and procedures 
- often synthesizable, for very complicated functions you can’t always decide what kind of hardware will result from this
- Rule of thumb: if you use a function whose return value will reduce to a number at synthesis time then feel free to use anything within it 
- Packages
- Really good because they make code readable, easily modifiable, portable
- You should separate your packages to fully synthesizable packages and fully unsynthesizable for use with testbenches
- Variables
- Generally synthesizable
- Can sometimes be confusing in terms of how they update within a process because they are counter intuitive once you have digested how signal transactions turn into signal events
- There are cases when we have to use variables like in multiport memories and counters
- Attributes
- Some of them are synthesizable, some are absolutely not, some aren’t but you have to know what you are doing
- Files
- All forms of files access aren’t synthesizable, and usually will not cause synthesis error cause the synthesizer will ignore them
- One exception when you initialize the values of a rom  
- Wait statements
- Complicated, synthesizability of process with wait statements can vary
- wait on signal variable and wait until are generally synthesizable
- wait for and wait are generally unsynthesizable
- Generally any wait statement or attribute that mentions value of time is not synthesizable