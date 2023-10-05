---
layout: post
title: GO enrichment analysis for Maize genes
excerpt: "guide to run GO enrichment analysis for Maize (B73.v5) genes"
date: 2023-09-27T14:25:04+10:00
modified: 2023-09-27T14:25:04+10:00
tags: [PANNZER, topGO, R, bioinformatics, Ontology, GO]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---


# Guide for running GO enrichment on Maize genes

This guide is heavily based on the [David Winter](https://github.com/dwinter)'s guide available [here](https://github.com/dwinter/genome_factory/wiki/Taking-a-gene-list-and-PANNZER-output-through-to-GO-term-enrichment). Please refer to the documentation for more details. The GO annotations (PANZZER) for all NAMs are available [here](https://download.maizegdb.org/GeneFunction_and_Expression/Pannzer_GO_Terms) (files with `_GO.out` extension). We use the [`topGO`](https://bioconductor.org/packages/release/bioc/html/topGO.html) package in R to run the enrichment analysis and `R` (`tidyverse`) to plot the results.

## 1. Download the GO annotations

First, get the GO annotations you wish to run the enrichment on. For example, if you want to run the enrichment on the B73.v5 genes (eg: `Zm00001eb123456`), you can download the GO annotations from MaizeGDB [`B73_GO.out`](https://download.maizegdb.org/GeneFunction_and_Expression/Pannzer_GO_Terms/B73_GO.out). The file looks something like this:



| qpid                 | ontology | goid  | desc                                        | ARGOT_score | ARGOT_PPV | ARGOT_rank | goclasscount |
|----------------------|----------|-------|---------------------------------------------|-------------|-----------|------------|--------------|
| Zm00001eb093920_P002 | BP       | 10119 | regulation of stomatal movement             |    11.68753 |  0.801096 |          1 |            1 |
| Zm00001eb093920_P002 | CC       | 5634  | nucleus                                     |     0.89513 |  0.442076 |          1 |            1 |
| Zm00001eb093920_P001 | CC       | 5634  | nucleus                                     |    4.110372 |  0.599079 |          1 |            8 |
| Zm00001eb093920_P001 | MF       | 976   | transcription cis-regulatory region binding |    1.176456 |  0.462191 |          1 |            1 |
| Zm00001eb093920_P001 | BP       | 30154 | cell differentiation                        |      0.9394 |  0.445432 |          1 |            1 |
| Zm00001eb033650_P002 | MF       | 43733 | DNA-3-methylbase glycosylase activity       |    11.67336 |  0.800795 |          1 |          100 |
| Zm00001eb033650_P002 | BP       | 6284  | base-excision repair                        |    8.374165 |  0.724882 |          1 |          100 |
| Zm00001eb033650_P001 | MF       | 43733 | DNA-3-methylbase glycosylase activity       |    11.67341 |  0.800796 |          1 |          100 |
| Zm00001eb033650_P001 | BP       | 6284  | base-excision repair                        |    8.374205 |  0.724883 |          1 |          100 |

(first 10 lines)
The `qpid` column in the GO annotations is the geneid. The `goid` column is the GO term id. The `ontology` column is the GO term ontology (Biological Processes or BP, Molecular Function or MF, Cellular Component or CC). The `ARGOT_score` is the score assigned by PANNZER. The `ARGOT_PPV` is the precision score assigned by PANNZER. The `ARGOT_rank` is the rank assigned by PANNZER. The `goclasscount` is the number of genes in the GO term. The `desc` is the description of the GO term. 


to download:

```bash
wget https://download.maizegdb.org/GeneFunction_and_Expression/Pannzer_GO_Terms/B73_GO.out
```

## 2. Get the list of genes you want to run the enrichment on

The list of genes can be anything, for example, the genes that are differentially expressed between two conditions or genes that are in a group of interest. Most common reason you want to run the enrichment is to check if a collection of genes have something in common. The differentially expressed genes between two conditions, are usually obtained from `DESeq2` output that is filtered based on `log2fc` and `p-value`. If you are running this on B73.v5 geneids should look  something like this:

```bash
Zm00001eb389020_T001
Zm00001eb009030_T001
Zm00001eb013120_T001
Zm00001eb107030_T001
Zm00001eb115040_T001
Zm00001eb241190_T004
Zm00001eb370330_T001
Zm00001eb269130_T001
Zm00001eb295360_T001
```

The `_T001` at the end of the geneid is the transcript id. Sometimes it can also be `_P001` 

## 3. Format the GO annotations and filter them to run topGO

Required packages:

```r
setwd("~/Desktop/GO_enrichment")
library(tidyverse)
library(scales)
library(topGO)
```

Read the GO annotations and filter them to only include the genes you want to run the enrichment on. The `qpid` (the geneid column) should be similar to the geneids in the list of genes you want to run the enrichment on. Note that your gene list have `_T00N` while the table has them `_P00N`. 

We also want to include only those geneids that have confident `ARGOT_score`  assigned by PANNZER. And finally, we want it in a format that `topGO` can read. We can do this in `R` as follows:

```r
fname = 'B73_GO.out'
go <-
  read.table(
    fname,
    quote = '"',
    sep = "\t",
    header = TRUE,
    colClasses = c('goid' = 'character', 'qpid' = 'character')
  )
# some filtering
go_filt <- go[(go$ARGOT_PPV > 0.5), ]
go_filt$goid <- paste0('GO:', go_filt$goid)
panzer_to_golist <- function(panzer_df){
  go_df <- aggregate( goid ~ qpid, data=panzer_df, FUN=c)
  structure(go_df$goid, .Names=go_df$qpid)
}
all_golist <- panzer_to_golist(go_filt)
```


## 4. Formatting input genelists

Since our genelist have transcript ids, we need to first convert them to protein ids.


```r
myInput <-
  read.table("mygenelist.txt",
             quote = "\"",
             comment.char = "")
colnames(myInput) <- "genenames"
myInput <- myInput %>%
  mutate(genenames = str_replace(genenames, "_T", "_P")) %>%
  mutate(myInput = "yes")
head(myInput)
```
the stdout should show:
```
             genenames myInput
1 Zm00001eb196330_P001     yes
2 Zm00001eb034510_P001     yes
3 Zm00001eb001710_P001     yes
4 Zm00001eb115990_P002     yes
5 Zm00001eb281950_P001     yes
6 Zm00001eb329780_P001     yes
```
 For `topGO` we need the entire list of genes with `yes` or `no` for each gene. The `yes` or `no` indicates if the gene is in the list of genes you want to run the enrichment on. We can do this in `R` as follows:

```r
 # create full list of genes
gene_names <- names(all_golist)
myGenes <- as.data.frame(gene_names)
colnames(myGenes) <- "genenames"
myGenes <- left_join(myGenes, myInput)
myGenes$myInput <- myGenes$myInput %>%
  replace_na('no')
myGenesF <- factor(as.integer(myGenes$myInput == 'yes'))
names(myGenesF) <- gene_names
```
The stdout should show:
```
> head(myGenesF, 3)
Zm00001eb000010_P001 Zm00001eb000020_P001 Zm00001eb000020_P002 
                   0                    0                    0
```

Now we are ready to run the enrichment analysis.


## 5. Run the enrichment analysis

If you have a large number of list to run the enrichment, you can create a function and use that to run the enrichment. 

```r
run_topGO <- function(gene_list, ontology, gene2GO_list){
  topGO_data <- new("topGOdata", ontology = ontology, allGenes = gene_list,
                    annot = annFUN.gene2GO, gene2GO = gene2GO_list)
  fishers_result <- runTest(topGO_data, algorithm = "elim", statistic = "fisher")
  fishers_table <- GenTable(topGO_data, Fishers = fishers_result, useLevels = TRUE)
  fishers_table$Ontology <- ontology
  fishers_table
}
```

Now we can run the enrichment analysis on the list of genes we want to run the enrichment on. 

```r
myGenes_BP.table <- run_topGO(myGenesF, "BP", all_golist)
myGenes_MF.table <- run_topGO(myGenesF, "MF", all_golist)
myGenes_CC.table <- run_topGO(myGenesF, "CC", all_golist)
```

The stdout should show something like this:

```
> head(myGenes_BP.table)
       GO.ID                                        Term Level Annotated Significant Expected Fishers Ontology
1 GO:0000706 meiotic DNA double-strand break processi...    11         2           1     0.00  0.0010       BP
2 GO:0017126                             nucleologenesis     8         5           1     0.00  0.0026       BP
3 GO:0010555                        response to mannitol     6        11           1     0.01  0.0057       BP
4 GO:0016121                  carotene catabolic process     9        13           1     0.01  0.0068       BP
5 GO:1905786 positive regulation of anaphase-promotin...    13        14           1     0.01  0.0073       BP
6 GO:0005975              carbohydrate metabolic process     4      2529           5     1.32  0.0086       BP
```


You can also combine them to a single table using `rbind` if you wish:

```r
myGenesGO.table <- (rbind(myGenes_BP.table, myGenes_MF.table, myGenes_CC.table))
myGenesGO.table <- myGenesGO.table[order(myGenesGO.table$Fishers),]
```


## 6. Plot the results


For plotting, we will use `ggplot2` and `tidyverse` packages. If you are running this on multiple lists, you can create a function and use that to plot the results. Two visualizations that are easy to interpret are Bubble plot and Bar plot. 

### Bubble plot

```r
myBubblePlot <- function(table = myGenes_BP.table, term = "BP") {
  ggplot(table,
         aes(
           x = Term,
           y = -log10(Fishers),
           size = Significant,
           fill = -log10(Fishers)
         )) +
    expand_limits(y = 1) +
    geom_point(shape = 21) +
    #   scale_size(range = c(2.5, 12.5)) +
    scale_fill_continuous(low = 'royalblue', high = 'tomato') +
    xlab('') + ylab('-log10 (p value') +
    labs(title = paste0('GO Analysis (', term, ")")) +
    geom_hline(
      yintercept = c(-log10(0.05), -log10(0.01), -log10(0.001)),
      linetype = c("dotted", "dotted", "dotted"),
      colour = c("black", "black", "black"),
      size = c(1, 1, 1)
    ) +
    theme_bw(base_size = 12) +
    theme(
      legend.position = 'right',
      legend.background = element_rect(),
      plot.title = element_text(vjust = 1),
      plot.subtitle = element_text(vjust = 1),
      plot.caption = element_text(vjust = 1),
      
      axis.text.x = element_text(hjust = 1.10),
      axis.text.y = element_text(vjust = 0.5),
      axis.line = element_line(colour = 'black'),
      legend.key = element_blank(),
      legend.key.size = unit(1, "cm"),
    ) +
    coord_flip()
}
```

### Bar plot

```r
myBarPlot <- function(table = myGenes_BP.table, term = "BP") {
  table <- table %>% mutate(word = case_when(Significant == 1 ~ "hit",
                                             TRUE ~ "hits"))
  ggplot(table, aes(reorder(Term, -log10(Fishers)),
                    -log10(Fishers), fill = Ontology)) +
    geom_bar(stat = 'identity') +
    scale_fill_manual(values = c(
      "BP" = "#9531b8",
      "MF" = "#9dd745",
      "CC" = "#da4218"
    )) +
    geom_text(aes(label = paste(Significant, word, "of", Annotated)),
              hjust = 2,
              color = "white") +
    theme_classic(base_size = 14) +
    ylab("- log10 p-value") +
    xlab("") +
    labs(title = paste0('GO Analysis (', term, ")")) +
    scale_y_continuous(expand = expansion(mult = c(0, .1)),
                       breaks = scales::pretty_breaks()) +
    theme(legend.position = "none") +
    coord_flip()
}
```

To run them:

```r
myBubblePlot(table = myGenes_BP.table, term = "CC")
```
![Barplot](/assets/mybubbleplot.png)



```r
myBarPlot(table = myGenes_BP.table, term = "CC")
```

![Barplot](/assets/mybarplot.png)

You can export the plots as pdf or png using `ggsave` function. 

```r
ggsave("mybarplot.png")
```


There are various other visualizations as well, but these two are the most common ones. I hope this guide was helpful. If you have any questions, please feel free to contact me.
