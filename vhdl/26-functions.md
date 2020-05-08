# Functions in VHDL
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## Functions 
- Same as in programming language,  but we have to know how they are synthesized
- Declared in the architecture declaration body, if it needs to be used multiple times, it has to be declared in a package
- Can only use variable assignment, cannot use signals within a function
- Function can accept any number of arguments, has to be declared as inputs
- Can have one return value 
- Functions can be called as a concurrent or sequential statements
- Function body can be written using any statement that can be used within a process statement and execution of statements within a function happen sequentially
- Values of variables or constants within a function can exist only within a function call and will reset between different calls
- You cannot use wait statement in functions

```
function quad_calc(a,b,c,x : integer) return std_logic_vector(16 downto 0) is
        variable power1 : integer := 2;
        variable power2 : integer := 1;
        variable power3 : integer := 0;
        variable return_value : integer ;
begin
    return_value := a*x**power1+b*x**power2+c*x**power3;
    return conv_std_logic_vector(return_value,17);
end quad_calc;
```

## pure and impure functions
- Functions can be declared a pure or impure, majority are pure
- Pure functions are functions whose return value is defined exclusively through its input arguments and its impact on the world happens only by its return value
- Impure functions are functions whose return value depends on other variables outside the function

```
impure function raise_to_power(constant x : integer) return integer is
    return x**global_power;
end raise_to_power;
```

## synthesis
- Functions are generally synthesizable
- The only problem is what results from the synthesis cause functions can use very flexible syntax and tend to use a lot of arithmetic 
- Rule of thumb: if you use a function whose return value will reduce to a number at synthesis time then feel free to use anything within it 