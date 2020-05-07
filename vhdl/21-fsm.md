# Finite state machines
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## Controller and data path architecture
- fsm is a systematic and reliable way to write controllers for data path architectures
- In the datapath and controller architecture we separating processing from control completely 
- processing units are included in the datapath, and they only contain processing units, it doesn't know how or when to operate.
- anything that has to do with intelligence is kept in a controller and the controller contains no processing except the processing necessary to determine how to sequence the events
- the two communicate with each other using control and status signal 
- status signal from the datapath to controller they indicate to the controller the current state of the data path
- the controller then takes the status signal and depending on it, determines what to do next 
- then send the command to the datapath using control signal 
- data inputs and outputs go to the data path and come from it directly
- if some of the inputs are needed to make a decision by the controller, then they'll be rerouted by the datapath through status signal

![data-path-controller](imgs/fsm/data-path-controller.png)

## Finite state machines
- controllers which look at the datapath and break the state of it down two a finite number of states
- a state is a condition in which the circuit exists which defines a specific outputs and inputs that the datapath has
- we should be able to make a decision about what to do next knowing the current state and the output of the datapath (status signal) only 
- one of the properties of a finite state machine is that at every state all of the control signals going to the datapath are defined 
- a properly designed FSM is a state machine when we are in a certain state we know all of the outputs of the FSM, all of the controls provided to the datapath, we also know the next transition, what is the next state we are going to be, but that can only be deduced if we know the inputs as well as the current state 
- state transition diagram and state transition table

![fsm](imgs/fsm/fsm.png)

![fsm-flow](imgs/fsm/fsm-flow.png)


# coding state machines in vhdl
- three process approach, the FSM is coded entirely using three processes 

## Three state processes
- state update process: update the current state to the next state
- next state process: turns in what the next state should be
- output process: turns in the current outputs of the FSM are

## fsm inputs and outputs
- input ports to the FSM should only be the status signals from the datapath, plus the clock and reset
- output ports to the FSM should only be the control signals provided to the datapath

```
Library IEEE;
Use ieee.std_logic_1164.all;

Entity fsm is
    port ( operation : in std_logic_vector(1 downto 0);
        FU_done : in std_logic;
        start : in std_logic;

        EN1, EN2, EN3, EN4 : out std_logic;
        EN_reg : out std_logic;
        operation_sel : out std_logic_vector(1 downto 0);

        clk, reset : in std_logic
        );
end fsm;

architecture behavioral of fsm is
    type state is (reset_state, await_start, operate_select, operate1, operate2, operate3, operate4, select, store);
    signal current_state, next_state : state;
begin
```

### Next state process
- next state process is a combinational process that determines the next state only, and doesn't update the state of the machine 
- it has a complete sensitivity list, 
- when others helps when the state signal contains unkown state like X,Z value so we go to the reset state because the state machine is in a weird state and doesn't know what to do 
- use elses in conditions to avoid implicit latches 

```
next_state: process (current_State, start, operation)
begin
    case current_state is
        when reset_state =>
            next_state <= await_start;
        when await_start =>
            if start = '1' then
                next_state <= operate_select;
            else
                next_state <= await_state;
            end if;
        when operate_select =>
            if operation = "00" then
                next_state <= operate1;
            elsif operation = "01" then
                next_state <= operate2;
            elsif operation = "10" then
                next_state <= operate3;
            elsif operation = "11" then
                next_state <= operate4;
            end if;
        when operate1 =>
            next_state <= select;
        when operate2 =>
            next_state <= select;
        when operate3 =>
            next_state <= select;
        when operate4 =>
            next_state <= select;
        when select =>
            next_state <= store;
        when store =>
            next_state <= await_start;
        when others =>
            next_state <= reset_state;
    end case;
end process;
```
### state update process
- a clocked process, when positive edgo of the clock it updates the current state with the next state value

```
state_update: process(clk, reset)
begin
    if reset = '1' then
        current_state <= reset_state;
    elsif clk'event adnd clk='1' then
        current_state <= next_state;
    end if;
end process;
```
### outputs process 
- outputs process is a combinational process, based on the current state it determines all the outputs of the state machine (all the control signals) even if they are trivial or don't care, we have to define them all, to avoid creating an implicit latch by exhaustivly listing all outputs 
- it should have a when others defined at the end and define what happens to all output signals if an unkown state came, the safest way to do it is to copy everything from the reset state and put it in when others 

```
outputs_process: process (current_state, operation)
begin
    case current_state is
        when reset_state =>
            EN1 <= '0';EN2 <= '0';EN3 <= '0';EN4 <= '0';
            EN_reg <= '0';
            operation_sel <= "00";
        when await_start =>
            EN1 <= '0';EN2 <= '0';EN3 <= '0';EN4 <= '0';
            EN_reg <= '0';
            operation_sel <= "00";
        when operate_select =>
            EN1 <= '0';EN2 <= '0';EN3 <= '0';EN4 <= '0';
            EN_reg <= '0';
            operation_sel <= "00";
        when operate1 =>
            EN1 <= '1';EN2 <= '0';EN3 <= '0';EN4 <= '0';
            EN_reg <= '0';
            operation_sel <= "00";
        when operate2 =>
            EN1 <= '0';EN2 <= '1';EN3 <= '0';EN4 <= '0';
            EN_reg <= '0';
            operation_sel <= "00";
        when operate3 =>
            EN1 <= '0';EN2 <= '0';EN3 <= '1';EN4 <= '0';
            EN_reg <= '0';
            operation_sel <= "00";
        when operate4 =>
            EN1 <= '0';EN2 <= '0';EN3 <= '0';EN4 <= '1';
            EN_reg <= '0';
            operation_sel <= "00";
        when select =>
            EN1 <= '0';EN2 <= '0';EN3 <= '0';EN4 <= '0';
            EN_reg <= '0';
            operation_sel <= operation;
        when store =>
            EN1 <= '0';EN2 <= '0';EN3 <= '0';EN4 <= '0';
            EN_reg <= '1';
            operation_sel <= "00";
        when others =>
            EN1 <= '0';EN2 <= '0';EN3 <= '0';EN4 <= '0';
            EN_reg <= '0';
            operation_sel <= "00";
    end case;
end process;
```
