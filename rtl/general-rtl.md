# General RTL advice
source: multiple sources

## Tips
- no feedback from a combinational circuit, put a register w clock and feedback it

- don't use a gate output as a clock (clock skew, affected by gates propagation delays), same clock for all (synchronization)-> use MUX in the data path

- don't generate a pulse using gate delays (don't cascade buffers/not gates to get a pulse after a certain amount of time <sum of gates' propagation delays>), ->use clocked flipflops instead to get output after # of clocks

- use synchronous circuits to eliminate glitches, unwanted narrow pulse caused by propagation delays of circuits, ->use a clocked flipflop at the output to get the output depending on the clock.

- use only synchronous counters and not ripple counters.

- It is illegal to nest modules, i.e., write module within a module. (compilation error), instead call a module within a module

```
    module module_1 (// List the first module I/Os here) ;

            // Declare the first module I/Os, wires, and registers.

            // Write the required combination and sequential

            // logic for the first module.

            module_2 U1 (// List second module I/Os here, calling ports by name) ;

            module_3 U2 (// List third module I/Os here, calling ports by name) ;

            // Call other modules, if any. U1, U2, etc. are instantiations.

            // Note the presence of ‘;’ at the end of each of the statements.

    endmodule // This signifies the end of module_1. Note that there is no space between ‘end’ and ‘module’.
```

- separate combinational and sequential circuits, for ex: always block for AND gate depending on A,B (always @(A or B)), and another clocked always block for the D flip flop to output the output

- for a flipflop if you set Q = d, Q_n = !d; this will produce two flipflops instead,use inverter: assign Q_n = ! Q; !d

- Whatever signals are used in an ‘always’ block, they are declared as registers.

- if statement is a MUX, if else is not specified, a latch is created to hold the previous value (latch i/p: signal, clock:SEL). Latches are inferred unless all signals are assigned in all branches.

- Avoid all latches in your design since they pass on glitches in the circuit.

- it would be advisable to restrict the number of ‘if–elseif–else’ statements to four or five based on experience. In designs, where this thumb-rule is exceeded, one may explore the possibility of using ‘case’ statements in lieu of ‘if–else if’ statements.

- ‘case’ statements must be used if conditions are mutually exclusive. (uses a signle redundant variable)

## Tools directives
- full_case , parallel_case directives 
- When adding "full_case" or "parallel_case" directives to a case statement, 
- the directives are added as a comment immediately following the case expression at the end of the case statement header and before any of the case items on subsequent lines of code.

### full_case 
`case (SELECT) // synopsys full_case;`
- indicates that all cases are specified even if they don’t consider all possibilities.
- one-hot assignment (i.e., only one ‘1’ entry in the signal such as SELECT = 010) is shown first, change only 1 bit

### parallel_case
`case (SELECT) // synopsys parallel_case;`
- indicates that all cases listed are mutually exclusive to prevent priority encoded logic.(latches created)

### combination
`case (SELECT) // synopsys parallel_case full_case`
- latches may be eliminated and a MUX created instead,The chip area is considerably lower in this case.


