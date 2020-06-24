# Dynamic hazards
- Source [this](https://www.youtube.com/playlist?list=PLyWAP9QBe16qiSMkBcAnUMxFagLIJzmv1) playlist on Testing.

## Introduction
- As discussed in [glitches and logical hazards](17-glitches-logical-hazards.md) dynamic hazards happen when you make a transition
    - Dynamic 0-1: means you are in a stable `0` and going to stable `1` but you see a glitch happening while you make the transition
    - Dynamic 1-0: means you are in a stable `1` and going to stable `0` but you see a glitch happening while you make the transition

## Dynamic hazard detection
- K-map approach isn't feasible for dynamic hazards so we deal with this using expression analysis 
- [Static hazarad](18-19-20-21-22-static-hazards.md) is possible when the variable appears in true and inverted form
- Dynamic hazard is possible if the variable makes three appearances, and they have a true and inverted form 
- If a variable satisfy the previous condition, you look for
    - `BB'`: static 0 hazard
    - `B+B'`: static 1 hazard
    - `B(B+B')`: dynamic 1-0 hazard
    - `BB'+B`: dynamic 0-1 hazard
- Dynamic hazard implies an underlying static hazard

## Example
- Function `F=(A+B')(B+C)+BD+E`
- `A`,`C`,`D`, and `E` don't make multiple appearances, each of them has a single appearance at the output, so they aren't sources of any hazard
- `B` is a possible source of hazards
    - static hazard because it makes an appearance in true and inverted form
    - `B` appears three times, so it can cause dynamic hazard
- Bring any of the hazard structures by forcing the other variables to have specific values
- `A=0`, `C=0`, `D=1`, `E=0` with these inputs we make the three instances of `B` observable `F=B'B+B`
    - This is a structure connected to 0-1 dynamic hazard if `B` makes a transition while the four other inputs have these specific values
    - This can result in a dynamic hazard, and doesn't necessarily have to, it depends on the differential delays of the `B` and `B'`
- The function has other two static hazards related to input `B`, independent from the dynamic hazard and have to be addressed separately
    - static 1 hazard: `A=0`, `C=1`, `D=1`, `E=0`, this will make `F=B'+B`
    - static 0 hazard: `A=0`, `C=0`, `D=0`, `E=0`, this will make `F=B'B`
- Addressing the dynamic hazard doesn't address the static hazards because they occur of different input combinations of the other variables

### Addressing hazards
- The static 0 hazard can be addressed by multiplying `ANDing` the original expression by a new term `F1=F(A+C+D+E)`
    - the hazard happens when all the other variables equal to 0
    - multiplying `ANDing` them with the function won't change it and will force `F1=0` when `B` has a transition (regardless of `B`)
    - for any other combination of `A`,`C`,`D`,`E`, the new bracket equals to `1` and the function equals the original one `F1=F`
- The static 1 hazard can be addressed by adding `ORing` the original expression by a new term `F2=F+(A'CDE')`
    - This too doesn't change the original function
    - for any other combination of `A`,`C`,`D`,`E`, the function `F2=F`
- To solve both static hazads `F3=F(A+C+D+E)+(A'CDE')` the order doesn't matter
    - This gonna mask the static hazards but it won't erase the dynamic hazard
    - because the dynamic hazard happens because of different combination of inputs than the masked combination
- To remove the dynamic hazards we add a new term for the combination `A=0`, `C=0`, `D=1`, `E=0` that makes `F=B'B+B`
- The problem here is that if you form a prodcut term of this input combination `F != F+A'C'DE'` and added it to the function or formed a sum term `F != F(A+C+D'+E')` and multiplied it with the function this will chaneg the original function
    - So you need to suppress every underlying static hazard for its input combination
- We have `F=B'B+B` so we
    - suppress the underlying static hazard caused because `B'B`
        - by multiplying the brackets that originally had a `BB'` in the original function not the entire function
        - `F=(A+B')(B+C)(A+C+D'+E)+BD+E`

## Summary
- When you look at a logic function you try to obsereve which logic variables have more than one appearance in true and complement form
    - if they make two appearances then there is a possiblity of a static hazard
    - if they make three appearances then there is a possiblity of a dynamic hazard
- Then you try to extract one of the four forms
    - if you can extract more than one you have to extract and address all of them
    - because each of them represents a different kind of hazard
- for each of the observed hazards, you get the combination of the other variables that makes them appear at the output and that's the input combination that you are worried about
