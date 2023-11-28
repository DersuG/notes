# GPU
- **Compute Unified Device Architecture (CUDA):**
    - parallel computing platform and api model
    - programmers can program the GPU
    - widely used programming model for GPU computing
- integrated into modern CPUs
- **FLOPS:** performance unit
    - floating point operations per second
    - MFLOPS (10^6)
    - GFLOPS (10^9)
    - TFLOPS (10^12)
    - PFLOPS (10^15)
    - theoretical peak performance: `cps = vector_ops_per_second * num_cores`

## NVIDIA GPUs
- Tesla V100 (Volta) (2017)
    - 5120 INT cores
    - 5120 FP32 cores
    - 2560 FP64 cores
    - 640 tensor cores
    - 80 SMs (streaming multiprocessors), each with:
        - 64 INT cores
        - 64 FP32 cores
        - 32 FP64 cores
        - 8 tensor cores
        - L1 cache + shared memory
        - register file
        - within each SM, cores share PC, IR, and warp scheduler
            - **warp:** group of 32 threads

## Manycore GPU architecture
- each core has independent ALU
- cores share control logic units (PC, IR, scheduler, etc.)
- **SIMT:** single instruction, multiple threads
- smaller caches
- simpler control
    - no branch prediction
    - no data forwarding
    - shared control logic units
- cheap ALUs
- have their own device memory (unlike CPU which has to use the host's memory)
- no OS support (e.g. virtual memory)

## GPU programming
- divide program into GPU part (for parallel execution) and CPU part (for sequential execution)
- move input data to GPU device memory (H2D)
- execute
- move results back to host memory (D2H)

## Program execution on GPU
- program runs in hybrid mode on CPU and GPU
    - starts on CPU
    - launch GPU kernels to run parallel GPU code with multiple threads
    - return to CPU to execute sequential code