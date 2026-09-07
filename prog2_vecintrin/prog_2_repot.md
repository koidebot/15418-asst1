# Problem 2: SIMD Vectorization

For `N = 10000`, I obtained:

| Vector Width | Vector Utilization | Total Vector Instructions |
|---:|---:|---:|
| 2 | 81.453621% | 411579 |
| 4 | 80.646024% | 211215 |
| 8 | 80.467434% | 106229 |
| 16 | 80.461103% | 53128 |
| 32 | 80.354358% | 26608 |

Vector utilization changes very little as vector width increases, decreasing from about 81.45% for width 2 to 80.35% for width 32. The decrease occurs because different lanes may have different exponents and therefore require different numbers of loop iterations. Once a lane finishes, it is masked off while other lanes continue executing. With wider vectors, there is a slightly greater chance of having lanes with different completion times. However, the utilization is not very sensitive to vector width.

The total number of vector instructions decreases approximately by a factor of two each time the vector width doubles. Since the input size is fixed, doubling the vector width approximately halves the number of vectors that must be processed. The number of loop iterations within each vector does not increase very much. 