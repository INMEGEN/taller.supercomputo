# Ejemplo práctico


Primer consejo: Siempre documentar. 


## README

### Proyecto: Búsque da variantes de un solo nucleótido (SNPs) en secuencia de DNA

Somera descripción del proyecto. Origen de las muestras, datos de la secuenciación, objetivos del análisis, notas sobre literatura que leer acerca de los detalles técnicos. Razones y evidencia de porqué elegir un algoritmo sobre otro.

Sobre Freebayes:

FreeBayes es un detector Bayesiano de variantes genéticas diseñado para encontrar pequeños polimorfismos, específicamente SNPs (*single-nucleotide polymorphisms*), indels (inserciones y deleciones), MNPs (*multi-nucleotide polymorphisms*) y ventos complejos (inserciones compuestas y eventos de sustitución) más pequeñas que la longitud de una alineación de secuencia de lectura corta.

Publicación: <http://arxiv.org/abs/1207.3907>


La pipeline sigue las sugerencias del algoritmo freebayes que está publicado en su página:

<https://github.com/ekg/freebayes>

**Datos y formato de entrada:**

Son cuatro muestras pareadas de XXXX en formato fastq.


## Pipeline:


### 001 - mk\_bwa_align

Alineamiento de secuencias con el algoritmo de Burrows-Wheeler. 

Formato de entrada de este módulo: fastq

Formato de salida: BAM

<http://bio-bwa.sourceforge.net/bwa.shtml>


### 002 - mk-indexbam

Indexa y ordena con samtools. Necesario para el siguiente paso.


### 003 - mk_rmdup

El autor del algoritmo recomienda marcar duplicados si es que las muestras fueron procesadas con PCR.

### 004 - mk-indexbam

Indexa y ordena de nuevo con samtools. Necesario para el siguiente paso.

### 005 - mk\_freebayes_varcall

El llamado de variantes en sí. La entrada son archivos BAM ordenados e indexados y la salida son archivos VCF, formato estándar para reportar variantes.

____

### datos:

```
/home/alumno63/variantcaller/data
```

### Referencia

```
/reference/ftp.broadinstitute.org/bundle/hg38/Homo_sapiens_assembly38.fasta
```


### repositorios

```
https://github.com/INMEGEN/mk_bwa_align.git
https://github.com/INMEGEN/mk-indexbam.git
https://github.com/INMEGEN/mk_rmdup.git
https://github.com/INMEGEN/mk_freebayes_varcall.git
```

