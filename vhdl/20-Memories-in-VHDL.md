# Memories in VHDL 
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## Registers
- Registers are not suitable for large scale storage in asics and fpgas, instead we have to declare memories in a way that allows the synthesizer to know we are talking about memories instead of registers

## Memories in FPGAs and ASICs
- Two types of memories: roms and rams
- the syntax allows the synthesizer to know realise we are talking about memories, what it does with memories depends on the platform
    - in FPGAs it usually implements it as block rams (specialized cells in fpgas that performs mass storage efficiently)
    - in asics srams and embedded memories usually implemented using a separate memory compiler and the synthesizer will often ignore the parts where we declare memories and leave them to the memory compiler, otherwise the synthesizer might implement it as a large register file
- when the synthesizer see the initialization of the memory contents it knows we are declaring a rom as its contents exist at compilation time
- in case of a large rom, we declare the contents of the rom usually using file io
- we can remove the effect of the enable by hardwiring it when we instantiate the rom

```
entity rom_block is
    generic(
        address_bits : integer := 2;
        rom_word_width : integer := 4;
    );
    port(
        clk : in std_logic;
        en : in std_logic;
        address_bus : in std_logic_vector(address_bits-1 downto 0);
        data_out : out std_logic_vector(rom_word_width-1 downto 0)
    );
end entity;

architecture behavioral of rom_block is
    type rom_type is array ((2**address_bits)-1 down to 0) of std_logic_vector(rom_word_width-1 downto 0);
    signal rom_contents : rom_type := ("0000","0001","1111","1110");
    signal read_data : std_logic_vector(rom_word_width-1 downto 0);
begin
    read_data <= rom_contents(conv_integer(address_bus));
    process(clk)
    begin
        if clk'event and clk='1' then
            if en='1' then
                data_out <= read_data;
            end if;
        end if;
    end process;
end architecture;
```

## Rams
```
entity ram_block is
    generic(
        address_bits : integer := 2;
        ram_word_width : integer := 4;
    );
    port(
        clk : in std_logic;
        en : in std_logic;
        we : in std_logic;
        address_bus : in std_logic_vector(address_bits-1 downto 0);
        data_in : in std_logic_vector(ram_word_width-1 downto 0);
        data_out : out std_logic_vector(ram_word_width-1 downto 0)

    );
end entity;

architecture behavioral of ram_block is
    type ram_type is array ((2**address_bits)-1 down to 0) of std_logic_vector(ram_word_width-1 downto 0);
    signal ram_memory : ram_type;
begin
    process(clk)
    begin
        if clk'event and clk='1' then
            if en='1' then
                if we='1' then
                    ram_memory(conv_integer(address_bus)) <= data_in;
                else
                    data_out <= ram_memory(conv_integer(address_bus));
                end if;
            end if;
        end if;
    end process;
end architecture;
```

### what to do with data out bus when writing 
- read first the last value from the location to the output before writing to it
```
--- read-first method, data_out takes the value of location from previous cycle
if we='1' then
    ram_memory(conv_integer(address_bus)) <= data_in;
end if;
data_out <= ram_memory(conv_integer(address_bus));
```
- latch the output ,read the last value of the data out port regardless of what was written on this cycle
```
-- latched output, The data_out port preserves its last known value
if we='1' then
    ram_memory(conv_integer(address_bus)) <= data_in;
else
    data_out <= ram_memory(conv_integer(address_bus));
end if;
```
- write first, the value of data out port takes the new value
```
-- Write first, the value of data_out port takes the new value
if we='1' then
    ram_memory(conv_integer(address_bus)) <= data_in;
    data_out <= data_in
else
    data_out <= ram_memory(conv_integer(address_bus));
end if;

```

## Multiport memory
- mutli port memory, multiple address buses 002,004
- shared_variable is a variable declared in the architecture part, and exists between multiple processes 
- we have mutliple processes a process for every independent bus, they share the memory contents, so we need to share the memory between them
- if you use a signal to declare the memory and writing to it by the two processes, you'll get an error because a signal cann't be written to by two processes, this will create a contention on the node and it's not allowed.
- the only way to create multiple processes writing to the same node is to use a shared variable.

```
entity ram_2p is
    generic(
        address_bits : integer := 2;
        ram_word_width : integer := 4;
    );
    port(
        clk1 : in std_logic;
        en1 : in std_logic;
        we1 : in std_logic;
        address_bus1 : in std_logic_vector(address_bits-1 downto 0);
        data_in1 : in std_logic_vector(ram_word_width-1 downto 0);
        data_out1 : out std_logic_vector(ram_word_width-1 downto 0)
        clk2 : in std_logic;
        en2 : in std_logic;
        we2 : in std_logic;
        address_bus2 : in std_logic_vector(address_bits-1 downto 0);
        data_in2 : in std_logic_vector(ram_word_width-1 downto 0);
        data_out2 : out std_logic_vector(ram_word_width-1 downto 0)


    );
end entity;

architecture behavioral of ram_2p is
    type ram_type is array ((2**address_bits)-1 down to 0) of std_logic_vector(ram_word_width-1 downto 0);
    signal ram_memory : ram_type;
begin
    process(clk1)
    begin
        if clk1'event and clk1='1' then
            if en1='1' then
                if we1='1' then
                    ram_memory(conv_integer(address_bus1)) <= data_in1;
                else
                    data_out1 <= ram_memory(conv_integer(address_bus1));
                end if;
            end if;
        end if;
    end process;

    process(clk2)
    begin
        if clk2'event and clk2='1' then
            if en2='1' then
                if we2='1' then
                    ram_memory(conv_integer(address_bus2)) <= data_in;
                else
                    data_out2 <= ram_memory(conv_integer(address_bus2));
                end if;
            end if;
        end if;
    end process;

end architecture;
```
