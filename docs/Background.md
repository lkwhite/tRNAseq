---
layout: page
title: Background
permalink: /background/
nav_order: 2
---

# Background and prior work
As of the time of this writing (July 2023) three groups have reported tRNA sequencing on Oxford Nanopore platforms:

### [Thomas _et al._, ACS Nano (2021)](https://pubmed.ncbi.nlm.nih.gov/34618430/)

This paper from the UCSC Nanopore group established proof of principle for direct RNA sequencing of  _in vitro_ transcribed and biological _E. coli_ tRNAs on the Oxford MinION platform. The authors defined [alignment parameters](https://github.com/mitenjain/marginAlign-tRNA) for nanopore-sequenced tRNAs, and also tested a variety of combinations of individual purified biological tRNAs, synthetic tRNAs, and total tRNAs ligated with individual adapter species. Finally, this paper describes correlations between known sites of modifications on _E. coli_ tRNAs and mismatches (base calling "errors") at the same and/or neighboring nucleotide positions.

The authors note that most sequencing runs were performed for under 24 hours due to "due to a deterioration in functional channels over time," and that they restarted some experiments up to 5 times due to pore clogging issues. Yields are in the range of 50-100k reads, depending on the sample.

### [White _et al._, RNA (2023)](https://pubmed.ncbi.nlm.nih.gov/36854608/)

This publication from our group applied the approach from Thomas _et al._ above to perform direct tRNA sequencing of _S. cerevisiae_ tRNA on the Oxford MinION platform. We generated tRNA sequencing libraries from five different yeast strains with alterations in tRNA maturation/processing/degradation pathways. We used these tRNA sequencing data to identify analogous basecalling error signals (primarily mismatches) produced by changes in tRNA modifications when tRNA splicing is disrupted, including 2´-phosphates installed during RNA ligation. An GitHub repository for this work can be found [here](https://github.com/hesselberthlab/RNARePore).

NB: despite consultation with Oxford Nanopore and substantial in-lab troubleshooting, in our hands neither changing RNA inputs nor adapter ratios made a substantive impact on either library throughput or the ratio of free adapter to sequencing reads reported in the MinKNOW software during the run.

### [Lucas _et al._, Nature Biotechnology (2023)](https://pubmed.ncbi.nlm.nih.gov/37024678/)

This paper from the Novoa lab elucidated the reason for the perplexing behavior above — the MinKNOW software distinguishes free adapters from RNA reads based primarily on the duration of time a molecule spends transiting the pore. A major contribution of this manuscript is the implementation of a custom configuration for capturing tRNA reads that would otherwise be discarded during sequencing due to their short lengths. As described on the [nano-tRNAseq repository](https://github.com/novoalab/Nano-tRNAseq), this workflow involves first performing a sequencing run using the MinKNOW default configuration, and saving out a bulk fast5 file. This bulk file is then re-run in a "simulation" mode in MinKNOW using a custom .toml file that contains a) alternative parameters for classifying RNA reads and adapter sequences based duration in the pore and b) a path to the bulk fast5 file. Using this approach, the authors report higher yield MinION runs across _S. cerevisiae_ samples with and without deletions of several tRNA modification enzymes, under either normal growth conditions, heat, or oxidative stress. These comparisons enabled them to further characterize basecalling errors produced by select tRNA modifications.