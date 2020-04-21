# Configurations in vhdl 
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## Configuration
- Vhdl elements that can be used in large designs two manage how components are used within the design 
- Used to manage how components are used as an instance within another component 
- Instance architecture binding in a centralized location in a structural way
- Rename the ports of a component before they are used within another design

### Configuration example
- Here we have a big entity called big_adder, and it has two architectures struct and behavioural, each one instantiation other entities
- Architecture struct instantiates arch11 from entity Ent1 and arch23 from Ent2
- Architecture behav instantiates arch31 from Ent3 two times and arch32 from Ent3 one time
  

- Configuration can be used hierarchically to describe instantiations on a hierarchical basis 
  

- Adapt the port name of a component before it’s used in another design
- It doesn’t declare a specific signal connection in the architecture, it’s renaming the ports of the entity 
  

- Configuration in small designs are generally not that good, they do help in very large designs which is managed by many different people, or when you use other code that doesn’t follow some convention you are using and need to rename port names
