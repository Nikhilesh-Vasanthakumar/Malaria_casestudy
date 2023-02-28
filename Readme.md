

This readme file is for a case study analysing the relationship between Malaria parasites and 

##Info on Data used
A tar zip file was provided which contains 7 Plasmodium genomes 
1.Plasmodium_berghei
2.Plasmodium_cynomolgi
3.Plasmodium_faciparum
4.Plasmodium_knowlesi
5.Plasmodium_vivax
6.Plasmodium_yoelii
7.Toxoplasma_gondii

The Toxoplasma.gff file was given after gene Prediction which is from Toxoplasma gondii considered as an outgroup.

##Genepred using Gene Mark for the Plasmodium gene
The files containing scaffolds and chromosomes were used to predict genes.
The following code was used to predict genes using Genemark.

>gmes_petal.pl --ES --sequence Plasmodium_cynomolgi.genome

##Cleaning the sequenced H.tartakovskyi genome

The average GC content in the given file is 26.30% and the expected GC content for the parasite ranges from 19% to 43%
So we take the third quartile value of 31% to filter out the avian sequences which have a mean GC content of 42% using the following code.

>gmes_petap.pl --ES --cores 12 --min_contig=3000 --sequence filter.fasta

