# SeqSero2 v1.0.1
Salmonella serotype prediction from genome sequencing data.

Online version: http://www.denglab.info/SeqSero2

# Introduction 
SeqSero2 is a pipeline for Salmonella serotype prediction from raw sequencing reads or genome assemblies

# Dependencies 
SeqSero has three workflows:

(A) Allele micro-assembly (default). This workflow takes raw reads as input and performs targeted assembly of serotype determinant alleles. Assembled alleles are used to predict serotype and flag potential inter-serotype contamination in sequencing data (i.e., presence of reads from multiple serotypes due to, for example, cross or carryover contamination during sequencing). 

Allele micro-assembly workflow depends on:

1. Python 3;

2. [Burrows-Wheeler Aligner v0.7.12](http://sourceforge.net/projects/bio-bwa/files/);

3. [Samtools v1.8](http://sourceforge.net/projects/samtools/files/samtools/);

4. [NCBI BLAST v2.2.28+](https://blast.ncbi.nlm.nih.gov/Blast.cgi?PAGE_TYPE=BlastDocs&DOC_TYPE=Download);

5. [SRA Toolkit v2.8.0](http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?cmd=show&f=software&m=software&s=software);

6. [SPAdes v3.9.0](http://bioinf.spbau.ru/spades);

7. [Bedtools v2.17.0](http://bedtools.readthedocs.io/en/latest/);

8. [SalmID v0.11](https://github.com/hcdenbakker/SalmID).


(B) Raw reads k-mer. This workflow takes raw reads as input and performs rapid serotype prediction based on unique k-mers of serotype determinants. 

Raw reads k-mer workflow (originally SeqSeroK) depends on:

1. Python 3;
2. [SRA Toolkit](http://www.ncbi.nlm.nih.gov/Traces/sra/sra.cgi?cmd=show&f=software&m=software&s=software) (optional, just used to fastq-dump sra files);


(C) Genome assembly k-mer. This workflow takes genome assemblies as input and the rest of the workflow largely overlaps with the raw reads k-mer workflow


# Executing the code 
Make sure all SeqSero2 and its dependency executables are added to your path (e.g. to ~/.bashrc). Then type SeqSero2_package.py to get detailed instructions.

    Usage: SeqSero2_package.py 

    -m <string> (which workflow to apply, 'a'(raw reads allele micro-assembly), 'k'(raw reads and genome assembly k-mer), default=a)

    -t <string> (input data type, '1' for interleaved paired-end reads, '2' for separated paired-end reads, '3' for single reads, '4' for genome assembly, '5' for nanopore fasta, '6'for nanopore fastq)

    -i <file> (/path/to/input/file)

    -p <int> (number of threads for allele mode, if p >4, only 4 threads will be used for assembly since the amount of extracted reads is small, default=1) 

    -b <string> (algorithms for bwa mapping for allele mode; 'mem' for mem, 'sam' for samse/sampe; default=mem; optional; for now we only optimized for default "mem" mode)
 
    -d <string> (output directory name, if not set, the output directory would be 'SeqSero_result_'+time stamp+one random number)
	
    -c <flag> (if '-c' was flagged, SeqSero2 will only output serotype prediction without the directory containing log files)
    
    --check <flag> (use '--check' flag to check the required dependencies)
    
    -v, --version (show program's version number and exit)
	

# Examples
Allele mode:

    # Allele workflow ("-m a", default), for separated paired-end raw reads ("-t 2"), use 10 threads in mapping and assembly ("-p 10")
    SeqSero2_package.py -p 10 -t 2 -i R1.fastq.gz R2.fastq.gz
	
K-mer mode:

    # Raw reads k-mer ("-m k"), for separated paired-end raw reads ("-t 2")
    SeqSero2_package.py -m k -t 2 -i R1.fastq.gz R2.fastq.gz

    # Genome assembly k-mer ("-t 4", genome assemblies only predicted by the k-mer workflow, "-m k")
    SeqSero2_package.py -m k -t 4 -i assembly.fasta
	
# Output 
Upon executing the command, a directory named 'SeqSero_result_Time_your_run' will be created. Your result will be stored in 'Seqsero_result.txt' in that directory. And the assembled alleles can also be found in the directory if using "-m a" (allele mode).


# Citation
Zhang S, Yin Y, Jones MB, Zhang Z, Deatherage Kaiser BL, Dinsmore BA, Fitzgerald C, Fields PI, Deng X.  
Salmonella serotype determination utilizing high-throughput genome sequencing data.  
**J Clin Microbiol.** 2015 May;53(5):1685-92.[PMID:25762776](http://jcm.asm.org/content/early/2015/03/05/JCM.00323-15)
