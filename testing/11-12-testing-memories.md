# Testing memories
- Source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16qiSMkBcAnUMxFagLIJzmv1) playlist on Testing.

## Differences between testing memory chip and CLB
- There are significant differences which has to do with how memories are designed
- memories are designed with one thing in mind desity
    - we want memories to be very dense inorder to store alot of data in small area
    - ofcourse speed is very important for memories but density is also on the top
- for logic density is usually not a huge concern, area is cheap and power and speed seems to rule supreme
- because memories are designed with a very different settings in mind we need a different testing approach

## Memories testing
- memories are very simple in terms of logic, a specific data memory only has to do two things
    - store a value
    - read that value from the storage location
- for testing memories we only have to do two things, write zero in a location then read it, write one in a location and read it
- That's very simple, what have been going against them is thier desnsity, because they are packed closely together, and there is alot of them and you need to test all of them
    - and the fact that they are packed together means that sometimes they tend to affect each other
    - so you can't test a location in isolation because maybe once you activate the other cells around it it might fail
- There is an additional problem with memories that affects drams in particular
    - They tend to have very specialized fabrication techniques focused on increasing the storage capacitor while mainting the desntiy of the dram cell
    - These techniques might result in failures that are peculiar to them only and thus requires specific testing techniques
    - Dram also seems to have failures that occur over time just because the value is stored dynamically (on a capacitor)

- A memory chip that contains millions of storage locations has a high possiblity of having a faulty location
    - if we insist that every memory chip used is 100% functional we might end up with a zero yield process
    - instead memory testing techniques tend to focus on identifying specific cells which have faults and isolating these cells

## Memories faults
- faults are ways to represent defects
- the same applies to memories as it did to logic
    - means that these fault models doesn't necessarily reflect something specific that happened in the physical level
- Types
    - Stuck at fault
    - transition fault
    - coupling fault
    - NPSF (neighbor pattern specific fault)

### Stuck at fault
- Exactly the same as the [stuck at fault](5-stuck-at-fault-model.md) in logic
- there is a specific cell and it's either gonna be normal or stuck at one or stuck at zero

#### Memory march
- Memory in general are tested using an approach called memory march
- Memory march means that we apply a number of steps to test a specific cell in the memory and we march to the next address and test that cell as well and keep doing that until we reach the end of the memory
- We are marching through the memory by incrementing the address when we finish testing a specific cell
- when you find that a specific cell is fault you don't through out the entire chip, you can actually deal with a few bits being off

#### Testing a stuck at fault
- you apply a zero to a certain cell and then read it
- then you apply a one then read it
- if both got the same result (both resulted in zero or one) then it's stuck at that result
- otherwise it's functioning normally, and it doesn't suffer from stuck at faults

### Transition faults
- The cell can have one or a zero written to it
- but the fault is that it refuses to make a transition
    - it might refuse to make a transition from one to zero or vice versa
- A one-to-zero transition fault or a zero-to-one transition fault
- This is different from stuck at fault
    - The cell can have a value of one or zero written to it
    - but what it refuses to do is transitioning from either of them
- While testing that a cell can have a one written and read from it and zero can rule out stuck at fault, it doesn't rule out transition faults
- Assume that a cell contains an initial value of `1`,
    - you then apply writing and reading `1` and it passes
    - then apply writing and reading of `0` and it passes
    - this proves that this cell doesn't have a stuck at `1` or `0` fault
    - however this cell could have a zero-to-one transition fault because you never tried to have the cell transition from containing a `0` to containing a `1`  this is when the fault is exposed
- To prove that a cell doesn't have a transition fault
    - zero-to-one you write a `0`, then write a `1` then read the `1`
    - one-to-zero you write a `1`, then write a `0` then read the `0`
    - this test proves that this cell can have a transition from one value ot the other and also proves that the cell doesn't have a stuck at fault

### Coupling fault
- A fault that involve more than one memory location at a time
    - Two cells coupled together, their values are affected by each other
- Types
    - Bridging fault
    - Inversion fault
    - Idempotent fault

#### Briding faults
- Two cells are bridged to each other (shorted to each other)   
    - both contain the same value, if you change one the other changes too
    - This doesn't mean there is a physical short circuit between them (at the defect level) but it means there is a defect that causes them to be bridged
- This doesn't mean we have to throw away both locations, but it means that you have lost at least one of them

#### Inversion faults
- Directional fault (the effect goes only in one direction)
    - one cell has an impact on the other but not vice versa
    - The first cell will pass the tests, but the fault will appear at the other one
- A transition in one cell triggers a transition on the other
    - If the first cell goes from `0` to `1`, the other cell will go from whatever it had to the opposite

#### Idempotent faults
- like inversion faults, they are also affect two cells and they are also directional
- A transition in one of the cells will set the value of the other cell
- There are four possible combination
    - `0 -> 1` causes the other cell to go to `0`
    - `0 -> 1` causes the other cell to go to `1`
    - `1 -> 0` causes the other cell to go to `0`
    - `1 -> 0` causes the other cell to go to `1`

#### Testing coupling fautls
- Memory marches are not very useful in detecting these faults
- if we have a `0 -> 1` `0` idempotent fault
    - to test it we write `0` to the first cell and `1` to the second
    - then write `1` to the first, and read the second cell
    - if we read `1` from the second cell then this specific fault isn't present
    - if we read a `0` from the second cell, this doesn't mean that this specific fault exist, it means that there is a fault, not necessarily this specific fault cause it might be an inversion fault
    - to tell that it's this idempotent fault we write a `0` to first cell, `0` to the second, then `1` to the second
    - if we read a `0` from the second, then this means the second cell location hasn't made a transition in this case, then this means there is this specific idempotent fault
    - if we read a `1` from the second cell, then this is an inversion fault
- The test becomes much more complicated when it involves two cells

### NPSF (neighbor pattern specific fault)
- Complicated and very challanging to detect because they involve alot of cells simultaneously  
- The most common and most practical faults in memories
    - if a fault occurs in a memory it's likely to be an NPSF
- A specific cell in the memory could be faulty because the effect of its neighbors (its four direct neighbors or eight neighbors)
- Divided into two categories
    - Active NPSF
    - Passive NPSF

#### Passive NPSF
- the fault observed on a cell is dependant on a pattern along its neighbors
- When the eight neighbors have a specific pattern, the cell behaves erroneously (could be stuck to `1`,`0`, toggle, it could cause it to do anything other than working proberly)
- this could be considered a conditional fault
- To test this you need to test (all previous tests) with all patterns of the surronding, you need to repeat this `2^8` times

| 0 | 1 | 1 |
|---|---|---|
| 1 | x | 0 |
| 1 | 1 | 0 |

#### Active NPSF
- Occurs when a pattern in the neighboring cells and a transition in one of them causes erroneous behavior in the target cell
- For example when the surrounding cells having the pattern `0001` and one cell transition `0011` a fault occurs
- This is similar to idempotent fault between the transitioned cell and the target cell, but it's conditional upon the other cells having this specific pattern

|   | 0 |   |     |   | 0 |   |
|---|---|---|-----|---|---|---|
| 0 | x | 0 |  -> | 0 | x | 1 |
|   | 1 |   |     |   | 1 |   |













