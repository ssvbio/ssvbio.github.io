---
layout: post
title: "Format VEP text output with Miller"
tags: VEP Miller
---
VEP (variant effect predictor) is an often used variant annotation tool. It outputs the results in multiple formats as per user supplied parameters. One such format is text (tab separated) format. However, some times, parsing VEP output could be time consuming for newbies. One of the newbie question to format VEP output is as follows:
```code
IMPACT=MODIFIER;DISTANCE=1246;STRAND=-1;BIOTYPE=transcribed_pseudogene;REFSEQ_MATCH=rseq_mrna_match;GIVEN_REF=T;USED_REF=T;HGVSg=chr1:g.13116T>G;AF=0.0971;AFR_AF=0.0295;AMR_AF=0.121;EAS_AF=0.0248;EUR_AF=0.1869;SAS_AF=0.1534;MAX_AF=0.1869;MAX_AF_POPS=EUR
IMPACT=MODIFIER;STRAND=1;BIOTYPE=transcribed_pseudogene;REFSEQ_MATCH=rseq_mrna_match;GIVEN_REF=T;USED_REF=T;HGVSc=NR_046018.2:n.464-105T>G;HGVSg=chr1:g.13116T>G;AF=0.0971;AFR_AF=0.0295;AMR_AF=0.121;EAS_AF=0.0248;EUR_AF=0.1869;SAS_AF=0.1534;MAX_AF=0.1869;MAX_AF_POPS=EUR
IMPACT=MODIFIER;DISTANCE=4253;STRAND=-1;BIOTYPE=miRNA;REFSEQ_MATCH=rseq_mrna_match;GIVEN_REF=T;USED_REF=T;HGVSg=chr1:g.13116T>G;AF=0.0971;AFR_AF=0.0295;AMR_AF=0.121;EAS_AF=0.0248;EUR_AF=0.1869;SAS_AF=0.1534;MAX_AF=0.1869;MAX_AF_POPS=EUR
```
One can write a program or use a regular program that can parse kvp (key-value pairs) text or write a script. However, the different KVPs in each line, make programmer life difficult. Such kind of uneven dkvp files are well handled by **miller** program and is available in most of the ubuntu/debian repos.

Following is the code to format above input:
```code

$ mlr --d2p  --ifs ";"  unsparsify --fill-with "NA" file.txt                                                                                                                         

IMPACT   DISTANCE STRAND BIOTYPE                REFSEQ_MATCH    GIVEN_REF USED_REF HGVSg           AF     AFR_AF AMR_AF EAS_AF EUR_AF SAS_AF MAX_AF MAX_AF_POPS HGVSc
MODIFIER 1246     -1     transcribed_pseudogene rseq_mrna_match T         T        chr1:g.13116T>G 0.0971 0.0295 0.121  0.0248 0.1869 0.1534 0.1869 EUR         NA
MODIFIER NA       1      transcribed_pseudogene rseq_mrna_match T         T        chr1:g.13116T>G 0.0971 0.0295 0.121  0.0248 0.1869 0.1534 0.1869 EUR         NR_046018.2:n.464-105T>G
MODIFIER 4253     -1     miRNA                  rseq_mrna_match T         T        chr1:g.13116T>G 0.0971 0.0295 0.121  0.0248 0.1869 0.1534 0.1869 EUR         NA

```