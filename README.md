
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
   qzhang@e6b9c1a37b42:/kb/module$ STAR  --runMode genomeGenerate --runThreadN 4 --genomeDir ./STAR_genome_directory/ --genomeFastaFiles testReads/test_long.fa
   
    May 31 23:18:10 ..... started STAR run
    May 31 23:18:10 ... starting to generate Genome files
    May 31 23:18:12 ... starting to sort Suffix Array. This may take a long time...
    May 31 23:18:13 ... sorting Suffix Array chunks and saving them to disk...
    May 31 23:18:14 ... loading chunks from disk, packing SA...
    May 31 23:18:15 ... finished generating suffix array
    May 31 23:18:15 ... generating Suffix Array index
    May 31 23:18:17 ... completed Suffix Array index
    May 31 23:18:17 ... writing Genome to disk ...
    May 31 23:18:18 ... writing Suffix Array to disk ...
    May 31 23:18:18 ... writing SAindex to disk
    May 31 23:18:19 ..... finished successfully

qzhang@e6b9c1a37b42:/kb/module$ ls -la STAR_genome_directory/

    total 2044944
    drwxrwxrwx  2 root root       4096 May 31 23:18 .
    drwxrwxrwx 28 root root       4096 May 31 23:18 ..
    -rw-r--r--  1 3184 root  524288000 May 31 23:18 Genome
    -rw-r--r--  1 3184 root    1650003 May 31 23:18 SA
    -rw-r--r--  1 3184 root 1565873619 May 31 23:18 SAindex
    -rw-r--r--  1 3184 root       8000 May 31 23:18 chrLength.txt
    -rw-r--r--  1 3184 root      48890 May 31 23:18 chrName.txt
    -rw-r--r--  1 3184 root      56890 May 31 23:18 chrNameLength.txt
    -rw-r--r--  1 3184 root      19580 May 31 23:18 chrStart.txt
    -rw-r--r--  1 3184 root        490 May 31 23:18 genomeParameters.txt
</p>


<h4>The basic options to run a mapping job are as follows</h4>
<p>

--runThreadN NumberOfThreads
--genomeDir /path/to/genomeDir
--readFilesIn /path/to/read1 [/path/to/read2 ]

  i.e., 
  STAR --genomeDir <Directory with the Genome Index>  --runThreadN <# cpus> --readFilesIn <FASTQ file> --outFileNamePrefix <OutputPrefix>
  
  e.g.,
  qzhang@e6b9c1a37b42:/kb/module$ STAR --genomeDir STAR_genome_directory/  --runThreadN 4 --readFilesIn testReads/small.forward.fq --outFileNamePrefix Experiment1Star
  
    May 31 23:22:16 ..... started STAR run
    May 31 23:22:16 ..... loading genome
    May 31 23:22:17 ..... started mapping
    May 31 23:22:19 ..... finished successfully

  e.g.(for paried-end data),
  qzhang@e6b9c1a37b42:/kb/module$ STAR --genomeDir STAR_genome_directory/  --runThreadN 4 --readFilesIn testReads/small.forward.fq testReads/small.reverse.fq --outFileNamePrefix Experiment1Star_paired
  
    May 31 23:23:27 ..... started STAR run
    May 31 23:23:27 ..... loading genome
    May 31 23:23:28 ..... started mapping
    May 31 23:23:32 ..... finished successfully
</p>

<h4>STAR will create several output files</h4>
<p>

The most important of which is the "*.Aligned.out.sam". The default output is a SAM file.
</p>

