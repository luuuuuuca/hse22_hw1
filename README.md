# hse_hw1
## Обязательная часть
### Ярлыки для доступа к данным
____
```
ln -s /usr/share/data-minor-bioinf/assembly/oil_R1.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oil_R2.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R2_001.fastq
ln -s /usr/share/data-minor-bioinf/assembly/oilMP_S4_L001_R1_001.fastq
```
### С помощью команды seqtk выбираем случайно 5 миллионов чтений типа paired-end и 1.5 миллиона чтений типа mate-pairs 
```
seqtk sample -s722 oil_R1.fastq 5000000 > sub1.fastq
seqtk sample -s722 oil_R2.fastq 5000000 > sub2.fastq
seqtk sample -s722 oilMP_S4_L001_R1_001.fastq 1500000 > matep1.fastq
seqtk sample -s722 oilMP_S4_L001_R2_001.fastq 1500000 > matep2.fastq
```
### Оценка чтений FastQC и создание отчета через MultiQC
```
mkdir fastqc
ls sub* matep* | xargs -tI{} fastqc -o fastqc {}
