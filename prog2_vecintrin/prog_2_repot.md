# Problem 2: SIMD Vectorization

For `N = 10000`, I obtained:

| Vector Width | Vector Utilization | Total Vector Instructions |
|---:|---:|---:|
| 2 | 81.453621% | 411579 |
| 4 | 80.646024% | 211215 |
| 8 | 80.467434% | 106229 |
| 16 | 80.461103% | 53128 |
| 32 | 80.354358% | 26608 |

Vector utilization changes very little as vector width increases, decreasing from about 81.45% for width 2 to 80.35% for width 32. This decrease occurs because different lanes may have different exponents and therefore require different numbers of loop iterations. Once a lane finishes, it is masked off while the remaining active lanes continue executing, which lowers utilization. Wider vectors make it slightly more likely that a vector contains lanes with different completion times. 

Utilization decreases by  about 1.1% across vector widths, so utilization is not very sensitive to vector width for this workload.

Since the input size is fixed, doubling the vector width approximately halves the number of vectors that must be processed. Wider vectors may contain some lanes that finish earlier than others, so a few lanes can become inactive while the remaining lanes continue. However, this does not increase the number of loop iterations enough to offset the benefit of processing more elements per instruction. Thus the total vector instruction count still decreases by roughly \(1/W\).