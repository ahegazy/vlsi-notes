# attributes
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## Attributes
- Special way to extract information about named object (signal, component, instance, entity, data type) in vhdl
- Their position regarding synthesizability is a question
- Useful in scalable code with generics, and it’s a very good practice to do so
    - Ex. define a signal with a range same as another signal’s length s2 : std_logic_vector(s1’length-1 downto 0) 

## Attrbiutes Synthesizability 
- Some attributes are safe to use in a synthesizable code, others are not
   - Any attribute that creates a copy of a signal, or deals with time as an operand or as a return that attribute should never be used in the design, it should be used in the test bench to do testing
   - Attributes that act on data types or gives you information about data types are generally safe to use, if they are used in a correct way so that return a value that is constant at synthesis time they can be perfectly safe to use
   - Attributes that deals with entities and entities name are usually used in assert statements in order to aid in debugging, this means they’ll normally ignored in synthesis so it doesn’t matter if used or not
   - General advice, use attributes carefully 

## User defined attributes 
   - Only useful for very large designs involves many design teams that need to move information between designs and work libraries
