---
title: "Expand rows using awk"
tags: [awk]
layout: post
---

Several times, we tend to collapse and expand rows based on group information. In this note, let us learn to expand the collapsed rows. Following is an example input:

```bash
input:

a	1,2,3
b	horse
```
Now we need to expand the rows in a single row for each value in second column
```bash
$ awk  -v OFS="\t"  '{split($2,a,","); for (i in a) {print $1,a[i]}}' test.txt

a	1
a	2
a	3
b	horse
b	donkey
b	dog
```
Trick is in splitting the second column, store the values in an array and then print the values for each element in the array. Let us look at few real life examples.

```bash
input:
$cat test.txt 

41867799,41867388,41866990  chr17:41866927-41867904
```
awk code and output
```bash
$awk -v FS='[-:\t]' -v OFS="\t"  ' NR != 1 {split($1,a,","); for (i in a) {print $2,$3"-"a[i],$3,a[i]}}' test.txt

chr17   41866927-41867799   41866927    41867799
chr17   41866927-41867388   41866927    41867388
chr17   41866927-41866990   41866927    41866990
```
Another example
```bash
input:

ID                 GENE              Qvalue
T0100007038  NECAP2            1.17
T0100007063  FAM231C; FAM231A  -1.04
T0100007206  CDA               -1.15
T0100007207  PINK1; MIR6084    1.1
```
Code and output
```bash
awk -v OFS="\t" '{split($2,a,";"); for(i in a)print $1,a[i],$3}' test.txt 

ID  GENE     Qvalue
T0100007038   NECAP2   1.17
T0100007063   FAM231C      -1.04
T0100007063   FAM231A      -1.04
T0100007206   CDA      -1.15
T0100007207   PINK1    1.1
T0100007207   MIR6084      1.1
```
