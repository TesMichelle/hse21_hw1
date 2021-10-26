
# commands
```
$ ls /usr/share/data-minor-bioinf/assembly/* | xargs -tI{} ln -s {}
```
---
```
$ eqtk sample -s 10052001 oil_R1.fastq 5000000 > PE1.fq
$ eqtk sample -s 10052001 oil_R2.fastq 5000000 > PE2.fq
$ seqtk sample -s 10052001 oilMP_S4_L001_R1_001.fastq 1500000 > MP1.fq
$ seqtk sample -s 10052001 oilMP_S4_L001_R2_001.fastq 1500000 > MP2.fq
```
---

```
$ rm *.fastq
$ ls -1 | xargs -tI{} fastqc {}
$ mkdir fastqc_raw
$ ls *.html -1 | xargs -tI{} mv -v {} fastqc_raw
$ ls *.zip -1 | xargs -tI{} mv -v {} fastqc_raw
$ multiqc fastqc_raw -o multiqc_raw
```

---
```
$ platanus_trim PE1.fq PE2.fq
$ platanus_internal_trim MP1.fq MP2.fq
```
```
$ mkdir fastqc_trimmed
$ ls *trimmed -1 | xargs -tI{} fastqc {} -o fastqc_trimmed
$ multiqc fastqc_trimmed -o multiqc_trimmed
```
---
```
$ screen -S mashishkin_1_assemble
$ bash
$ platanus assemble -o Poil -f PE1.fq.trimmed PE2.fq.trimmed 2> assemble.log
```
```
$ platanus scaffold -o Poil -c Poil_contig.fa -IP1 PE1.fq.trimmed PE2.fq.trimmed -OP2 MP1.fq.int_trimmed $ MP2.fq.int_trimmed 2> scaffold.LOGFILE
```
```
$ platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 PE1.fq.trimmed PE2.fq.trimmed -OP2 MP1.fq.int_trimmed $ MP2.fq.int_trimmed 2> gap_close.log
```
# Add.
seqtk sample -s 10052001 oil_R1.fastq 0.2 > PE1_.fq
seqtk sample -s 10052001 oil_R1.fastq 0.2 > PE2_.fq
seqtk sample -s 10052001 oilMP_S4_L001_R1_001.fastq 0.2 > MP1_.fq
seqtk sample -s 10052001 oilMP_S4_L001_R2_001.fastq 0.2 > MP2_.fq

platanus_trim PE1_.fq PE2_.fq
platanus_internal_trim MP1_.fq MP2_.fq

platanus assemble -o Poil_ -f PE1_.fq.trimmed PE2_.fq.trimmed 2> assemble.log
platanus scaffold -o Poil -c Poil__contig.fa -IP1 PE1_.fq.trimmed PE2_.fq.trimmed -OP2 MP1_.fq.int_trimmed MP2_.fq.int_trimmed 2> scaffold.LOGFILE
platanus gap_close -o Poil -c Poil__scaffold.fa -IP1 PE1_.fq.trimmed PE2_.fq.trimmed -OP2 MP1_.fq.int_trimmed MP2_.fq.int_trimmed 2> gap_close.log
