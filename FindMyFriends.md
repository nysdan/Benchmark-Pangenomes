Benchmark pangenomes
================
Daniela Costa
27/3/2019

FindMyFriends
=============

``` r
dirs <- list.dirs(path = "~/Documents/Trabajo/Benchmark/genomes_fasta")
dirs <- dirs[-1]
genomas <- list()
for(i in 1:length(dirs)){
  genomas[[i]] <- list.files(path = dirs[i], 
                             pattern = ".fasta$", 
                             full.names = T)
}
names(genomas) <- stringr::str_split(dirs, pattern = "/", simplify = T)[,8]
genomas
```

    ## $`1e_06`
    ##  [1] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_06/genome1.fasta" 
    ##  [2] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_06/genome10.fasta"
    ##  [3] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_06/genome2.fasta" 
    ##  [4] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_06/genome3.fasta" 
    ##  [5] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_06/genome4.fasta" 
    ##  [6] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_06/genome5.fasta" 
    ##  [7] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_06/genome6.fasta" 
    ##  [8] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_06/genome7.fasta" 
    ##  [9] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_06/genome8.fasta" 
    ## [10] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_06/genome9.fasta" 
    ## 
    ## $`1e_07`
    ##  [1] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_07/genome1.fasta" 
    ##  [2] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_07/genome10.fasta"
    ##  [3] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_07/genome2.fasta" 
    ##  [4] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_07/genome3.fasta" 
    ##  [5] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_07/genome4.fasta" 
    ##  [6] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_07/genome5.fasta" 
    ##  [7] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_07/genome6.fasta" 
    ##  [8] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_07/genome7.fasta" 
    ##  [9] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_07/genome8.fasta" 
    ## [10] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_07/genome9.fasta" 
    ## 
    ## $`1e_08`
    ##  [1] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_08/genome1.fasta" 
    ##  [2] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_08/genome10.fasta"
    ##  [3] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_08/genome2.fasta" 
    ##  [4] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_08/genome3.fasta" 
    ##  [5] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_08/genome4.fasta" 
    ##  [6] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_08/genome5.fasta" 
    ##  [7] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_08/genome6.fasta" 
    ##  [8] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_08/genome7.fasta" 
    ##  [9] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_08/genome8.fasta" 
    ## [10] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/1e_08/genome9.fasta" 
    ## 
    ## $`5e_06`
    ##  [1] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_06/genome1.fasta" 
    ##  [2] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_06/genome10.fasta"
    ##  [3] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_06/genome2.fasta" 
    ##  [4] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_06/genome3.fasta" 
    ##  [5] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_06/genome4.fasta" 
    ##  [6] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_06/genome5.fasta" 
    ##  [7] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_06/genome6.fasta" 
    ##  [8] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_06/genome7.fasta" 
    ##  [9] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_06/genome8.fasta" 
    ## [10] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_06/genome9.fasta" 
    ## 
    ## $`5e_07`
    ##  [1] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_07/genome1.fasta" 
    ##  [2] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_07/genome10.fasta"
    ##  [3] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_07/genome2.fasta" 
    ##  [4] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_07/genome3.fasta" 
    ##  [5] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_07/genome4.fasta" 
    ##  [6] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_07/genome5.fasta" 
    ##  [7] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_07/genome6.fasta" 
    ##  [8] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_07/genome7.fasta" 
    ##  [9] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_07/genome8.fasta" 
    ## [10] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_07/genome9.fasta" 
    ## 
    ## $`5e_08`
    ##  [1] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_08/genome1.fasta" 
    ##  [2] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_08/genome10.fasta"
    ##  [3] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_08/genome2.fasta" 
    ##  [4] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_08/genome3.fasta" 
    ##  [5] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_08/genome4.fasta" 
    ##  [6] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_08/genome5.fasta" 
    ##  [7] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_08/genome6.fasta" 
    ##  [8] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_08/genome7.fasta" 
    ##  [9] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_08/genome8.fasta" 
    ## [10] "/Users/danielacosta/Documents/Trabajo/Benchmark/genomes_fasta/5e_08/genome9.fasta"
