# DRC and LVS
source: [this](https://www.youtube.com/watch?v=9t0xb5TmIs0&list=PLyWAP9QBe16qWQzq_IQtGKO9Yz8QvCWvY&index=14&t=0s) video from the series on ASIC design flow

## DRC and LVS
- After placement and routing we end up with a design that has achieved closure, then we have a layout that reading for taking out (sent to fabrication facility)
## Parasitic extraction
- Extract the effects of parasitics on delay from the overall layout once everything is connected together.
- the impact of these parasitics on delay is gonna be stored on a file called standard delay format file, it’s then fed back to the post PAR simulation to do post layout simulation
- Post layout simulation is basically post PAR simulation with the enclosure of parasitics (capacitances) extracted after parasitic extraction, this is the simulation that gives the best estimate of delays that we can see, if this passes then it’s unlikely the chip would get back
## Design rules check
- Once parasitic extraction is done we have to do DRC (design rules check), automatic process done by the tool and checks whether every single layer in the layout obeys every single rules in the design rules, if found violation, reports to the designer

### Design rule violation in std cells
- Why design rule violations might found while the layout consists of standard cells? 
- std cells come from the vendor that makes the design rules, so they won’t have violations
- When you attach cells together, there can be desgin rule violations.
- can be due to abutment of the cells to each other
- Usually results from the distribution of power, and from routing and from the clock network
- More likely to be found in the metal layers than in the lower layers

## layout versus schematic
- Final step is LVS (layout versus schematic)
- Does the layout does the function that we intended to do in the first place
- All previous simulations are performed on the vhdl netlist, we don’t know if the layout corresponds to the vhdl that we are simulating
- Lvs ensure that it does the function
- It scans the layout, whenever it finds and intersection for diffusion or body for example it judges that there is a transistor, so it’ll map where the transistors are, which transistors are connected to which and form these into logic gates
- Then it’ll break down everything into logic equations, simplify these logic equations, so that it has equations that describes how the circuit functions
- Then it’ll take the reference vhdl netlist, simplify its logic equations
- Compare the result logic equations, if both match then lvs passes, if it doesn't then there is a problem, and lvs will tell you where the problems are
- Finally if all tests pass, then the design is ready for tapeout, send the files to the fab facility.
- usually design is sent in the form of GDS 2 files (graphical database system), simple format, text file has head of a certain format
- Bulk of the gds2 file is a set of vertices of where every block starts and ends, automatically generated from the layout (gds2 <--> 3d layout)


> *last modified 15/04/2020*