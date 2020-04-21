# File I/O in VHDL
source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16p2HXVcyEgGAFicXJI797jK) playlist on VHDL design.

## File access 
- File access is used to 
- Load test data and dump test results in testbench
- Load large roms 

## File access in vhdl
- To use files you have to declare using two packages 
- std.textio declares two data types file and line types, and four functions read, write, readline, writelines
- Std_logic_textio redefines the functions to accept arguments of std_logic data type instead of bit data type

### writing to a file
- Writing to a file is a two step process
- Write the signal to a line
- Write the line to the file
- Because lines in a file can be of variable length

### Reading from a file  
- Read a line of data
- Access the value from the line you read

## approaches in testing
   - Reading the test vector from a file, asserting the results, dumping the results in a file
   - Reading the test vector from a file, dumping the output to a file, analyze the file in a high level simulator 
  
  

## Reading and writing to memories
   - Declare a function that reads the memory contents from a file and insert it in a variable
   - It takes zero time to initialize the variables in the ram at the beginning of simulation]
   - To use the function in testbench to initialize the values of the ram we call the function and assign its values to a signals
  
