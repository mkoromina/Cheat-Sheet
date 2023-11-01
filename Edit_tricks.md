
## Get the maximum and minimum value of a column (.gz file) 

```
gzcat file.gz | awk 'NR == 1 || $3 < min {line = $0; min = $3}END{print line}'
```
returns the minimum value of the 3rd column

```
gzcat file.gz | awk -v max=0 'FNR > 1 {if($3>max){want=$0; max=$3}}END{print want}'
```
returns the maximum value of the 3rd column


## Edit GWAS summary stats (here in .tsv format) by renaming certain column(s)

```
 sed -i -e 's/NCON/N_controls/' munged_sumstats.tsv
```

## Convert your GWAS summary stats to the GCTA .cojo format status

```
for i in *.sumstats
do
awk '{print $1, $5, $6, $12, $10, $11, $4, $7}'  OFS='\t' ${i} > ${i}.gcta.cojoformat
done
```


## Add header to any file in unix

```
echo -e "PROBE\tSNP\tBP\tP\tBETA" | cat -  file.txt  >  file.headers.txt 
```

## Split a column based on a string character in Unix

```
awk '{sub (/;/, OFS)} 1' OFS="\t"  file.headers.txt >  file.headers2.txt
```

## Merge multiple .txt files and keep the header of the 1st file

```
head -1 0.file.txt > file.txt
tail -qn +2 {0..1000}.file.txt >> file.txt
```

## Extract the line from a file (here COJO output) based on the min value of a certain column (here column 13)

```
for i in gwas_chr*.ma_5e-6.cma.cojo
do
awk 'NR == 1 {line = $0; min = $13}
     NR > 1 && $13 < min {line = $0; min = $13}
     END{print line}' "$i"
     done
```

## Print your count of jobs in Minerva
Number of jobs you are running (private, premium, etc)

```
bjobs -u all | grep private | awk '{print $2}' | sort | uniq -c
bjobs -u all | grep private | grep RUN | awk '{print $2}' | sort | uniq -c
bjobs | grep PEND | awk '{print $1}'
```

## Change the Minerva job submission parameters for a list of jobs

```
for jobID in `bjobs -u koromm03 | grep PEND | awk '{print $1}'`
do bmod -W .. -n ... -R ... $jobID
done
```
Example ```bmod -R rusage[mem=80GB] -R himem  jobnumber```

## File permissions
### Individual permissions
```
chmod 777 #simple read, write, execute
setfacl -m u:usr:rwx,m::rwx /path/to/file

setfacl -Rm u:usr:rwx,m::rwx /dir
setfacl -Rdm u:usr:rwx,m::rwx /dir

setfacl -m u:usr:rwx,u:usr2:rwx,m::rwx
```

### Group permissions 
```
setfacl -Rm g:groupname:rwx,m::rwx /dir
setfacl -Rdm g:groupname:rwx,m::rwx /dir
```





