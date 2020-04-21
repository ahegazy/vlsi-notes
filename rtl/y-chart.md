# Y-chart and RTL tips
source: [this](https://www.youtube.com/watch?v=voi0oZI5Tug&list=PLFhizsGPFKt8gz-bYlKMDCgBKwxMc33H2&index=20&t=0s) video
 
## Gajski-Kuhn chart

![y-chart](imgs/y-chart/y-chart.png)

![rtl-vs-behav-model](imgs/y-chart/rtl-vs-behav-model.png)

## Linting
- After writing RTL code, run a linting tool on the code to check for RTL style checks
- Synopsys spyGlass, Mentor Autocheck, questasim

![linting](imgs/y-chart/linting.png)

## Points to check in RTL code

![checklist-of-rtl-code](imgs/y-chart/checklist-of-rtl-code.png)

## Blocking and non blocking assignments
- DONOT MIX BLOCKING AND NON BLOCKING WITHIN SAME ALWAYS BLOCK

![blocking-non-blocking](imgs/y-chart/blocking-non-blocking.png)

## COMB_LOOP 
- feedback in combinational logic

![comb-loop](imgs/y-chart/comb-loop.png)

## INFERRED LATCHES (implicit)
- Latches makes problems in timing constraints

![implicit-latches](imgs/y-chart/implicit-latches.png)

## Multiple Drivers

![multiple-drivers](imgs/y-chart/multiple-drivers.png)

## FSM DEADLOCK
- Fsm is stuck in one state

![fsm-deadlock](imgs/y-chart/fsm-deadlock.png)
