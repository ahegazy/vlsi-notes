# Digital Design optimization
source: (Book) Digital Integrated Circuit Design From VLSI Architectures to CMOS Fabrication, chapter 2

## Merits of architectural alternatives
1. circuit size A. (area or circuit complexity [gate equivalent] )
2. cycles per data item Γ (# of computation cycles separates releasing/accepting 2 data items)
3. Longest path delay tlp (time required for data to propagate along longest ocmbinational path) (circuit should settle to steady state withing single computation period tcp ) (tlp <= tcp) (tcp=tclk)
4. Time per data item T (time between releasing 2 data items) (T = Γ * tcp)
5. Data throughput Θ = 1/T (data items process per time unit)
6. Size–time product AT (H.W resources spent to obtain certain throughput)
7. Latency L (number of computation cycles from a data item being entered into a circuit until the pertaining result becomes available at the output)
8. Energy per data item E (amount of energy dissipated in carrying out some given computation on a data item.)

## DDG (Data dependency graph)
1. Decomposing function f into a sequence of subfunctions that get executed
    - one after the other in order to reuse the same hardware as much as possible.
    - iterative decomposition: resource sharing through step-by-step execution. 
    - The computation of function f is broken up into a sequence of d subtasks which are carried out one after the other. (effective when f can be separated into multiple nearly identical subfunctions)
2. Pipelining of the functional unit for f to improve computation rate by cutting down combinational depth and by working on multiple consecutive data items simultaneously.
    - cutting combinational depth into several separate stages of approximately uniform computational delays by inserting registers in between, The combinational logic between two subsequent pipeline registers is designed and optimized to compute one
specific subfunction.
    - shimming registers: registers used to add latency in one of the pathes for multiple feedforward path pipelining
    - coarse grain: complex fn, better performance in pipelining - fine grain
3. Replicating the functional unit for f and having all units work concurrently.
    - Concurrency is obtained from providing q instances of identical functional units
for f and from having each of them process one out of q data items in a cyclic manner. To that end, two synchronous q-way switches distribute and recollect data at the chunk’s input and output respectively.
4. Time sharing: parallel data streams undrgo processing, pool hardware by having a single functional unit process     the parallel data streams one after the other in a cyclic manner (referred to as multiplexing or as resource sharing in the context of
circuit design)
5. Associativity transform (Algebric transform) depends on the operation

