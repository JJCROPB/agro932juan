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
###### -Then I did the alignment for the 10 individuals using this code:
###### for i in {1..10}; do bwa mem ecoli.fasta doc/l$i.R1.fq doc/l$i.R2.fq | samtools view -bSh - > data/l$i.bam; done
for i in *.bam; do samtools sort $i -o sorted_$i; done
for i in sorted*.bam; do samtools index $i; done
###### -After doing the alignment, I proceeded to to the SNP calling utilizing samtools, a variant calling software. Fisrt I converted the BAM files to a BCF file called myraw.bcf
###### Samtools faidx ecoli.fasta
samtools mpileup -g -f ecoli.fasta sorted_l1.bam sorted_l2.bam sorted_l3.bam sorted_l4.bam sorted_l5.bam sorted_l6.bam sorted_l7.bam sorted_l8.bam sorted_l9.bam sorted_l10.bam> myraw.bcf
###### -After that, I did the SNP call unsing bcftools:
###### bcftools call myraw.bcf -cv -Ob -o snps.bcf
###### -Later, I extract the allele frequency at each position using bcftools in a file called frq.txt: 
###### bcftools query -f '%CHROM %POS %AF1\n' snps.bcf > largedata/frq.txt
###### -Then, I opened R to perform a calculation and get θπ:
###### pi <- function(n=10, p=0.1){
  return(n/(n-1)*(1-p^2-(1-p)^2))
}
pi(n=10, p=0.1)
frq <- read.table("largedata/frq.txt")
###### -Then, I generated a plot with the general allele frequency using this code:
###### maf=frq.txt colum 3
maf <- c(0.1, 0.1, 0.3, 0.1, 0.3, 0.2, 0.1, 0.4, 0.1, 0.1)
sfs <- table(maf)
barplot(sfs, col="#cdc0b0", xlab="Minor allele frequency", 
        ylab="No. of segregating sites", 
        cex.axis =1.5, cex.names = 1.5, cex.lab=1.5)
###### And to save it in a graphs/Rplots.pdf:
###### pdf plot.pdf
###### 
###### 
###### 
###### 
###### 


