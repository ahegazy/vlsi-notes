# Synthesis
source: [this](https://www.youtube.com/watch?v=IxcAvkb2EoY&list=PLyWAP9QBe16qWQzq_IQtGKO9Yz8QvCWvY&index=12&t=0s) video from the series on ASIC design flow

# Synthesis
- Step at which the design starts to become hardware

## Design flow
- High level modeling 
- Fixed point modeling --> Fixed point simulation

### RTL modeling --> behavioural simulation
- Behavioural simulation is bit-accurate and cycle-accurate 
- Functional checking.
- Missing timing information (delays)
- Output from behavioural simulation should match the output from fixed point simulation bit-by-bit

### Synthesis --> post synthesis simulation
- Synthesizer uses the library and the VHDL model 
- Interprets the VHDL model in terms of building blocks exists in the library
- Its output is still a vhdl file called a netlist, contains instantiations of standard cell that exist in the library and the connections of these files with each other
- Synthesis (behavioural VHDL to structural VHDL derived entirely from the library)
- Output have information from the library about delays so this allows the synthesizer to calculate critical path delay, and delays of all paths
- post synthesis simulation is simulation like behavioural simulation but contains information about gate delays

> *last modified 15/04/2020*