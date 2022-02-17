###### agro932juan
###### This is a project that aims to perform a simulation of Next Generation Sequencing dataset in  two populations. For that I used the reference genome of the ribosomeRNA and ###### transferRNA of Escherichia coli genes for 16S rRNA, 5S rRNA, 23S rRNA, tRNA-Ile, tRNA-Ala, partial and complete sequence https://www.ncbi.nlm.nih.gov/nuccore/216643
###### I used wgsim software https://github.com/lh3/wgsim to run the simulation
###### First, I create this repository including some directories to organize the files
###### Then, I cloned this repository in CRANE, a high performance cluster from UNL
###### After that, I cloned and instaled the software wgsim into the repository in CRANE https://github.com/lh3/wgsim
###### Once it was cloned, I downloaded the reference genome with 4481 bp and place it into the repository in CRANE https://www.ncbi.nlm.nih.gov/nuccore/216643
###### With the reference genome and the software installed, I ran this code to perform the data simulation using wgsim
###### wgsim ecoli.fastaq -N 100 -1 60 -2 60 -r 0.1 -e 0 \ -R 0 -X 0 -S 1000000 docs/l1.R1.fq docs/l1.R2.fq
###### 
######
######
######
######

