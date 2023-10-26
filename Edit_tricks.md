
## Get the maximum and minimum value of the 3rd column from a .gz file 

```
gzcat daner_BIP_PGC3_HLA_1KG_dosages_run_repeat.gz | awk 'NR == 1 || $3 < min {line = $0; min = $3}END{print line}'
```
returns the minimum value of the 3rd column

```
gzcat daner_BIP_PGC3_HLA_1KG_dosages_run_repeat.gz | awk -v max=0 'FNR > 1 {if($3>max){want=$0; max=$3}}END{print want}'
```
returns the maximum value of the 3rd column


## Edit GWAS summary stats (here in .tsv format) by renaming certain column(s)

```
 sed -i -e 's/NCON/N_controls/' /Users/koromm03/Desktop/pgc_bip_munged.tsv
```

## Convert your GWAS summary stats to the GCTA .cojo format status

```
for i in *.sumstats
do
awk '{print $1, $5, $6, $12, $10, $11, $4, $7}'  OFS='\t' ${i} > /home/koromina/DENTIST_2/${i}.gcta.cojoformat
done
```


## Add header to any file in unix

```
echo -e "PROBE\tSNP\tBP\tP\tBETA" | cat -  cov.geneHCP100+gPCs20+AllMeta.txt  >  cov.geneHCP100+gPCs20+AllMeta.headers.txt 
```

## Split a column based on a string character in unix

```
awk '{sub (/;/, OFS)} 1' OFS="\t"  cov.geneHCP100+gPCs20+AllMeta.headers.txt >  cov.geneHCP100+gPCs20+AllMeta.headers2.txt
```

## Merge multiple .txt files and keep the header of the 1st file

```
head -1 0.file.txt > file.txt
tail -qn +2 {0..1000}.file.txt >> file.txt
```

## Extract the line from a file (here COJO output) based on the min value of a certain column (here column 13)

```
for i in pgc3_bip_chr*.ma_5e-6.cma.cojo
do
awk 'NR == 1 {line = $0; min = $13}
     NR > 1 && $13 < min {line = $0; min = $13}
     END{print line}' "$i"
     done
```


