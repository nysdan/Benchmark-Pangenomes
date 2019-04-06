Benchmark pangenomes
================
Daniela Costa

PanOct
======

Input:
------

*PanOCT* requires 3 input files:

1- all vs. all blast table results. Here's an example:

    # BLASTP 2.2.15 [Oct-15-2006]
    # Query: NT08AB0001
    # Database: /usr/local/annotation/NTAB08/lbrinkac/acinetobacter.pep
    # Fields: Query id, Subject id, % identity, alignment length, mismatches, gap openings, q. start, q. end, s. start, s. end, e-value, bit score
    NT08AB0001  NT08AB0001  100.00  465 0   0   1   465 1   465 0.0  907
    NT08AB0001  NT20ABA0020 99.78   465 1   0   1   465 1   465 0.0  907
    NT08AB0001  NT16AB0001  99.78   465 1   0   1   465 1   465 0.0  907
    NT08AB0001  NT17AB0001  99.26   407 3   0   59  465 1   407 0.0  783
    NT08AB0001  NT20ABA1287 26.97   178 119 7   114 284 58  231 6e-07   50.8
    # BLASTP 2.2.15 [Oct-15-2006]
    # Query: NT08AB0002
    # Database: /usr/local/annotation/NTAB08/lbrinkac/acinetobacter.pep
    # Fields: Query id, Subject id, % identity, alignment length, mismatches, gap openings, q. start, q. end, s. start, s. end, e-value, bit score
    NT08AB0002  NT20ABA0019 100.00  382 0   0   1   382 1   382 0.0  734
    NT08AB0002  NT16AB0002  100.00  382 0   0   1   382 1   382 0.0  734
    NT08AB0002  NT08AB0002  100.00  382 0   0   1   382 1   382 0.0  734
    NT08AB0002  NT17AB0002  99.74   382 1   0   1   382 1   382 0.0  733

This file can be obtained using the following:

``` r
fi <- list.files("prueba/genomas_fasta", pattern = ".fasta$", full.names = T)
fi2 <- list.files("prueba/genomas_fasta", pattern = ".fasta$")
for(i in 1:length(fi)){
  fasta <- seqinr::read.fasta(fi[i])
  fastaAA <- lapply(fasta, seqinr::translate)
  write.fasta(sequences = fastaAA, file.out = paste0("prueba/genomas_AA/", gsub(".fasta", "", fi2[i]), "_AA.fasta"), names=names(fastaAA))
}
```

Cat all sequences and make blast database:

    cat *AA.fasta > all_AA.fasta
    makeblastdb -in all_AA.fasta -dbtype prot

Blast all vs. all

    blastp -query all_AA.fasta -db DB/all_AA.fasta -outfmt 7 -out blast_allAA.txt

2- A text file containing unique genome identifiers, one identifier per line. Here I'll use the genomes names:

``` r
writeLines(gsub(".fasta", "", fi2), "prueba/genome_tags.txt", sep = "\n")
```

3- The gene attribute file. It is a tab-delimited file containing the following data: contig id, protein identifier (e.g. locus), 5′-coordinate, 3′-coordinate, annotation and genome identifier.

The gft files were obtained from gffs using gffread (<https://github.com/gpertea/gffread>).

``` r
fi <- list.files("genomes_gtf", full.names = T)
fi2 <- list.files("genomes_gtf")
dfs <- list()
for(i in 1:length(fi)){
  gtf <- read.table(fi[i], sep = "\t", header=F, stringsAsFactors = F)
  gtf <- gtf[gtf$V3=="CDS",]
  protID <- stringr::str_split(stringr::str_split(gtf[,9],
                                                  pattern = "; ",
                                                  simplify = T)[,1], 
                               pattern = " ",
                               simplify = T)[,2]
  df <- data.frame(contig = gtf[,1], 
           protID=protID,
           start=gtf[,4],
           end=gtf[,5],
           annotation=protID,
           genome=gsub("_gene\\d+", "", protID),
           stringsAsFactors=F)
  dfs[[i]] <- df
}
gene_attr <- do.call(rbind.data.frame,dfs)
write.table(gene_attr, file="gene_attr.txt", sep = "\t", col.names = F, row.names = F, quote = F)
```

4- The protein fasta file used in the all-versus-all BLASTP searches

Run PanOCT
----------

    ~/Programas/panoct_v3.23/bin/panoct.pl -t blast_allAA.txt -f genome_tags.txt -g gene_attr.txt -P all_AA.pep -S Y -L 1 -M Y -H Y -V Y -N Y -F 1.33 -G y -c 0,95,100

Flags:
- **-t**: all vs. all blast results
- **-f**: genome names file (see input file 2)
- **-g**: gene attributes table
- **-P**: protein fasta file
- **-S Y**: means that strict criteria for ortholog determination is chosen. Other options are *m* (intermediate) or *n* (off)
- **-L 1**: minimun match length
- **-M Y**: create microarray-like data for normalized BLAST scores yes *Y* or no *N*
- **-H Y**: create a table of hits yes *Y* or no *N*
- **-V Y**: create an ortholog matchtable yes *Y* or no *N*
- **-N Y**: create a normalized BLAST score file yes *Y* or no *N*
- **-F \#**: Deprecate shorter protein fragments when protein is split due to frameshift or other reason. Takes an argument between 1.0 and 2.0 as a length ratio test - recommended value is 1.33
- **-G y**: create a normalized BLAST score histogram file containing a row for each genome and two rows for each pair of genomes yes *Y* or no *N*
- **-c 0,25,50**: comma separated list of numbers between 0-100 (0,50,75,100 might be a good choice) each number represents the percent of genomes needed for a cluster to be considered a core cluster. For each number two files are generated: one for core clusters and one for noncore clusters.

Output
------

``` r
matchtable_0_1 <- read.table("prueba/output/matchtable_0_1.txt", 
                             header = F, 
                             stringsAsFactors = F)
colnames(matchtable_0_1)[2:11] <- c("genome1", "genome10", "genome2", 
                                    "genome3", "genome4", "genome5", 
                                    "genome6", "genome7", "genome8",
                                    "genome9")
matchtable_0_1[,1] <- paste0("cluster", matchtable_0_1[,1])
rownames(matchtable_0_1) <- matchtable_0_1[,1]
matchtable_0_1 <- matchtable_0_1[,-1]
knitr::kable(matchtable_0_1[1:6,1:6])
```

|          |  genome1|  genome10|  genome2|  genome3|  genome4|  genome5|
|----------|--------:|---------:|--------:|--------:|--------:|--------:|
| cluster1 |        1|         1|        1|        1|        1|        1|
| cluster2 |        1|         1|        1|        1|        1|        1|
| cluster3 |        1|         1|        1|        1|        1|        1|
| cluster4 |        1|         1|        1|        1|        1|        1|
| cluster5 |        1|         1|        1|        1|        1|        1|
| cluster6 |        1|         1|        1|        1|        1|        1|

Clusters in all genomes (core): 97
Clusters in 90% of genomes (soft-core): 2
Clusters in 10-90% of genomes (cloud): 7
Singletons: 1
**Total: 107 clusters**

``` r
gplots::heatmap.2(t(as.matrix(matchtable_0_1)), 
                  trace = "none", 
                  density.info = "none", 
                  col = c("#a2a8d3","#38598b"), 
                  cexRow = 0.5, 
                  labCol = NA, 
                  key=F, main = "PanOCT with dataset dist=1E-6")
```

![](PanOCT_files/figure-markdown_github/unnamed-chunk-5-1.png)
