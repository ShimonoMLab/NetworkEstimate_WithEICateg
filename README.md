# NetworkEstimate_WithEICateg

GPU-accelerated transfer entropy and sorted local transfer entropy analysis for estimating directed excitatory and inhibitory interactions in neuronal spike-train data.

This repository provides MATLAB/CUDA code associated with the following peer-reviewed article:

**Kajiwara, M., Nomura, R., Goetze, F., Kawabata, M., Isomura, Y., Akutsu, T., & Shimono, M. (2021). _Inhibitory neurons exhibit high controlling ability in the cortical microconnectome_. PLOS Computational Biology, 17(4), e1008846. https://doi.org/10.1371/journal.pcbi.1008846**

## Overview

This repository implements GPU-accelerated analysis code for estimating directed neuron-neuron interactions from spike-train data.

The main program, `ASDFTEslteKyotoCuda_mod.m`, calculates transfer entropy and sorted local transfer entropy to assess neuron-neuron interactions while taking into account a range of synaptic or interaction delays.

Transfer entropy is used to quantify directed causal coupling strengths among neurons. Sorted local transfer entropy is related to conventional transfer entropy but is designed to distinguish inhibitory influences from excitatory influences by applying a sorting procedure that accounts for the reversed signs of local transfer entropy values in excitatory and inhibitory interactions.

The code is designed for computational neuroscience studies involving:

- neuronal spike-train analysis
- transfer entropy
- local transfer entropy
- sorted local transfer entropy
- directed functional connectivity
- excitatory and inhibitory neuronal interactions
- cortical microconnectome analysis
- GPU-accelerated neural data analysis
- MATLAB/CUDA-based computational neuroscience

## Scientific Background

This repository supports the analysis framework used in:

**Kajiwara et al. (2021), _Inhibitory neurons exhibit high controlling ability in the cortical microconnectome_, PLOS Computational Biology.**

In that study, transfer entropy and sorted local transfer entropy were used to estimate directed interactions among cortical neurons and to analyze the distinct roles of excitatory and inhibitory neurons in cortical microconnectome organization and control.

This code is particularly relevant for researchers who want to estimate directed interaction matrices from neuronal spike trains and then analyze how excitatory and inhibitory neuronal populations contribute to network structure and controllability.

## Description

`ASDFTEslteKyotoCuda_mod.m` calculates transfer entropy and sorted local transfer entropy to assess neuron-neuron interactions, taking into account a specified range of delays.

Transfer entropy quantifies directed causal coupling strengths among neurons.

Sorted local transfer entropy is similar to conventional transfer entropy but is designed to distinguish inhibitory influences and excitatory influences by using a sorting method that takes into account the reversed signs of local transfer entropy values for excitatory and inhibitory interactions.

`ASDFTEslteKyotoCuda_mod.m` is a MATLAB script that loads the CUDA-related files:

- `TransentPTXSLTEslte.cu`
- `transentPTXSLTEslte.ptx`

Therefore, after compiling `TransentPTXSLTEslte.cu` with MATLAB, run `ASDFTEslteKyotoCuda_mod.m` in MATLAB.

The CUDA code calculates transfer entropy for a fixed delay, while `ASDFTEslteKyotoCuda_mod.m` calculates transfer entropy and sorted local transfer entropy across multiple delays.

The GPU code is expected to be approximately 10–100 times faster than comparable CPU-based implementations, depending on the computational environment and dataset size.

## Requirements

The following software and hardware are required:

- MATLAB
- CUDA
- CUDA-compatible GPU
- MATLAB Parallel Computing Toolbox or an equivalent environment supporting `mexcuda`
- A basic compiler environment for compiling CUDA code with MATLAB

For example, refer to MATLAB documentation for `mexcuda`:

https://jp.mathworks.com/help/parallel-computing/mexcuda.html

## Files

The main files include:

- `ASDFTEslteKyotoCuda_mod.m`  
  Main MATLAB function for calculating transfer entropy and sorted local transfer entropy across multiple delays.

- `TransentPTXSLTEslte.cu`  
  CUDA source code used for GPU-accelerated transfer entropy calculation.

- `transentPTXSLTEslte.ptx`  
  Compiled PTX file used by the MATLAB code.

- `spike.mat`  
  Example spike-train data file.

## Setup

Compile the CUDA file in MATLAB using:

    mexcuda ./TransentPTXSLTEslte.cu

After successful compilation, run the main MATLAB code.

## Usage

Load the example spike-train data and run the analysis as follows:

    load ./spike.mat
    delay0 = [1:30];
    [peakTE, SLTEdelays, TEdelays, delayindex, CI, peakTE_all] = ASDFTEslteKyotoCuda_mod(spikes, delay0, 1);

## Inputs of the Main Code

Main function:

    ASDFTEslteKyotoCuda_mod

Inputs:

- `spikes`  
  Spike-train data in structure form.  
  The last line expresses the data size, and the second line from the last one is blank.

