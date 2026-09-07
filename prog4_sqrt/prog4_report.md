## Problem 4: Iterative Square Root

### Part 1: Baseline

For the default random input:

- ISPC speedup: 4.74x
- Task ISPC speedup: 34.45x

The speedup due to SIMD parallelization is 4.74x. The additional speedup from multi-core parallelization is approximately 7.27x.


### Part 2: Good Input

I modified `initGood()` so that every input value is `2.99999f`.

For this input:

- ISPC speedup: 6.91x
- Task ISPC speedup: 46.85x

Using identical difficult inputs improves SIMD performance because all lanes require approximately the same number of iterations. This minimizes divergence and keeps the SIMD lanes active together.

The SIMD speedup improved from 4.74x to 6.91x. The additional multi-core speedup was 6.79\times.

Therefore, the improvement primarily came from better SIMD utilization rather than improved multi-core scaling.

### Part 3: Bad Input

I modified `initBad()` so that one value in every group of eight is `2.99999f` and the other seven values are `1.0f`.

For this input:

- ISPC speedup: 0.96x
- Task ISPC speedup: 6.98x

This produces poor SIMD performance because one lane requires many iterations while the other seven lanes finish quickly. The completed lanes become inactive while the remaining lane continues executing.

The ISPC implementation is therefore slightly slower than the serial implementation. Multi-core execution still improves performance because different groups of work can still execute in parallel across multiple CPU cores.