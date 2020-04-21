# operators
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

- When using operators in vhdl, you have to make sure that the signal that is used to store the result is wide enough to allow the storage of the result noiselessly 
   - An adder of two N bits wide operands, requires N+1 bits for the result
   - A multiplier of M and N bits wide operands, requires M+N bits wide result
- Then you can perform truncation of the result if you want
- Comparison operators act on integers or std_logic types, and produce a boolean or logical result either 0 or 1 but they are not binary values or bits, they are true and false results
   - They will generally be implemented in hardware using subtractors then will look at the sign bit of the result to find out if the operand greater or not
- add, subtract, shift, concatenation, comparison operators are safe operations, hardware cost is very low and the synthesizer is really good at producing good hardware out of them.
- Caution with multiply operator, cause they are relatively slow, gotta be careful about the width of the output which require performing fixed point simulation before vhdl, and the number of multipliers used and if you could timeshare some of multipliers do so.
   - In a good hardware implementation the critical path will usually be the path with multipliers
- Suggest never use exponentiation, division, remainder, quotient operators because it’s really hard to predict how this would get translated into hardware depends on the vendor and the synthesizer, but anyway the resulting hardware is gonna be really bad, slow, and power hungry
   - Mostly division can be replaced by multiplication somewhere else in the logic
- Shift operators are preference and they’ll produce predictable hardware but it’s more readable and easier to write shifting using the concatenation operator
