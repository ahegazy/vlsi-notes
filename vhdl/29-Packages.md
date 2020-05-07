# Packages in VHDL 
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## Packages
- Packages are collections of named items that can be reused across multiple designs across the same work library
- Collections of named items: types, subtypes, functions, procedures, components, attributes, constants, files and so on
- Can be used to centralize the location where any of these things are declared
   - Using package: use work.my_synth_package.all 

```
package my_synth_package is
    constant bus_width : integer := 16;
    component full_add is
        port (a, b, cin: in std_logic;
            s, cout : out std_logic);
    type state_machine_type is (reset, wait, operate);
end my_synth_package;
```
### Hardware cost
- Declare a package or declare use of it does not have an impact on resource usage unless you actually call it in a design
- Packages are synthesizable if all of its constituents are synthesizable so it’s generally useful to split your packages to synthesizable packages and unsynthesizable
- Packages can be separated into package declaration and package body, everything within the package must be declared in the declaration section, but things can be relegated to the body of the package like constants values, or function declarations
  
```
package my_synth_package is
    constant bus_width : integer;
    component full_add is
        port (a, b, cin: in std_logic;
            s, cout : out std_logic);

    function linear_combine(combine_vector: in std_logic_vector (2 downto 0));
        return std_logic;
end my_synth_package;

package body my_synth_package is
    constant bus_width : integer := 16;
    function linear_combine(combine_vector: in std_logic_vector (2 downto 0));
        return std_logic is
    begin
        return (combine_vector(0) xor combine_vector(1) xor combine_vector(2));
    end;

end my_synth_package;

```

