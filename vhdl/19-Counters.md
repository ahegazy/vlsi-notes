# Counters
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## Counters
- A counter consists of a Register and an adder
- Counters are one of the main exceptions of the rule about not mixing arithmetic and registering in the same process because we know what they do and look like and it’s predictable
- In the example If you hardwire the count_up signal to 1 the synthesizer will know you want an up counter only and it’ll replace it with a standard cell that’ll only count up, same goes for other signals
    - So you don’t need to change the syntax if you want different hardware behaviour, you can hardwire signals and the synthesizer is smart enough to detect what you mean by this

```
process(clk, reset)
begin
    if reset='1' then
        counter_out <= (others => '0');
    elsif clk'event and clk = '1' then
        if enable_counter = '1' then
            if count_up = '1' then
                counter_out <= counter_out + 1;
            else
                counter_out <= counter_out - 1;
            end if;
        end if;
    end if;
end process;
```

## Counters using variables
- Counters can and often are declared using variables,
- One of the ways in which variables can be safely used and the results of synthesis are very predictable
- One problem is that the variable exists only within the process, so if you need this value of the counter to be used somewhere else, with concurrent statements outside processes for example, you are better off declaring the counter using signals

```
process(clk, load_counter)
    variable counter_out : integer range 0 to 7 := 0;
begin
    if clk'event and clk='1' then
        counter_out := (counter+1) mod 5;
    end if;
end process;
```
