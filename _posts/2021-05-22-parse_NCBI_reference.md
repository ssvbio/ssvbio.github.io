---
layout: post
title: Parse NCBI human reference file to a standard reference file
tag: NCBI seqkit grep
---

Many bioinformaticians/ computational biologists will make a common mistake once or twice in their career. One such is to mix reference files. For eg. reference fill will have chromosomes denoted by numbers (1,2,3 etc), and vcf file is denoted by alphanumeric string (chr1, chr2, chr3 etc). To add to this, each bioinformaticis server has it's own formatting for reference files. Let us take NCBI for today. I have downloaded recent GRCh38 assembly from [here](https://ftp.ncbi.nlm.nih.gov/refseq/H_sapiens/annotation/GRCh38_latest/refseq_identifiers/). Headers of the sequences in file is  a long list and few of them are as follows.

```bash

NC_000001.11 Homo sapiens chromosome 1, GRCh38.p13 Primary Assembly
NT_187361.1 Homo sapiens chromosome 1 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR1_CTG1_UNLOCALIZED
NT_187362.1 Homo sapiens chromosome 1 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR1_CTG2_UNLOCALIZED
NT_187363.1 Homo sapiens chromosome 1 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR1_CTG3_UNLOCALIZED
NT_187364.1 Homo sapiens chromosome 1 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR1_CTG4_UNLOCALIZED
NT_187365.1 Homo sapiens chromosome 1 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR1_CTG5_UNLOCALIZED
NT_187366.1 Homo sapiens chromosome 1 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR1_CTG6_UNLOCALIZED
NT_187367.1 Homo sapiens chromosome 1 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR1_CTG7_UNLOCALIZED
NT_187368.1 Homo sapiens chromosome 1 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR1_CTG8_UNLOCALIZED
NT_187369.1 Homo sapiens chromosome 1 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR1_CTG9_UNLOCALIZED
NC_000002.12 Homo sapiens chromosome 2, GRCh38.p13 Primary Assembly
NT_187370.1 Homo sapiens chromosome 2 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR2_RANDOM_CTG1
NT_187371.1 Homo sapiens chromosome 2 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR2_RANDOM_CTG2
NC_000003.12 Homo sapiens chromosome 3, GRCh38.p13 Primary Assembly
NT_167215.1 Homo sapiens chromosome 3 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR3UN_CTG2
NC_000004.12 Homo sapiens chromosome 4, GRCh38.p13 Primary Assembly
NT_113793.3 Homo sapiens chromosome 4 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR4_RANDOM_CTG4
NC_000005.10 Homo sapiens chromosome 5, GRCh38.p13 Primary Assembly
NT_113948.1 Homo sapiens chromosome 5 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR5_RANDOM_CTG1
NC_000006.12 Homo sapiens chromosome 6, GRCh38.p13 Primary Assembly
NC_000007.14 Homo sapiens chromosome 7, GRCh38.p13 Primary Assembly
NC_000008.11 Homo sapiens chromosome 8, GRCh38.p13 Primary Assembly
NC_000009.12 Homo sapiens chromosome 9, GRCh38.p13 Primary Assembly
NT_187372.1 Homo sapiens chromosome 9 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR9_UNLOCALIZED_CTG1
NT_187373.1 Homo sapiens chromosome 9 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR9_UNLOCALIZED_CTG2
NT_187374.1 Homo sapiens chromosome 9 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR9_UNLOCALIZED_CTG3
NT_187375.1 Homo sapiens chromosome 9 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR9_UNLOCALIZED_CTG4
NC_000010.11 Homo sapiens chromosome 10, GRCh38.p13 Primary Assembly
NC_000011.10 Homo sapiens chromosome 11, GRCh38.p13 Primary Assembly
NT_187376.1 Homo sapiens chromosome 11 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR11_CTG1_UNLOCALIZED
NC_000012.12 Homo sapiens chromosome 12, GRCh38.p13 Primary Assembly
NC_000013.11 Homo sapiens chromosome 13, GRCh38.p13 Primary Assembly
NC_000014.9 Homo sapiens chromosome 14, GRCh38.p13 Primary Assembly
NT_113796.3 Homo sapiens chromosome 14 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR14_CTG1_UNLOCALIZED
NT_167219.1 Homo sapiens chromosome 14 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR14_CTG2_UNLOCALIZED
NT_187377.1 Homo sapiens chromosome 14 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR14_CTG3_UNLOCALIZED
NT_113888.1 Homo sapiens chromosome 14 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR14_CTG4_UNLOCALIZED
NT_187378.1 Homo sapiens chromosome 14 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR14_CTG5_UNLOCALIZED
NT_187379.1 Homo sapiens chromosome 14 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR14_CTG6_UNLOCALIZED
NT_187380.1 Homo sapiens chromosome 14 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR14_CTG7_UNLOCALIZED
NT_187381.1 Homo sapiens chromosome 14 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR14_CTG8_UNLOCALIZED
NC_000015.10 Homo sapiens chromosome 15, GRCh38.p13 Primary Assembly
NT_187382.1 Homo sapiens chromosome 15 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR15_RANDOM_CTG1
NC_000016.10 Homo sapiens chromosome 16, GRCh38.p13 Primary Assembly
NT_187383.1 Homo sapiens chromosome 16 unlocalized genomic scaffold, GRCh38.p13 Primary Assembly HSCHR16_RANDOM_CTG1

``` 

Now we need to extract only chromosome and mitochondria related and convert the headers to following format

```bash
chr 1
chr 2
chr 3
chr 4
chr 5
chr 6
chr 7
chr 8
chr 9
chr 10
chr 11
chr 12
chr 13
chr 14
chr 15
chr 16
chr 17
chr 18
chr 19
chr 20
chr 21
chr 22
chr X
chr Y
chr MT
```

Now let us do this in steps:

```bash

$ seqkit grep -vrinp "scaffold|novel|fix" GRCh38_latest_genomic.fna | sed -r '/^>/ s/.*\s(.*),\s.*/>chr \1/;s/mitochondrion/MT/' > grch38_chr_only.fna 

```
Now let us check the headers

```bash

$seqkit seq -n grch38_chr_only.fna 
chr 1
chr 2
chr 3
chr 4
chr 5
chr 6
chr 7
chr 8
chr 9
chr 10
chr 11
chr 12
chr 13
chr 14
chr 15
chr 16
chr 17
chr 18
chr 19
chr 20
chr 21
chr 22
chr X
chr Y
chr MT
```

Now if we want to have only numbered (without `chr`) fastas, let us try this:

```bash
$ seqkit grep -vrinp "scaffold|novel|fix" GRCh38_latest_genomic.fna | sed -r '/^>/ s/.*\s(.*),\s.*/>\1/;s/mitochondrion/MT/' > GRCh38_latest_genomic_int.fna

```

