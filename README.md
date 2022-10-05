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
mkdir multiqc
multiqc -o multiqc fastqc
```
#### (опечаталась в самом начале) 
![ty](https://github.com/luuuuuuca/hse22_hw1/blob/main/Снимок%20экрана%202022-10-06%20в%2000.27.12.png)
![rt](https://github.com/luuuuuuca/hse22_hw1/blob/main/Снимок%20экрана%202022-10-06%20в%2000.31.50.png)
### Обрезание чтений 
```
platanus_trim sub*
platanus_internal_trim matep*
```
### Удаление первоначальных файлов
```
rm sub1.fastq
rm sub2.fastq
rm matep1.fastq 
rm matep2.fastq
```
### Оценка качества обрезанных чтений через FastQC
```
mkdir fastqc_trimmed
ls sub* matep*| xargs -tI{} fastqc -o fastqc_trimmed {}
```
### Создание отчета через MultiQC
```
mkdir multiqc_trimmed
multiqc -o multiqc_trimmed fastqc_trimmed
![ui](https://github.com/luuuuuuca/hse22_hw1/blob/main/Снимок%20экрана%202022-10-06%20в%2002.15.12.png)
```
### Сбор континг из подрезанных чтений через platanus assemble
```
time platanus assemble -o Poil -f sub1.fastq.trimmed sub2.fastq.trimmed 2> assemble.log
```
### Сбор скаффолдов из подрезанных чтений через platanus scaffold
```
time platanus scaffold -o Poil -c Poil_contig.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matepairs.fastq.int_trimmed matepairs2.fastq.int_trimmed 2> scaffold.log
```
### Уменьшение числа промежутков
```
time platanus gap_close -o Poil -c Poil_scaffold.fa -IP1 sub1.fastq.trimmed sub2.fastq.trimmed -OP2 matep1.fastq.int_trimmed matep2.fastq.int_trimmed 2> gapclose.log
```
### Удаление подрезанных чтений
```
rm matepairs.fastq.int_trimmed
rm matepairs2.fastq.int_trimmed
rm sub1.fastq.trimmed
rm sub2.fastq.trimmed
```





