- performance comparison between CUDA vs OpenCL
- most applications CUDA at most is 30% better
- OpenCL portability does not fundamentally affect performance


To investigate the performance vs portability trade-off of OpenCL, we make
extensive investigations and experiments with diverse applications ranging from
syntheic ones to real world ones, and we observe the performance idfferences
between CUDA and OpenCL. In particular, we give a detailed analysis of the
performance differences and then conclude that udner a fair comparison, the two
programming models are equivalent.

    Cuda Terminology    |       OpenCL Terminology
    ----------------------------------------------
    Global Memory       |       Global Memory
    Constant Memory     |       Constant Memory
    Shared Memory       |       Local Memory
    Local Memory        |       Private Memory
    Thread              |       Work-item
    Thread-block        |       Work-group


    Performance Ratio = OpenCL Performance / CUDA Performance
