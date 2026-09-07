## Problem 3

### Part 1: ISPC SIMD

For view 1, the serial implementation took 175.963 ms and the
single-core ISPC implementation took 37.023 ms, for a 4.75x speedup.

The theoretical maximum speedup is 8x because the
compiler generates 8-wide SIMD instructions. The actual speedup is
lower because pixels in a SIMD gang may require different numbers
of Mandelbrot iterations. When some pixels finish before others,
their SIMD lanes become inactive while the remaining lanes continue
executing. 

### Part 2: ISPC Tasks

| Tasks | Speedup vs. serial |
|---:|---:|
| 2 | 9.49x |
| 4 | 11.72x |
| 8 | 18.58x |
| 16 | 31.89x |
| 32 | 32.16x |
| 64 | 34.29x |

Increasing the number of tasks improves load balance. Different
regions of the Mandelbrot image require different amounts of
computation, so assigning only a few large regions can cause some
cores to finish while others remain busy. Creating more, smaller
tasks allows the ISPC runtime to distribute work more evenly among
the eight cores. Eventually the benefit decreases because the work
is already well balanced and additional tasks introduce scheduling
overhead.

The best task count I tested was 64, which achieved a 34.29x speedup
over the serial implementation.