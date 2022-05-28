# Five Point Stencil - Performance Engineering  

## Graph depicting running time for various strategies
![Time vs Strategies](https://user-images.githubusercontent.com/92354970/170807848-f3e79103-0046-4ba3-bf89-07749f49a96f.png)

### System Details
```
Number of CPUs = 12
Model name     = Intel(R) Core(TM) i7-8700K CPU @ 3.70GHz
```
### Parameters
```
Time Steps = 1024
Input Size = 1024 x 1024
Substeps = 32
Number of threads = 8
Block size        = 8 x 8
SIMD              = __mm256*
```

## Brief description of strategies
With above figure as reference
All strategies described below are in spatial domain (2-D)
1. Naive Implementation: Source --> Sample code
2. Blocking:
   a. Reordered index to row-major form
   b. Compute output in 2-D 8 x 8 chunks
3. Blocking + SIMD(\_\_mm256*)
   a. Implement 2(above)
   b. Replace 8 x 8 blocks (not near the borders) by SIMD related instructions (_step_opt_block()_)
4. Multithreaded + Blocking
   a. Each matrix is split into blocks (depending on num_threads) in both x and y direction.
      This is followed by each warp (group of num_threads executing row(x)-direction processing in one-go)
5. Cache-oblivious / Recursive
   a. Each matrix is split into 4-blocks recursively. 
   b. Each sub-unit with size equal to 8x8 is processed by SIMD-accelerated block
   c. Each sub-unit with size less than 8x8 is processed by naive block
6. SIMD + Cache-oblivious + Multithreaded
   

## Future Work
Perform blocking in temporal domain in addition to spatial (2-D) domain
