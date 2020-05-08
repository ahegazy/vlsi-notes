# Procedures and overloading
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## Procedure
- Similar to functions with two differences
- Procedures can use signal assignments
- Procedures can have arguments that are input, output or inout direction and doesn’t have a return cause it can return values as output signals
- Procedures ins and outs are similar to entity’s ports in a way, they are the bridge between entities and functions in a way

```
procedure batch_calculate(
    a, b, c : in integer;
    signal d, e, f: out integer;
    signal g : inout integer) is
begin
    d <= a*a;
    e <= b*b;
    f <= c*c;
    g <= g*g;
end batch_calculate;
```

## overloading
- Common in programming languages
- Redefining a function multiple times with different arguments lists
- It’s legal and useful, the synthesizer look at the argument list used when the function is called and determine the definition
- Operators are special functions, operators in vhdl are overloaded cause you can say a+b with any type std_logic_vector, integer, and it’ll use the + with the different data types differently
## predefined functions in std packages 
- Conversion functions in std packages convert a type to another type, usually used to move between std_logic_vector and integers 

### conv_integer 
- conv_integer(std_logic_vector)
- Converts std_logic_vector into integers
- Has to be used either after defining either std_logic_signed or std_logic_unsigned packages, they’ll redefine the function for signed or unsigned numbers

### conv_std_logic_vector
- conv_std_logic_vector(integer, size of the bus) 
- Has to be defined either for signed or unsigned numbers

### conv_signed and conv_unsigned
-  conv_signed(), conv_unsiged()
- used to convert std_logic_vector types
- they can accept integer numbers
- Convert between unsigned and signed types and used when we mix between them in operation
- If you are unsure of using signed or unsigned number it’s safer to use signed numbers cause you don’t know if you have negative numbers
