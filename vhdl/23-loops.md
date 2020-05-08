# Loops
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## For generate statement
  - It is different than loops
  - It’s is a structure that is used in concurrent vhdl outside the process, 
  - Doesn’t describe a loop, it describes a repeated component instantiations
  - There is usually something that binds those instationations together like having input or outputs coming from the same bus
  - This is particularly true when the number of instantiations is generic 
  - It can always replaced by multiple instantiations
## loops 
  - Used in the sameway like programming languages 
  - Can only be used in process statement, and happens in sequence 

### For loops
  - Variable is implicitly declared, sensitivity list is full, but an implicit latch is created because the if statement has no else

```
process(Din)
begin
    for loop_condition in 0 to 9 loop -- loop condition is a variable
        if conv_integer(Din) = loop_condition then
            flag <= '1';
        end if;
    end loop;
end process;
```

### While loops
  - A more complicated conditions can be used in while loops, increment can be more than one unlike for loops
  - Any variable has to be explicitly declared in the process declarations section

```
process(Din)
    variable loop_condition : integer;
begin
    loop_condition := 0;
    while loop_condition < 10
        if conv_integer(Din) = loop_condition then
            flag <= '1';
        end if;
        loop_condition := loop_condition + 1;
    end loop;
end process;

```

### loops execution time
  - It takes zero time to execute the loop, everytime the process is called the loop executes once instantaneously 
  - Exit or break and next or continue statements can be used in loops

```
process(Din)
begin
    for loop_condition in 0 to 9 loop
        if conv_integer(Din) = loop_condition then
            flag <= '1';
        end if;
        if conv_integer(Din) = 5 then
            exit
        end if;
        if conv_integer(Din) = 3 then
            next
        end if;
    end loop;
end process;
```
## loops synthesises 
  - Loops in general are synthesizable 
  - Problem with some loops is that you can't guess what kind of hardware the synthesizer is gonna replace the loop with
  - This is particularly true with very complicated loops with next or exit statements, or loops embedded in conditions
  - Advice about loops if you can guess the hardware will result from using the loops then use it,
  - If you find yourself using the loops the same way as in programming languages then you probably need to step back and guess what kind of hardware would result from the description you are using
  - Example of using loops in shift registers 
