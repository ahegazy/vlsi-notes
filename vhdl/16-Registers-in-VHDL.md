# Registers in VHDL 
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## Registers declaration
-  delcared in processes by chaning a signal D on a clk'event positive edge or negative edge
- Process sensitivity list doesn’t include D, if you put D, it won’t make any difference for the synthesis as the Q value doesn’t change unless there is a positive edge on clk
- For latches we had to put D in the sensitivity list because the condition was true for a long time (half clock cycle) so if D change during the positive cycle we need to update Q

```
entity DFF is
port(
D: in std_logic;
clk: in std_logic;
Q: out std_logic
);
end entity;

architecture structural of DFF is

process(clk)
begin
    if clk'event and clk = '1' then
        Q <= D;
    end if;
end process;

end architecture;
```

## Synchronous and Asynchronous signal
- In asynch signal the signal has to be added to the sensitivity list.
- In sycnh signal the clock masks its function, it won’t function without a clock edge
- Asic tools recognise the form of asynch reset and replace it with a scan register which is helpful in design testability
- Having an asynch reset is very useful, because in state machines it’s always good to have, a state to which you can go back when something goes wrong and that’s the reset state to override anything that could have gone wrong with the clock, pipeline etc

### Synchronous enable

```
process(clk)
begin
    if clk'event and clk = '1' then
        if enable='1' then
            Q <= D;
        end if;
    end if;
end process;
```
### Asynchronous enable

```
process(clk, enable)
begin
    if enable='1' then
        if clk'event and clk = '1' then
            Q <= D;
        end if;
    end if;
end process;
```


## Registers usage
- Two reasons to declare registers 
- Use it in a pipeline
- Use it as a general storage in a register file
