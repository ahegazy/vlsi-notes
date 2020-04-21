# Place and route
source: [this](https://www.youtube.com/watch?v=8w0UEjMBYT8&list=PLyWAP9QBe16qWQzq_IQtGKO9Yz8QvCWvY&index=13&t=0s) video from the series on ASIC design flow

## place and route
- Most critical step in the design flow
- After synthesis we have a netlist (vhdl file consists of structural connection of standrad cells from the library)

## Next steps
### Partitioning and floorplanning
- Partitioning: Is the design a singular unity or can be partitioned into blocks that are logically connected to each other
- Floorplanning: Once we have decided on the partitions we have the real state of the asic and we have to decide which areas of the asic are dedicated to which partitions
- Whether partitioning and floorplanning makes sense is related to the next step place and route

### Place and route --> Post PAR(place and route) simulation
- Take the layouts of each of the standard cells we are using, then place them in a specific locations in the asic, then route them to each other using metal layers so that we have the overall layout of the entire circuit
- Placement and routing are inseparable and iterative operations
- The tool function in constraints
    - Pin placement constraints
    - Aspect ratio of the chip
    - Area and speed
- Tool keeps trying (depending on the optimization effort specified) to find a layout that gives a positive or zero slacks on all paths to report closure
    - If the tools gives up it’ll report failure to close for the designer and It’ll report  all the slacks in the circuits specifically the -ve slacks
    - Outputs can be: closure, failure (setup time violations, -ve slacks).
    - If there is a hold time violation tool usually solve it by adding buffers
    - Failure can be solved by relaxing the constraints, increasing the optimization effort, revisiting the design 
- Post PAR simulation is bit-accurate, cycle-accurate, models gate delays and interconnect delays


> *last modified 15/04/2020*