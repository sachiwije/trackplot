## Introduction

`trackplot.R` is a fast, simple, and minimal dependency R script to generate IGV style track plots (aka locus plots) from bigWig files.

### Usage

```r
download.file(url = "https://raw.githubusercontent.com/PoisonAlien/trackplot/master/trackplot.R", destfile = "trackplot.R")
source('trackplot.R') 

#1. Basic usage
trackplot(bigWigs = bigWigs, loci = "chr3:187,715,903-187,752,003")

#2. With gene models (by default autoamtically queries UCSC genome browser for hg19 transcripts)
trackplot(bigWigs = bigWigs, loci = "chr3:187,715,903-187,752,003", draw_gene_track = TRUE, build = "hg38")

#3. With GTF file as source for gene models
trackplot(bigWigs = bigWigs, loci = "chr3:187,715,903-187,752,003", draw_gene_track = TRUE, gene_model = "hg38_refseq.gtf.gz", isGTF = TRUE)

#4. Heighlight regions of interest

## Example regions to heighlight (optional)
markregions = data.frame(
    chr = c("chr3", "chr3"),
    start = c(187743255, 187735888),
    end = c(187747473, 187736777),
    name = c("Promoter-1", "Promoter-2")
  )
  
trackplot(
  bigWigs = c("CD34.bw", "EC.bw", "LC.bw", "CD4p.bw", "CD8p.bw"),
  loci = "chr3:187,715,903-187,752,003",
  draw_gene_track = TRUE,
  build = "hg38",
  mark_regions = markregions,
  custom_names = c("CD34", "EC", "LC", "CD4+", "CD8+")
)
```

<img src="example.png" /></a>


### Features

 * Fast - Above example plot took less than a minute on my 5 year old [macbook Pro](https://support.apple.com/kb/sp715?locale=en_GB) 
 * Automatically queries UCSC genome browser for gene models.
 * Supports GTF and standard UCSC gene formats as well.
 * Customization: Each track can be customized for color, scale, and width.
 * Minimal dependency. Plots are generated in pure base R graphics. 

### Dependencies

`trackplot` has only two dependencies. 

* [data.table](https://cran.r-project.org/web/packages/data.table/index.html) R package - which itself has no dependency.
* [bwtool](https://github.com/CRG-Barcelona/bwtool) - a command line tool for processing bigWig files. Install and move the binary to a PATH (e.g; `/usr/local/bin`). If you have trouble compiling the tool, follow [these](https://gist.github.com/PoisonAlien/e19b482ac6146bfb03142a0de1c4fbc8) instructions. Alternatively, you can download the pre-built binary for [macOS](https://www.dropbox.com/s/kajx9ya6erzyrim/bwtool_macOS.tar.gz?dl=1) or [centOS](https://www.dropbox.com/s/77ek89jqfhcmouu/bwtool_centOS_x86_64.tar.gz?dl=1)
 
### Arguments

Available arguments

```r
#' Generate IGV style locus tracks with ease
#' @param bigWigs bigWig files. Default NULL. Required.
#' @param loci target region to plot. Should be of format "chr:start-end". e.g; chr3:187715903-187752003
#' @param chr chromosome of interest. Default NULL. This argument is mutually exclusive with `loci`
#' @param start start position. Default NULL. This argument is mutually exclusive with `loci`
#' @param end end interest. Default NULL. This argument is mutually exclusive with `loci`
#' @param nthreads Default 1.
#' @param binsize bin size to extract signal. Default 50 (bps).
#' @param draw_gene_track Default FALSE. If TRUE plots gene models overlapping with the queried region
#' @param gene_model File with gene models. Can be a gtf file or UCSC file format. If you have read them into R as a data.frame, that works as well. Default NULL, automatically fetches gene models from UCSC server
#' @param isGTF Default FALSE. Set to TRUE if the `gene_model` is a gtf file.
#' @param tx transcript name to draw. Default NULL. Plots all transcripts overlapping with the queried region
#' @param gene gene name to draw. Default NULL. Plots all genes overlapping with the queried region
#' @param collapse_tx Default FALSE. Whether to collapse all transcripts belonging to same gene into a unified gene model
#' @param gene_fsize Font size. Default 1
#' @param gene_track_height Default 2 
#' @param scale_track_height Default 1
#' @param query_ucsc Default FALSE. But switches to TRUE when `gene_model` is not given.
#' @param build Genome build. Default `hg19`
#' @param col Color for tracks. Default `#2f3640`. Multiple colors can be provided for each track
#' @param groupAutoScale Default TRUE
#' @param show_axis Default FALSE
#' @param custom_names Default NULL and Parses from the file names.
#' @param custom_names_pos Default 0 (corresponds to left corner)
#' @param mark_regions genomic regions to highlight. A data.frame with at-least three columns containing chr, start and end positions.
#' @param mark_regions_col color for highlighted region. Default "#192A561A"
#' @param mark_regions_col_alpha Default 0.5
```

### Caveat

 * Windows OS is not supported
 
![](https://media.giphy.com/media/cKJjGbH7R5KKcJIR5u/giphy.gif)


### Citation

If you find the script useful consider [citing bwtool](https://academic.oup.com/bioinformatics/article/30/11/1618/282756)


*Pohl A, Beato M. bwtool: a tool for bigWig files. Bioinformatics. 2014 Jun 1;30(11):1618-9. doi: 10.1093/bioinformatics/btu056. Epub 2014 Jan 30. PMID: [24489365](https://pubmed.ncbi.nlm.nih.gov/24489365/); PMCID: PMC4029031.*



### Acknowledgements 

[Joschka Hey](https://github.com/HeyLifeHD) for all the cool suggestions :)
