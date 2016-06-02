
> PePr v1.1.10 
> Timestamp: 06-01-2016 Yanxiao Zhang zhangyx.shawn@gmail.com
------------------------------------------------------------

###Introduction
PePr is a ChIP-Seq Peak-calling and Prioritization pipeline 
that uses a sliding window approach and models read counts across 
replicates and between groups with a negative binomial distribution. 
PePr empirically estimates the optimal shift/fragment size and 
sliding window width, and estimates dispersion from the local genomic
area. Regions with less variability across replicates are ranked more
favorably than regions with greater variability. Optional 
post-processing steps are also made available to filter out peaks
not exhibiting the expected shift size and/or to narrow the width of peaks.

### Installation
1. Make sure your python version is higher than 2.6. Version 3.X may not be fully supported yet.
2. Install **pip** in your system if you don't have it. 
3. `pip install PePr` or `pip install PePr --user`(if you don't have administrator privilege). Optionally, you can download tarball (PePr-[version].tar.gz) from [github](https://github.com/shawnzhangyx/PePr/) and install using `pip install PePr-[version].tar.gz`
4. If installation is successful, you could directly invoke the script by typing `PePr`. A help message will show up. 

### Supported File Formats
* Single-end: BED, BAM, SAM. 
* Paired-end: BAM, SAM. The files must be sorted by the read names. Users can use `samtools sort -n sample.bam sample.sorted_by_name` to sort the file. 

### Basic Usage Examples
*Warning: These are working examples with minimal required parameters. For the best performance (or to avoid bad fitting) on your data, please read the manual carefully and choose the right parameters.* 
* For peak-calling, run: `PePr -c chip_rep1.bam,chip_rep2.bam -i input_rep1.bam,input_rep2.bam -f bam`
* For differential binding analysis with input samples, run: `PePr -c chip1_rep1.bam,chip1_rep2.bam -i input1_rep1.bam,input1_rep2.bam --chip2 chip2_rep1.bam,chip2_rep2.bam --input2 input2_rep2.bam,input2_rep2.bam -f bam`
* For differential binding analysis without input samples, run: `PePr -c chip1_rep1.bam,chip1_rep2.bam --chip2 chip2_rep1.bam,chip2_rep2.bam -f bam`

### Parameters
* *-h, --help*
: Show this help message and exit.
* *-i INPUT1*
: Group 1 input files. *REQUIRED.* Multiple file names are
    separated by comma,e.g. input1.bed,input2.bed
* *-c CHIP1*
:  ChIP-Seq chip group 1 files. *REQUIRED.* Multiple file names are
    separated by comma, e.g. chip1.bed,chip2.bed
* *--input2 INPUT2*
:    Group 2 input files. Used in differential binding analysis.
* *--chip2 CHIP2* 
:    Group 2 ChIP-Seq files. Used in differential binding analysis.
* *-n NAME*
:    Experiment name. DEFAULT: "NA"
* *-f FORMAT*
:    File format. *REQUIRED.* (currently support bed, sam, bam)
* *-s SHIFTSIZE*
:    Half fragment size. The number of bases to shift
    forward and reverse strand reads toward each other.
    If not specified by user, PePr will empirically 
    estimate this number from the data. A separate shift 
    size will be estimated for each ChIP file. 
* *-w WINDOWSIZE*
:    Window size. If not specified by user, PePr will 
    estimate this from the data.
* *--diff*
:   Whether to perform differential binding analysis.
    If so, please provide chip2 and input2 samples as well.
* *--threshold THRESHOLD*
 :   Significance cutoff for p-value DEFAULT:1e-5
* *--peaktype PEAKTYPE*
:    Is peak shape sharp or broad?(sharp, broad)
* *--custom_normalization NORMALIZATION*
:   Default: Yes. You can disable PePr's default normalization
    method by specifying 'NO'. Or you can provide your own
    Normalization constants separated by comma. 
* *--num-processors=NUM_PROCS*
:   Number of CPUs use on it.
* *--input-directory=INPUT_DIRECTORY*
:   where the data files are. The path specified here will be a prefix added to each of the files. The best practice is to always use absolute path in here. 
*  *--output-directory=OUTPUT_DIRECTORY*
:   where you want the output files to be. PePr will add this path as a prefix to the output files. It is recommended to use the absolute path. 
*  *--version*
:   Will show the version number and exit. 

## Output Files
 * *NAME__PePr_peaks.bed:* A tab-delimited file containing chromosomal position of the peak, name, signal value, fold change, p-value and Benjamini-Hochberg FDR. Peak format is same as the [ENCODE BroadPeak](https://genome.ucsc.edu/FAQ/FAQformat.html#format13) format. 
* *NAME__PePr_[chip1/2]_peaks.bed*: this is the same as above, but only available when you run in differential binding mode. "chip1_peaks" are enriched in chip1, "chip2_peaks* are enriched in chip2. 
* *NAME__PePr_parameters.txt:* A file containing the parameters to reproduce the results. 
* *NAME-Date-debug.log:* This file contains the detailed information about the running status. Useful debugging information contains: Chromosomes analyzed, shift size and window size estimation, number of candidate windows, etc.

### Links
* https://github.com/shawnzhangyx/PePr/ # Source code
* https://ones.ccmb.med.umich.edu/tags/PePr/ # PePr FAQ
* https://pypi.python.org/pypi/pepr #PyPI package index

### Questions?
Questions are preferred to be posted on https://ones.ccmb.med.umich.edu/, you'll very likely find similar problems in there too (https://ones.ccmb.med.umich.edu/tags/PePr/?tab=Summary). You're also welcome to shoot me an e-mail at yanxiazh@umich.edu, I'll try replying to you as soon as possible. 


### Cite PePr
Zhang Y, Lin YH, Johnson TD, Rozek LS, Sartor MA. [PePr: A peak-calling prioritization pipeline to identify consistent or differential peaks from replicated ChIP-Seq data.](http://www.ncbi.nlm.nih.gov/pubmed/24894502) Bioinformatics. 2014.
