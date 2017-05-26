
# STAR
---

A [KBase](https://kbase.us) module generated by the [KBase SDK](https://github.com/kbase/kb_sdk).


This Module was initialized with a generated example App.  To compile and run the
example App implementation, run:

    cd STAR
    make          (required after making changes to $module_name.spec)
    kb-sdk test   (will require setting test user account credentials in test_local/test.cfg)

For more help on how to modify, register and deploy the example to KBase, see the
[KBase SDK documentation](https://github.com/kbase/kb_sdk).

[Reference website](https://github.com/alexdobin/STAR)
[Reference manual](https://github.com/alexdobin/STAR/blob/master/doc/STARmanual.pdf)

<h3>STAR 2.5.3a</h3>
<h4>Basic STAR workflow consists of 2 steps</h4>
<p>
  -  1. Generating genome indexes files

	- In this step user supplied the reference genome sequences (FASTA files) and annotations GTF file), from which STAR generate genome indexes that are utilized in the 2nd (mapping) step. The genome indexes are saved to disk and need only be generated once for each genome/annotation combination. A limited collection of STAR genomes is available from http://labshare.cshl.edu/shares/gingeraslab/www-data/dobin/STAR/STARgenomes/, however, it is strongly recommended that users generate their own genome indexes with most up-to-date assemblies and annotations.
	- Indexes generating is controlled by a variety of input parameters (Basic or Advanced options).
</p>
<p>
  -  2. Mapping reads to the genome-Align RNA-Seq Reads to the genome with STAR

	- In this step user supplies the genome files generated in the 1st step, as well as the RNA-seq reads (sequences) in the form of FASTA or FASTQ files. STAR maps the reads to the genome, and writes several output files, such as alignments (SAM/BAM), mapping summary statistics, splice junctions, unmapped reads, signal (wiggle) tracks etc. 
	- Output files: Log files, .sam files, .bam files, splice junctions, etc.
	STAR produces multiple output files. All files have standard name, however, you can change the file prefixes using --outFileNamePrefix /path/to/output/dir/prefix. By default, this parameter is ./, i.e. all output files are written in the current directory. 

	- Mapping is controlled by a variety of input parameters (Basic or Advanced options).
</p>
<p>
  -  STAR command line has the following format:

	- STAR --option1-name option1-value(s)--option2-name option2-value(s) ...
	If an option can accept multiple values, they are separated by spaces, and in a few cases - by commas.
</p>

<h4>The basic options to generate genome indexes are as follows</h4>
<p>

    --runThreadN NumberOfThreads
    --runMode genomeGenerate
    --genomeDir /path/to/genomeDir
    --genomeFastaFiles /path/to/genome/fasta1 /path/to/genome/fasta2 ...
    --sjdbGTFfile /path/to/annotations.gtf
    --sjdbOverhang ReadLength-1

   i.e., 
   STAR  --runMode genomeGenerate --runThreadN <# cpus> --genomeDir <genome output directory> --genomeFastaFiles <input Genome FASTA file>

   e.g.,
   STAR  --runMode genomeGenerate --runThreadN 24 --genomeDir ./ --genomeFastaFiles mm9.fa
</p>


<h4>The basic options to run a mapping job are as follows</h4>
<p>

--runThreadN NumberOfThreads
--genomeDir /path/to/genomeDir
--readFilesIn /path/to/read1 [/path/to/read2 ]

  i.e., 
  STAR --genomeDir <Directory with the Genome Index>  --runThreadN <# cpus> --readFilesIn <FASTQ file> --outFileNamePrefix <OutputPrefix>
  e.g.,
  STAR --genomeDir mm9-starIndex/  --runThreadN 24 --readFilesIn Experiment1.fastq --outFileNamePrefix Experiment1Star

  e.g.(for paried-end data),
  STAR --genomeDir mm9-starIndex/  --runThreadN 24 --readFilesIn read1.fastq read2.fastq --outFileNamePrefix Experiment1Star 
</p>

<h4>STAR will create several output files</h4>
<p>

The most important of which is the "*.Aligned.out.sam". The default output is a SAM file.
</p>

