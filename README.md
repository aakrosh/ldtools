## Introduction

This suite consists of two programs: pagetag and senatag. They can be used to analyze the patterns of linkage disequilibrium (LD) between polymorphic sites in a locus. They bin the SNPs on the basis of a threshold level of LD as measured by r<sup>2</sup>.

The algorithm employed by pagetag and senatag is the same as used in ldSelect (Carlson et al., 2004). However it is designed to be much faster and more efficient than ldSelect.

The input genotype information is supplied to the pagetag module, which generates two intermediate outputs. They then serve as input for senatag, which outputs a list of tag SNPs.

## Usage and Flags

<pre>pagetag [options] input.txt snps.txt neighborhood.txt
    where input.txt is the prettybase file
    where the options are:
        -h,--help : print usage and quit
        -r,--rsquare: the rsquare threshold (default : 0.64)
        -f,--freq : the minimum allele frequency threshold (default: 0.0)
        -s,--sample : a list of samples to be clustered (default: all)</pre>

input.txt is the required input prettybase file. The prettybase file os a tab-delimited text file with the SNPs genotype information in the following columns:

<pre>        Site    Sample  Allele1 Allele2</pre>

for example:

<pre>        rs2334386 NA20364 G T
        rs2334386 NA20363 G G
        rs2334386 NA20360 G G
        rs2334386 NA20359 G G
        rs2334386 NA20358 G G
        rs2334386 NA20356 G G
        rs2334386 NA20357 G G
        rs2334386 NA20350 N N
        rs2334386 NA20349 G G
        rs2334386 NA20348 G G
        rs2334386 NA20347 G G
        rs2334386 NA20346 G G
        rs2334386 NA20345 G G
        rs2334386 NA20344 G G
        rs2334386 NA20342 G G</pre>

-r [Optional]  
Specifies the r<sup>2</sup> threshold of LD for the SNPs to be in the same bin. It can be a value in the interval [0.00, 1.00]. The default value is set to be 0.64.

-f [Optional]  
Specifies the threshold minor allele frequency (MAF) for a loci to be clustered. The default is set to be 0.00\. The permissible values are in the range (0.00, 0.50]

-s [Optional]  
Specifies a file which contains the names of the samples to be clustered. By default, all the samples in the prettybase file are clustered.

-h [Optional]  
Just print the help information for the module and exit.

The pagetag module creates two output files, snps.txt and neighborhood.txt. The file snps.txt contains the name of the SNPs that are to be considered for clustering, one on each line. The file neighborhood.txt consists of two columns: the first column contains the name of a SNP to be clustered, and the second column consists of a comma-separated list of all the other SNPs that have a r<sup>2</sup> greater than or equal to the threshold specified by the user.

<pre>senatag [options] neighborhood.txt snps.txt
    where snps.txt is a file of snps from one population (output from pagetag)
    where neighborhood.txt is neighborhood details for the population (output from  pagetag).
    where the options are:
        -h,--help : print usage and quit
        -e,--excluded : file with names of SNPs that cannot be TagSNPs
        -r,--required : file with names of SNPs that should be TagSNPs</pre>

-h [Optional]  
Just print the help information for the module and exit.

-e [Optional]  
Specifies a file, which has a list of SNPs that should not be used as tag SNPs. By default, all SNPs are considered to be candidate tag SNPs.

-r [Optional]  
Specifies a file, which has a list of SNPs that should be output as tag SNPs.

The senatag module prints the output on the screen, which comprises of two columns. The first column is the SNP that should be used as the tag SNP, whereas the second column is a comma-separated list of all the SNPs that are in LD with the called tag SNP.

## Comparison with ldSelect

The two modules essentially implement the same algorithm as ldSelect. The implementation is however designed to be more efficient, so that we can handle a higher number of samples and loci. This is primarily achieved by use of heap queues to calculate the next tag SNP after one has been chosen.

We evaluated our software using HapMap SNPs from a 4 Mb region on chr22 across several populations. The table details the results from the comparison of our implementations with ldSelect. Our implementation of the greedy algorithm returns the same number of tag SNPs as ldSelect, however it is much faster and uses fewer resources than that required by ldSelect. For three different datasets, we compare the number of tag SNPs identified and the time taken by ldSelect and our efficient implementation of the same algorithm as used by ldSelect. Only SNPs with a minor allele frequency greater than or equal to 0.05 were considered and two SNPs were in LD if the r<sup>2</sup> between them was equal to or greater than 0.8

<table border="1"><caption>Comparison of ldSelect and our implementation</caption>

<tbody>

<tr>

<th>Number of loci</th>

<th>Number of individuals</th>

<th>Number of tagSNPs</th>

<th>ldSelect(s)</th>

<th>Greedy approach(s)</th>

</tr>

<tr>

<td>1,317</td>

<td>83</td>

<td>808</td>

<td>600</td>

<td>26.2</td>

</tr>

<tr>

<td>1,094</td>

<td>85</td>

<td>464</td>

<td>235</td>

<td>19.5</td>

</tr>

<tr>

<td>1,208</td>

<td>88</td>

<td>555</td>

<td>450</td>

<td>27.7</td>

</tr>

</tbody>

</table>

## Installation

The modules are written in the Python programming language and should run on any system with >= python-2.4 installed.

## References

<pre>1\. Carlson CS, Eberle MA, Rieder MJ, Yi Q, Kruglyak L, Nickerson DA. 
Selecting a maximally informative set of single-nucleotide polymorphisms for 
association analysis using linkage disequilibrium. Am J Hum Genet. 2004 Jan; 
74(1):106-20\. Epub 2003 Dec 15.</pre>

## To-do

<pre>1\. Add suuport to handle data from multiple populations.
2\. Add support for -context, -gtPercentCluster, -gtPercentTagSnp flags of ldSelect.</pre>
