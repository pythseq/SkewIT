# SkewIT
SkewIT (Skew Index Test) is a tool for analyzing GC Skew in bacterial genomes. 
The main script `skewi.py` is located in the `/src/` folder. Users can make this script executable by running
 
    chmod +x skewi.py 
    ./skewi.py -h
    ./skewi.py --usage

**IMPORTANT:** GC Skew/SkewIT is intended for use with only complete, fully contiguous, bacterial sequences with no gaps. Sequences should be fully assembled from end-to-end for the calculated SkewI to be informative. Contigs/Scaffolds are not expected to display GC Skew. 

## SkewI.py Usage/Options

    python skewi.py -i SEQ.FASTA

    Required options:
    *   -i SEQ.FASTA...............fasta/multi-fasta sequence file
   
    Optional options:
    *   -o SKEWI.TXT...............output file [if none is provided, the program will print to standard out]
    *   -k WINDOW_SIZE.............size of window to assign a gc skew value [default: 20kb] 
    *   -f FREQUENCY...............number of bases between the start of each window [default: k == f, adjacent/non-overlapping windows]
    *   --min-seq-len LENGTH.......minimum sequence length required to analyze [default: 500kb]
    *   --complete/--all...........only analyze complete sequences/analyze complete and draft sequences [default: --complete]
    *   --plasmid/--no-plasmid.....include/exclude plasmid sequences [default: --no-plasmid]

### Input Files

Currently, input sequence files must be FASTA formatted and not zipped. 

### Output Format

If an output file is provided, the program will generate a tab-delimited, 2-column output file with headers. The first column will contain the full sequence ID/description. The second column will contain the calculated SkewI value. 

If no output file is provided, the program will print these two columns to the system standard out. Users can pipe this output into a file of their choice by running:
        `python skewi.py -i MYSEQ.FASTA > MYOUTPUT.TXT`


### Window Length/Frequency Options (-k/-f/--min-seq-len)
    
By default, the program will calculate SkewI using non-overlapping/adjacent windows of size 20kb only for sequences with a minimum length of 500kb. 

If users choose to change the window size (`-k`), but do not specify a window frequency, the program will by default use non-overlapping/adjacent windows (`k == f`) 

1. For overlapping sequences, specify a frequency < window length:
        
    `python skewi.py -i MYSEQ.FASTA -k 20000 -f 10000`

2. For no minimum sequence length, specify `--min-seq-len 0`
    
    `python skewi.py -i MYSEQ.FASTA --min-seq-len 0`

3. For a smaller window size (and therefore more resolution):
        
    `python skewi.py -i MYSEQ.FASTA -k 10000` 
    
The window size `-k` must always be larger or equal to frequency `-f`. Both values must be greater than 0. 
    

### Complete Genome Options (--complete/--all)
As the program was designed to work with RefSeq output files, these two options are provided to allow users to specify whether complete or all genomes in the provided files should be analyzed.

Specifying `--complete` will require that "complete" is in the sequence header, while specifying `--all` will allow any sequence to be analyzed. 
    


### Plasmid Options (--plasmid/--no-plasmid)
This program was designed for analysis of bacterial chromosomes, not plasmids. We have not tested the performance of the program on plasmid sequences. Therefore, by default, the program will skip any sequence containing "plasmid" in the header. 

If users would like to analyze plasmid sequences in their input files, simply specify `--plasmid` during runtime. 

# Author information
Updated: 2020/02/25 

Jennifer Lu, jennifer.lu717@gmail.com 
