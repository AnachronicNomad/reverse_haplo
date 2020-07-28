files.md
======

There are 4 required arguments for inputs, one recommended, and one optional. 

1. **Haplotype** file

This file is a giant ${0|1}$ matrix.  Each row is a SNP (rsID), each column is a haplotype.

```bash
$ head -n 5 ex.haps
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
```

2. **Legend** file

This file describes each row in the haplotypes file, indicating for each snip the rsID+BasePairPosition, and which allele in the base pair is represented by 0 or 1.  
The alleles can be any of the nucleotides {A|C|T||G}, where A/T and C/G are mutually exclusive (they bond to each other).  
However, either of the two base-pair alleles can be present, due to which strand is represented.

```bash
$ head -n 5 ex.leg
rs          position    X0  X1
rs10399749  45162        C   T 
rs4030303   72434        A   G 
rs4030300   72515        A   C 
rs940550    78032        C   T
```

3. **Map** file

Fine-scale recombination rates across region, from HapMap data? 

```bash
$ head -n 5 ex.map 
position COMBINED\_rate(cM/Mb) Genetic\_Map(cM)
45413 -1 0
558185 -1 0
558390 -1 0
711153 2.6858076690 0
```

4. **Disease Loci** arguments

When simulating propagation of a genetic disease, we need to specify which disease SNP from our given haplotype/legend will act as the disease marker.  
Sets of 4 values can be arbitrarily specified after the flag.  This arg has the format 

```bash
[ -dl {POSITION::integer} {ALLELE_ENC::binary} {RR1_HET::float} {RR2_HOM::float}, [POS ALLELE RR1 RR2, [...]] ]
```

where the components are: 

- POSITION
    + DataType :: Integer
    + Base pair physical location as outlined by Legend file
- ALLELE_ENC
    + DataType :: Binary {0|1}
    + The allele base pair variant as specified in Legend file
        * `0` for first allele specified (third column)
        * `1` for second allele (fourth column)
- RR1_HET
    + DataType :: Float
    + Heterozygote relative disease risk
- RR2_HOM
    + DataType :: Float
    + Homozygote relative disease risk

> n.b. the example they give is 
> 
>```bash
>$ ./hapgen ... -dl 1085679 1 1.5 2.25 2190692 0 2 4 ...  
>``` 
>
> such that two disease SNPs are specified within the Legends file: 
> The first at position `1085679`, with disease allele (any of {A|T|C|G}) in the `1` column of the legend, and risks RR1 and RR2 1.5 and 2.25, respectively.  The second disease SNP is at position `2190692` with base-pair allele in the `0` column with risks RR1 and RR2 of 2 and 4, respectively.  