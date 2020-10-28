---
title: Generate overlapping strings
tags: [cran-R "overlapping strings" stringr]
layout: post
---
Some times, for some obscure reason, you are asked to generate strings
of letters from a larger string with restriction on string size and
overlapping Window. For eg. You are asked to generate 3 letter
word/string from “**Apple**” with one alphabet overlap. From Apple, you
would be generationg, **App**, **ppl**, **ple** (one character overlap). You also
would like to put in a data fram for further manipulation. Here is an
example with radom letters of DNA code (ATCG).

``` r
print ("sting is ATCGATCGGGTTTAC")
```

[1] "sting is ATCGATCGGGTTTAC"

Now the requirement is to print 6 letter strings, with one letter
overlap. There are many ways to do it. Let us do it in a simple way.

``` r
library(stringr)
str_match_all("ATCGATCGGGTTTAC", "(?=(.{6}))")[[1]][,2]
```

[1] "ATCGAT" "TCGATC" "CGATCG" "GATCGG" "ATCGGG" "TCGGGT" "CGGGTT" "GGGTTT"

[9] "GGTTTA" "GTTTAC"

Now that we did this, this is temporary stop gap. Now let us make a
general function, where user would provide a character vector (with long
string), length of required of word and overlap window.

``` r
seq="ATCGATCGGGTTTAC"
extract_kmers=function(Seq,Len,Wind){
	df=data.frame(kmer=str_sub(seq,seq(1,nchar(seq)-(Len-1),Len-Wind),seq(Len,nchar(seq),Len-Wind)))
return(df)
}
```

Now let us extract 6 letter length (hexamers) words with one character
overlap

``` r
extract_kmers(seq,6,1)
```

kmer
1. ATCGAT
2. TCGGGT

Now let us extract 6 letter length (hexamers) words with two character
overlap

``` r
extract_kmers(seq,6,2)
```

kmer
1. ATCGAT
2. ATCGGG
3. GGTTTA

Now let us extract 6 letter length (hexamers) words with 5 character
overlap

``` r
extract_kmers(seq,6,5)
```

kmer
1. ATCGAT
2. TCGATC
3. CGATCG
4. GATCGG
5. ATCGGG
6. TCGGGT
7. CGGGTT
8. GGGTTT
9. GGTTTA
10. GTTTAC

Now let us extract 8 letter length (hexamers) words with one character
overlap

``` r
extract_kmers(seq,8,2)
```

 kmer
1. ATCGATCG
2. CGGGTTTA
