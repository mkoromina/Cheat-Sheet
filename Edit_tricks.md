
## Get the maximum and minimum value of a column (.gz file) 

```
gzcat file.gz | awk 'NR == 1 || $3 < min {line = $0; min = $3}END{print line}'
```
returns the minimum value of the 3rd column

```
gzcat file.gz | awk -v max=0 'FNR > 1 {if($3>max){want=$0; max=$3}}END{print want}'
```
returns the maximum value of the 3rd column


## Edit GWAS summary stats (.tsv format) by renaming certain column(s)

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


## Add header to any file in Unix

```
echo -e "PROBE\tSNP\tBP\tP\tBETA" | cat -  file.txt  >  file.headers.txt 
```

## Split a column based on a string character in Unix

```
awk '{sub (/;/, OFS)} 1' OFS="\t"  file.headers.txt >  file.headers2.txt
```

## Conditional filtering using awk commands
Let's assume that we have some ICD codes are stored in icd_codes.txt (only one column) and our data file is data.txt.
We use awk to filter the data file based on exact matches in the third column with icd_codes.txt.

```
awk 'NR==FNR{icd[$0]; next} $3 in icd' icd_codes.txt data.txt
```

## Conditional filtering using zgrep command
Let's assume that we have a gzipped file and we wish to filter this based on entries in snpids.txt (only one col).

```
zgrep -Fwf snpids.txt your_gzipped_file.gz | gzip > filtered_output.gz
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

## Print your count of jobs in a LSF job submission system
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
### Individual & Group permissions
```
chmod 777 #simple read, write, execute
chmod -R g+rwx /path/to/directory #modify group permissions
chmod -R u+rwx /path/to/directory #modify user permissions

setfacl -dm u:usr:rwx /path/to/directory #assign permissions to a specific user for a given dir
setfacl -b /path/to/directory #remove group permissions from a dir

```

