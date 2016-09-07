# R-BLAS
Notes on OpenBLAS installation for R and matrix multiplications

# OpenBLAS installation

Refer Bryan Lewis's post: http://illposed.net/r-on-linux.html

**Note**: For Ubuntu 14.04, the LAPACK replacement (i.e. `sudo ln -s /usr/lib/openblas-base/libblas.so.3 libRlapack.so
`) does not work. 

## Testing OpenBLAS installation and linking

The following code runs in 30-60 seconds on a 16 core, 112 GB RAM machine after correct linking and setting `OMP_NUM_THREADS=16` via `~/.bashrc` or invoking R as `OMP_NUM_THREADS=16 R`

```
Sys.getenv("OMP_NUM_THREADS")
# [1] "16"
a = matrix(runif(500*27000),500)
b = matrix(runif(500*117000),500)
a = sweep(a,2,colMeans(a),`-`)
b = sweep(b,2,colMeans(b),`-`)
a = sweep(a,2,apply(a,2,function(x) drop(sqrt(crossprod(x)))),`/`)
b = sweep(b,2,apply(b,2,function(x) drop(sqrt(crossprod(x)))),`/`)
gc()
#            used  (Mb) gc trigger   (Mb)  max used   (Mb)
# Ncells   216889  11.6     460000   24.6    460000   24.6
# Vcells 72402044 552.4  238313162 1818.2 248019880 1892.3
t1=proc.time()
x = t(a) %*% b
proc.time()-t1
#    user  system elapsed
# 424.792  35.104  29.184
#   user  system elapsed
```