- `delay0`  
  The delays of the postsynaptic neuron used for the analysis.  
  The default delay range is typically 1–30 ms.

## Example Data

Example file:

    spike.mat

This is a MATLAB data file containing neuronal spike sequences.

Expected data structure:

- Size of data: `(1, N+2)`
- `N` is the number of neurons.
- Components `1` to `N`: spike data, represented as timestamps of spike events.
- Component `N+2`: number of cortical neurons and maximum time step.
- Time bin size: 1 ms.

## Outputs of the Main Code

The main function returns:

- `TEdelays`  
  Transfer entropy values depending on delay, typically 1–30 ms, between spikes of presynaptic neurons and spikes of postsynaptic neurons.

- `SLTEdelays`  
  Sorted local transfer entropy values depending on delay, typically 1–30 ms, between spikes of presynaptic neurons and spikes of postsynaptic neurons.

- `peakTE`  
  Transfer entropy values at the peak point, namely the maximum point of the previously calculated `TEdelays`.

- `delayindex`  
  The bin index corresponding to the delay at which transfer entropy showed the peak value.

- `CI`  
  Coincidence Index values calculated from `TEdelays`.

- `peakTE_all`  
  Transfer entropy values only within five bins after their peak points.

## Recommended Use

This repository is intended for researchers who want to:

1. Estimate directed interactions from neuronal spike-train data.
2. Apply transfer entropy to cortical microconnectome analysis.
3. Distinguish excitatory and inhibitory influences using sorted local transfer entropy.
4. Reproduce or extend analyses related to Kajiwara et al. (2021).
5. Use GPU-accelerated MATLAB/CUDA code for large-scale neuronal interaction estimation.
6. Compare other directed connectivity estimation methods against transfer entropy-based approaches.
7. Use transfer entropy matrices for downstream network analysis, graph analysis, or controllability analysis.

## Citation

If you use this repository in any way that contributes to a publication, preprint, thesis, presentation, software tool, benchmark comparison, dataset analysis, or derivative codebase, please cite the peer-reviewed article below.

Please cite the paper if you:

- use the original code
- modify or extend the code
- reuse part of the code in another project
- use this repository as a benchmark
- compare another method against this implementation
- use the transfer entropy analysis workflow
- use the sorted local transfer entropy analysis workflow
- use the estimated interaction matrices for downstream network analysis
- build upon the excitatory/inhibitory interaction estimation procedure
- use this work as a reference for cortical microconnectome analysis, transfer entropy, sorted local transfer entropy, or GPU-accelerated spike-train analysis

### Peer-reviewed article

For the scientific method, results, analysis framework, and interpretation, please cite:

**Kajiwara, M., Nomura, R., Goetze, F., Kawabata, M., Isomura, Y., Akutsu, T., & Shimono, M. (2021). _Inhibitory neurons exhibit high controlling ability in the cortical microconnectome_. PLOS Computational Biology, 17(4), e1008846. https://doi.org/10.1371/journal.pcbi.1008846**

### Archived software release

If this repository is archived on Zenodo, please also cite the Zenodo software release when referring specifically to the archived software implementation.

**Kajiwara, M., Nomura, R., Goetze, F., Kawabata, M., Isomura, Y., Akutsu, T., & Shimono, M. (2026). _NetworkEstimate_WithEICateg: GPU-Accelerated Transfer Entropy and Sorted Local Transfer Entropy Analysis for Excitatory and Inhibitory Neuronal Interactions_. Zenodo. https://doi.org/10.5281/zenodo.xxxxxxxx**

Replace `10.5281/zenodo.xxxxxxxx` with the actual Zenodo DOI after creating the GitHub release and Zenodo archive.

### Short citation request

If you use this code, modify this code, reuse part of the implementation, compare another method against this implementation, or use the transfer entropy / sorted local transfer entropy workflow in your own study, please cite:

**Kajiwara, M., Nomura, R., Goetze, F., Kawabata, M., Isomura, Y., Akutsu, T., & Shimono, M. (2021). _Inhibitory neurons exhibit high controlling ability in the cortical microconnectome_. PLOS Computational Biology, 17(4), e1008846. https://doi.org/10.1371/journal.pcbi.1008846**

### Suggested citation sentence

This repository implements GPU-accelerated transfer entropy and sorted local transfer entropy analysis for estimating directed excitatory and inhibitory interactions in neuronal spike-train data.

## Related Repository

A related repository for reproducing network quantification analyses associated with Kajiwara et al. (2021) is available here:

https://github.com/ShimonoMLab/Network-Quantifications

## Related Paper

PLOS Computational Biology article page:

https://journals.plos.org/ploscompbiol/article?id=10.1371/journal.pcbi.1008846

## Contact

For detailed questions about the code, data format, reproduction procedure, CUDA/MATLAB setup, or possible extensions, please contact the corresponding authors of the associated paper or the repository maintainer.
