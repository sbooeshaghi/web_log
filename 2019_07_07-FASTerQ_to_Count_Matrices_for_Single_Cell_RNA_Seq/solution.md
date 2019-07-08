This dataset is human and 10xv2 chemistry. So we need to download (or build) the human index, dowload (or build) the human transcripts to genes map, and download the 10xv2 whitelist.

The species index and transcripts to genes for common species is available for download [here](https://github.com/pachterlab/kallisto-transcriptome-indices/releases/tag/ensembl-96). We can also download it and the whitelist straight in the command line.

**Note:** command line arguments are preceeded by `$`. For example, if you see `$ cd my_folder` then type `cd my_folder`.

```
$ curl -L https://github.com/pachterlab/kallisto-transcriptome-indices/releases/download/ensembl-96/homo_sapiens.tar.gz -o homo_sapiens.tar.gz
$ curl -L https://github.com/BUStools/getting_started/releases/download/getting_started/10xv2_whitelist.txt -o 10xv2_whitelist.txt
$ gunzip homo_sapiens.tar.gz
$ tar -xvf homo_sapiens.tar
```
The we can copy and paste this one-liner

```
$ mkfifo R1.gz R2.gz; curl -Ls ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR843/004/SRR8437614/SRR8437614_1.fastq.gz > R1.gz & curl -Ls ftp://ftp.sra.ebi.ac.uk/vol1/fastq/SRR843/004/SRR8437614/SRR8437614_2.fastq.gz  > R2.gz & kallisto bus -i homo_sapiens/transcriptome.idx -x 10xv2 -t 4 -o bus_out/ R1.gz R2.gz; cd bus_out/; mkdir tmp/; bustools correct -w ../10xv2_whitelist.txt -p output.bus | bustools sort -t 4 -T tmp/ -p - | bustools count -o genes -g ../homo_sapiens/transcripts_to_genes.txt -e matrix.ec -t transcripts.txt --genecounts -
```
and we have our gene count matrices.
