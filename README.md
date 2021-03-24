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

*index: make a folder for reference gene and downlaod the ebola 1999 gene
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



