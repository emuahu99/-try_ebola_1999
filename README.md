# -try_ebola_1999
scripts for ebola 1999 


* bwa package installation:
```
cat src
curl -OL http://downloads.sourceforge.net/project/bio-bwa/bwa-0.7.17.tar.bz2
tar jxvf bwa-0.7.17.tar.bz2
cd bwa-0.7.17/
make
ln -s ~/src/bwa-0.7.17/bwa ~/bin
```

* bwa index: make a folder for reference gene and downlaod the ebola 1999 gene
```
mkdir -p ~/refs/852
efetch -db nucleotide -id NC_002549 -format fasta > ~/refs/852/ebola-1999.fa
bwa index ~/refs/852/ebola-1999.fa
ls ~/refs/852
```
```
ebola-1999.fa       ebola-1999.fa.ann   ebola-1999.fa.pac
ebola-1999.fa.amb   ebola-1999.fa.bwt   ebola-1999.fa.sa
```

* Through blast:
```
makeblastdb -in ~/refs/852/ebola-1999.fa -dbtype nucl -parse_seqids
ls ~/refs/852
```
```
ebola-1999.fa       ebola-1999.fa.nhr   ebola-1999.fa.nsi
ebola-1999.fa.amb   ebola-1999.fa.nin   ebola-1999.fa.nsq
ebola-1999.fa.ann   ebola-1999.fa.nog   ebola-1999.fa.pac
ebola-1999.fa.bwt   ebola-1999.fa.nsd   ebola-1999.fa.sa
```

*make query
``` 
head -2 ~/refs/852/ebola-1999.fa > query.fa
cat results.sam 
```
```
@SQ SN:NC_002549.1  LN:18959
@PG ID:bwa  PN:bwa  VN:0.7.17-r1188 CL:bwa mem /Users/ucco/refs/852/ebola-1999.fa query.fa
NC_002549.1 0   NC_002549.1 1   60  70M *   0   0   CGGACACACAAAAAGAAAGAAGAATTTTTAGGATCTTTTGTGTGCGAATAACTATGAGGAAGATTAATAA  *   NM:i:0  MD:Z:70 AS:i:70 XS:i:0
```

* for comparison of blasting
```
blastn -db ~/refs/852/ebola-1999.fa -query query.fa > results.blastn
```

```
Query= NC_002549.1 Zaire ebolavirus isolate Ebola
virus/H.sapiens-tc/COD/1976/Yambuku-Mayinga, complete genome

Length=70
                                                                      Score        E
Sequences producing significant alignments:                          (Bits)     Value

NC_002549.1 Zaire ebolavirus isolate Ebola virus/H.sapiens-tc/COD...  130        6e-34


>NC_002549.1 Zaire ebolavirus isolate Ebola virus/H.sapiens-tc/COD/1976/Yambuku-Mayinga, 
complete genome
Length=18959

 Score = 130 bits (70),  Expect = 6e-34
 Identities = 70/70 (100%), Gaps = 0/70 (0%)
 Strand=Plus/Plus

Query  1   CGGACACACAAAAAGAAAGAAGAATTTTTAGGATCTTTTGTGTGCGAATAACTATGAGGA  60
           ||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
Sbjct  1   CGGACACACAAAAAGAAAGAAGAATTTTTAGGATCTTTTGTGTGCGAATAACTATGAGGA  60

Query  61  AGATTAATAA  70
           ||||||||||
Sbjct  61  AGATTAATAA  70



Lambda      K        H
    1.33    0.621     1.12 

Gapped
Lambda      K        H
    1.28    0.460    0.850 

Effective search space used: 1079922


  Database: /Users/ucco/refs/852/ebola-1999.fa
```
#vi query.fa
```
cat query.fa 
>NC_002549.1 Zaire ebolavirus isolate Ebola virus/H.sapiens-tc/COD/1976/Yambuku-Mayinga, complete genome
CGGACTTTTTTTTTACACAAAAAGAAAGAAGAATTTTTAGGATCTTTTGTGTGCGAATAACTATGAGGAAGATTAATAA
``
































