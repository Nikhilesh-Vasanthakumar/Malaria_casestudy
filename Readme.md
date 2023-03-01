

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

>gmes_petap.pl --ES --sequence Plasmodium_cynomolgi.genome

##Cleaning the sequenced H.tartakovskyi genome

The average GC content in the given file is 26.30% and the expected GC content for the parasite ranges from 19% to 43%
So we take the third quartile value of 31% to filter out the avian sequences which have a mean GC content of 42% and take the minimum contig length of 3000 using the **Scaffold_remove.py** script.

>gmes_petap.pl --ES --cores 12 --min_contig=3000 --sequence filter.fasta

To convert the gtf file to gff file the folowing code was used.
>cat H_tartakovskyi.gtf | sed "s/ GC=.*\tGeneMark.hmm/\tGeneMark.hmm/" > Ht2.gf

The converted gff file was used to parse the dna sequences and their respctive amino acid sequence to compare the genes using blastp

>gffParse.pl -i filter.fasta -g Ht2.gf -c -p -F

The parsed faa file and fna file were used to perform blastp search using the SwissProt database.

>blastp -query gffParse.faa -db SwissProt -out H_tartakovskyi.blastp -num_threads 12

The results of the blastp include some amount of avian contigs.To identify these we compare the blastp results to the uniprot and ebi datasets.

>python datParser.py H_tartakovskyi.blastp avian_contami.fna taxonomy.dat uniprot_sprot.dat > scaffolds.txt

The resulting scaffolds file was used to filter out the avian sequences which remained after the filtration(22 in our case).

>grep -A1 -f scaffolds.txt filter.fasta >bird_filter.fasta
>grep -v -f bird_filter.fasta filter.fasta >bird_filter_final.fasta

The remaining sequences were then used for gene prediction again and parsed to find the accurate gene counts(3720 genes) and genome size.

>gmes_petap.pl --ES --cores 12 --sequence bird_filter_final.fasta
>gffParse.pl -i filter.fasta -g Ht_final.gf -c -p -F
