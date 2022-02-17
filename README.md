###### agro932juan
###### This is a project that aims to perform a simulation of Next Generation Sequencing dataset in  two populations. 
###### -For that I used the reference genome of the ribosomeRNA and I transferRNA of Escherichia coli genes for 16S rRNA, 5S rRNA, 23S rRNA, tRNA-Ile, tRNA-Ala, partial and complete sequence https://www.ncbi.nlm.nih.gov/nuccore/216643
###### -I used wgsim software https://github.com/lh3/wgsim to run the simulation
###### -First, I create this repository including some directories to organize the files
###### -Then, I cloned this repository in CRANE, a high performance cluster from UNL
###### -After that, I cloned and instaled the software wgsim into the repository in CRANE https://github.com/lh3/wgsim
###### -Once it was cloned, I downloaded the reference genome with 4481 bp and place it into the repository in CRANE https://www.ncbi.nlm.nih.gov/nuccore/216643
###### -With the reference genome and the software installed, I ran this code to perform the data simulation using wgsim ecoli.fasta -N 100 -1 60 -2 60 -r 0.1 -e 0 \ -R 0 -X 0 -S 1000000 doc/l1.R1.fq doc/l1.R2.fq
###### -After getting this, I ran a simulation for 10 individuals using this code: for i in {1..10}
do
   wgsim ecoli.fasta -N 5000 -1 100 -2 100 -r 0.01 -e 0 -R 0 -X 0 l$i.R1.fq l$i.R2.fq
done
##### -Then I checked how many reads I generated with this code:
wc -l doc/l1.R1.fq
###### -After getting the Fastq files that contained the sequencing data, I used bwa, samtools and bcftools softwares to perform the alignment to the reference genome and at the end, get BAM files which are compressed binary version of SAM files, generated with the mapping programs. I used this code:
###### module load bwa samtools bcftools
###### bwa index ecoli.fasta
###### bwa mem ecoli.fasta doc/l1.R1.fq doc/l1.R2.fq | samtools view -bSh - > data/l1.bam
###### Then I did the alignment for the 10 individuals using this code:
###### for i in {1..10}; do bwa mem ecoli.fasta doc/l$i.R1.fq doc/l$i.R2.fq | samtools view -bSh - > data/l$i.bam; done
for i in *.bam; do samtools sort $i -o sorted_$i; done
for i in sorted*.bam; do samtools index $i; done
###### After doing the alignment, I proceeded to to the SNP calling utilizing samtools, a variant calling software:
###### 
###### 
###### 
###### 
###### 


