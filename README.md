# Performing admixture projection analyses in coordinate based windows

## Introduction
The following pipeline can be used to preform Admixture projection analyses in sliding windows. These were developed for the Darwin's finches project to assess population structuring within a dataset with un-even sampling between focal species. The pipeline is developed for use with two species/population groupings: here we use Daphne scandens and Daphne fortis.

The main script "WindowedAdmix.sh" consists of two main components:

1. An initial admixture run using an equal number of samples per species.
2. A second admixture run on the full target samples using the allele frequencies learned during step 1.

## Set up environment
To run the pipeline, the following conda environment needs to be set up:
```
conda create -n WindowedAdmix
conda activate WindowedAdmix
conda install -c bioconda admixture bcftools vcftools plink
```

Note: R will also need to be installed, along with the following packages:
- dplyr
- pophelper
- ggplot2
- ggthemes

## Running the pipeline
To perform windowed admixture analysis submit script along with the following parameters:
1. name of focal chromosome (as appears in VCF)
2. input VCF file
3. training sample list with equal numbers of species A and species B
4. target sample info specifying: sampleID  Island  Species
5. window size in bp
6. step size in bp
7. suffix for output directory name

An example submission, where we use an equal number of pre1984 scandens and fortis to train admixture, looks like this:
```
sbatch scripts/WindowedAdmix.sh \
chr1A Input.vcf \
31S_31F_to1983.txt 1984-2012_scandens_fortis.txt \
500000 500000 \
Daphne_Sc_Fo_1984to2012
```
Note: currently only windows of length "window size" will be outputted, meaning that trailing SNPs at the end of the focal chromosome will not be included in the analysis. To include these comment out lines 25-26 of "getWindowRanges.R" script:
```
#window_start <- window_start[which(window_stop < chrom_length)]
#window_stop <- window_stop[which(window_stop < chrom_length)]
```

## Plotting results
There are two plotting scripts that be used to visualise results:
1. plot_windowedAdmix.R - best for large windows
2. plot_windowedAdmix_smoothed.R - best for small windows

Examples outputs are shown below:
```
Rscript scripts/plot_windowedAdmix.R \
chr1A scandens fortis
```

```
Rscript scripts/plot_windowedAdmix.R \
chr1A scandens fortis 200000
```
