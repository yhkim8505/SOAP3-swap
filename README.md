# SOAP3-swap

SOAP3-swap is a GPU-based software based on SOAP3-dp for aligning short reads to a reference sequence considering swap of adjacent bases.

# <a name="Section1"></a>1. Hardware and Platform

To run SOAP3-swap, you need a linux workstation equipped with

  * a multi-core CPU with at least 16GB main memory (default quad-core);
  * a CUDA-enabled GPU with compute capability 9.0 and at least 3GB memory.

SOAP3-swap has been tested with the following GPU: NVIDIA Geforce 2080 Ti (11GB memory).

SOAP3-swap was developed under the 64-bit linux platform and the CUDA Driver version 9.0.

# <a name="Section2"></a>2. Usage (Almost same as SOAP3-dp)

## <a name="Section2.1"></a>2.1 Index builder

It is the same to SOAP3-dp.

### Step 1: Build the 2BWT index

Syntax:

```bash
  % ./soap3-dp-builder
```

For example:

```bash
  % ./soap3-dp-builder genome.fa
```

### Step 2: Convert the 2BWT index to the GPU-2BWT index

Syntax:

```bash
  % ./BGS-Build .index
```

For example:

```bash
  % ./BGS-Build genome.fa.index
```

## <a name="Section2.2"></a>2.2 Aligner

### <a name="pair"></a>1. One set of paired-end reads in FASTA, FASTQ or GZIP format

Input reads from two files [read file 1] and [read file 2], and both read files are in FASTA, FASTQ or GZIP format (The aligner can detect these format automatically).

Syntax:

```bash
  % ./soap3-swap pair [ref seq file].index [read file 1] [read file 2] [options]
```

```
Options:
-u [max value of insert size]	                    default: 500;
 	 
-v [min value of insert size]	                    default: 1;
 	 
-L [length of the longest read in the input]	    default: 120; normally in the range [75,200];
 	                                                if < 100, better using GPU with 6GB memory
 	 
-h [output option]	                                1: all valid alignments;
 	                                                2: all best alignment (default);
 	                                                3: unique best alignment;
 	                                                4: random best alignment
 	 
-b [output format]	                                1: Succinct;
 	                                                2: SAM (default);
 	                                                3: BAM
 	 
-o [output prefix]	                                The output file names:
 	                                                [output prefix].dpout.1, [output prefix].gout.1, [output prefix].gout.2, ...
 	                                                default: [read file 1];
 	 
-c [GPU device ID]	                                Specify the GPU device for running SOAP3-swap;
 	 
-I	                                                The input is in the Illumina 1.3+ FASTQ-like format
 	                                                default: this option is not enabled;
 	 
-A [sample name]	                                default: "default";
 	 
-D [read group ID]	                                default: [read file 1];
 	 
-R [read group option]	                            Assign the read group option
 	                                                default: this option is not enabled;

-p                                                  Output MD string and NM tag.
 	 
-s [max # of mismatches]	                        any integer from 0 to 4,
 	                                                disable dynamic programming and
 	                                                perform alignment with mismatches only.
 	                                                If max # of mismatches is not specified,
 	                                                default number is 3 for reads of length >= 50;
 	                                                or 2 otherwise.
 	                                                This option is not recommended.
```

Example: Assume read files query1.fa and query2.fa, and insert size range [200, 500].

```bash
  % ./soap3-swap pair genome.fa.index query1.fa query2.fa -u 500 -v 200
```

# <a name="Section4"></a>3. Fine tuning via configuration file soap3-swap.ini

> MatchScore (default 1), MismatchScore (default -2), GapOpenScore (default -3), GapExtendScore (default -1), swap (default -1):

These five parameters are the scorings of single match, single mismatch, gap opening, gap extension, and single swap in the dynamic programming module. MatchScore can be any integer greater than or equal to 1; the other scores can be any integers in the range [-10,-1].
