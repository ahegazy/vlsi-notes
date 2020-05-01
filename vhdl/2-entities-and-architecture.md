# Entities and architecture
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## design construction
- A vhdl design consists of at least two parts one entity and at least one architecture  

## Entity
- Entity describes that there is a building block 
   - It gives it a name
   - Defines the ports of this block (names, directions, sizes)
   - Does not indicate how this entity is implemented on the inside or how many times it’s used 
## Architecture
- Architecture describe how the entity is implemented on the inside
   - architecture arch-name of entity’s-name is 
      - Declaration of internal signals of the architecture, components used inside, constants used
   - Begin
      - Design implementation
   - End architecture

## Relationship between entity and architecture
- Entity could have multiple architectures, when entity is instantiated, we can specify which architecture to use
   - By default all instantiations will use the last declared architectures unless otherwise stated. 
