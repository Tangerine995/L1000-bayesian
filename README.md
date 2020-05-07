# L1000 peak deconvolution based on Bayesian analysis

## Overview
This project is intended to generate high quality perturbagen signatures from LINCS L1000 assay. We build a pipeline, in parallel with L1000 group, to process raw fluorescent intensity data into *z*-scores as perturbagen signatures. Pre-computed datasets covering a majority of LINCS L1000 Phase I and Phase II is available in [Downloads](#Downloads). 

Our pipeline is different from the L1000 pipeline mostly in the peak deconvolution algorithm. We implement our algorithm in both C++ and CUDA, which can be used with various languages. We give two [examples](/example) for how to use these functions with C++ natively and how to be called in *Wolfram Mathematica*. 

Also, we have prepared a small batch of real data and relavant code for you to test our pipeline at a very small scale. You may follow the [instructions](/pipeline), run the pipeline, and check the results.
 
## Datasets

### Summary
LINCS L1000 Phase I (GSE92742, small molecule treatments only) & Phase II (GSE70138) datasets generated by our pipeline are currently available. The datasets cover three levels: Our Level 4 and Level 5 data are equivalent to Level 4 and Level 5 data provided by L1000; the marginal distributions data of peak locations are similiar to L1000 Level 2 data, except that they are probability distributions instead of precise numbers of peak locations.

Unless you are interested in managing *z*-score inference and combination, we encourage you to use combined *z*-scores by bio-replicates (Level 5 data). 

### Downloads


| Description                               | Download                                      |
| ----------------------------------------- | --------------------------------------------- |
| Marginal distributions of peak locations  | [Bayesian_GSE70138_Level2_DPEAK.zip](http://callisto.astro.columbia.edu/files/L1000/Bayesian_GSE70138_Level2_DPEAK.zip)<br>[Bayesian_GSE92742_Level2_DPEAK.zip](http://callisto.astro.columbia.edu/files/L1000/Bayesian_GSE92742_Level2_DPEAK.zip)|
| Plate control *z*-scores                  | [Bayesian_GSE70138_Level4_ZSPC_n335465x978.h5](http://callisto.astro.columbia.edu/files/L1000/Bayesian_GSE70138_Level4_ZSPC_n335465x978.h5)<br>[Bayesian_GSE92742_Level4_ZSPC_n572534x978.h5](http://callisto.astro.columbia.edu/files/L1000/Bayesian_GSE92742_Level4_ZSPC_n572534x978.h5)|
| Combined *z*-scores by bio-replicates     | [Bayesian_GSE70138_Level5_COMPZ_n116218x978.h5](http://callisto.astro.columbia.edu/files/L1000/Bayesian_GSE70138_Level5_COMPZ_n116218x978.h5)<br>[Bayesian_GSE92742_Level5_COMPZ_n178984x978.h5](http://callisto.astro.columbia.edu/files/L1000/Bayesian_GSE92742_Level5_COMPZ_n178984x978.h5)|
| Checksum                                  | [Bayesian_L1000_sha512sum.txt](http://callisto.astro.columbia.edu/files/L1000/Bayesian_L1000_sha512sum.txt)|

The meta data are available from the publication by L1000 group: [GSE70138](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE70138) and [GSE92742](https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE92742). They include perturbagen and cell line information associated with signature and instance IDs in the datasets.

### Data stuctures

The *z*-score results (as HDF5) are compatible with those published by L1000 group. Each of them contains three datasets as follows:

* `/colid` are the signature IDs (Level 5) or instance IDs (Level 4);

* `/rowid` are the names of landmark genes;

* `/data` are the *z*-scores as a matrix.

Each marginal distribution file contain the information of peak locations on one plate. It contains four datasets as follows:

* `/colid` are the instance IDs;

* `/rowid` are the names of landmark genes;

* `/peakloc` are the locations of the peaks for calculating likelihood function;

* `/data` are encoded log-likelihoods as a rank-3 array of 16-bit unsigned integers. To retrieve the log-likelihoods, the values should be multiplied by a factor of `-0.001`. Note that they are not normalized. 

## Citation

Qiu, Yue, *et al.*, 2020, Bioinformatics, 36(9), 2787, [https://doi.org/10.1093/bioinformatics/btaa064](https://doi.org/10.1093/bioinformatics/btaa064)
