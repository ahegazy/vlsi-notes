# verilog random notes
- source: different sources

## Combinational and sequential circuits
- Combinational circuit 
    - AND, OR , etc ... immediate response, propagation delay
- Sequential circuit
    - combinaltional + registers : flip flops
    - op = fn(input, previous state)
    - synchronous circuits

## assign
- assign  x = y ; //buffer
- assign x = a&b ; //and gate
- ! : inversion (NOT) 
- ~ : multi-bit inversion bitwise (NOT)

## always block
```
    always @(A or B or C or F10) // whenever A,B,C or F10 change, go down
        begin
   	    F9 = (A&B)|(B&C)|(C&A) ; //AB+BC+CA // brackets () guide the synthesis tool
   	    F10 = {A, B, C} ; //concatenation , A: MSB, C: LSB
   	    F10 = {A,B,C, 3{0}}; // same as ^ but add 3 zeros at LSB  
   	    F12 = F10 << 2; left shift, put 2 zeros at LSB
    end // concurrent statements
```

## Eight input MUX 
- code using ‘always’ and ‘case’ statements.

```
// Fastest possible hardware implementation.
always @ (A or B or C or I0 or I1 or I2 or I3 or I4 or I5 or I6 or I7)
    begin
   	 case ({A, B, C})
   		 3'b000: mux8 = I0 ; // Read the input addressed by ABC
   		 3'b001: mux8 = I1 ; // and output the same to mux8.
   		 3'b010: mux8 = I2 ;
   		 3'b011: mux8 = I3 ;
   		 3'b100: mux8 = I4 ;
   		 3'b101: mux8 = I5 ;
   		 3'b110: mux8 = I6 ;
   		 3'b111: mux8 = I7 ;
   		 default: mux8 = 0 ; // The value can be I0 or any other.
   	 endcase
    end
```

## FULL ADDER
- Behavioral realization

```
    assign sum_total = (A + B) + C; // parenthesis: synthesis tool efficient optimization
```
- Data flow structure
```
    assign sum = (A^B) ^C ; // Realize sum.
    assign carryo = (A&B)|(B&C)|(C&A) ; // Realize carry out, AB + BC + CA.
```

## Misc 
- Gates that can be used in a structural design are ‘nand’, ‘nor’, ‘xor’, ‘xnor’, ‘buf’, and ‘not’. 
- this structure can be effectively used for the implementation of tri-state buffers/inverters such as the following:

    - bufif0 u1 (out, in, sel) ; // out = in if sel = 0, otherwise out is tri-stated
    - bufif1 u2 (out, in, sel) ; // out = in if sel = 1, otherwise out is tri-stated
    - notif0 u3 (out, in, sel) ; // out = ! in if sel = 0, otherwise out is tri-stated
    - notif1 u4 (out, in, sel) ; // out = ! in if sel = 1, otherwise out is tri-stated

