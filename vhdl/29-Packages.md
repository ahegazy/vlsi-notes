# Packages in VHDL 
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## Packages
- Packages are collections of named items that can be reused across multiple designs across the same work library
- Collections of named items: types, subtypes, functions, procedures, components, attributes, constants, files and so on
- Can be used to centralize the location where any of these things are declared
   - Using package: use work.my_synth_package.all 

### Hardware cost
- Declare a package or declare use of it does not have an impact on resource usage unless you actually call it in a design
- Packages are synthesizable if all of its constituents are synthesizable so it’s generally useful to split your packages to synthesizable packages and unsynthesizable
- Packages can be separated into package declaration and package body, everything within the package must be declared in the declaration section, but things can be relegated to the body of the package like constants values, or function declarations 
  
