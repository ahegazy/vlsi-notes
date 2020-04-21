# Constants and generics
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## Constants 
   - Constants same as programming languages constant, declared in the architecture using constant keyword, can be used to declare other constants or signals, widths. it’s a good practice because they help centralize numbers

## Generics
   - generics used exactly the same way as constants, they are labels that have constant numbers attached to them, the only difference is that generic is declared in the entity not in the architecture.
   - Generics can be used in the port declaration and desgin’s architecture
   - Generic map is optional in instantiation, you can declare some generics and leave others to get the default value, if there is no default value > error

## Difference between them
   - Difference between constants and generics is that generics can be left without a default value, constants must have a value.
   - Good practice in layered design is to use generics for all layers’ instantiations except the top layer you assign a value to this top layer’s generics cause this gives you scalable designs
