# try_ebola_1999
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
```

```
#compasison by bwa mem 
curl -O http://www.personal.psu.edu/iua1/courses/files/2014/data-14.tar.gz
tar xzvf data-14.tar.gz
```
after the tar, it created 2 files, read1.fq and read2.fq. 
```
bwa mem ~/refs/852/ebola-1999.fa read1.fq read2.fq > results.sam
```

```
#convert sam into bam
samtools view -Sb results.sam > temp.bam
# Sort the alignment.
samtools sort temp.bam -oresults.bam
#index the aligment
samtools index results.bam
# using samtools for filter, -f match the flag : remain the reads of match flag
# -F filter the flags: delete the reads of match flag. 
samtools view -f 16 results.bam
samtools view -F 16 results.bam
samtools view -F 16 results.bam |wc -l
samtools view -c -F 16 results.bam
```

```
# uniquely mapping reads.
samtools view -c -q 1 results.bam
20000
```


```
# Count the high quality alignment.
samtools view -c -q 40 results.bam
20000
```

```
# Using readseq transformig data
sudo apt install readseq
cd ~/edu/lec15
# get the full genebanl sequence of ebola 1999
efetch -db nucleotide -id NC_002549.1 -format gb > NC.gb
# get the genbank format, change it into GFF files.
readseq -format=GFF NC.gb
# rename
readseq -format=GFF -o NC-all.gff NC.gb

Choose an output format (name or #): 
1
ubuntu@VM-0-17-ubuntu:~/edu/lec15$ ls
mutations.txt  NC.gb	 read2.fq     results.bam.bai  temp.bam
NC-all.gff     read1.fq  results.bam  results.sam

# get the fasta file:
readseq -format=FASTA -o NC.fa NC.gb

cat NC-all.gff |egrep '\tgene\t'

```









































