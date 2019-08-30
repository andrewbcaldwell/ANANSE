# XXX Network

### Prediction of key transcription factors in cell fate determination using enhancer networks

![](/pic/Fig2.jpg)
`(A), Illustration of all the data used to predict key TFs in cell conversion. Those data include the enhancer database from ATAC-seq, DNase-seq or p300 ChIP-seq, the motif score of all TFs and the gene expression data of each cell type from RNA-seq. (B), The predicted cell-type specific TF binding profiles from enhancer database and TF's motif score in each cell type. (C), The predicted cell-type specific GRN based on TF/Gene binding, TF/Gene expression and its' distance. (D), The GRN difference between two interested cell types. (E), The ranked influence score of all TFs calculated from GRN.`

## Quick start

### Dependencies
* Install Anaconda and activate bioconda channels.

```
# Install all dependencies
$ conda create -n regnetwork python=3 gimmemotifs genomepy networkx chest dask pytables

# Activate the environment
$ conda activate regnetwork

# install dependency via pip
$ pip install adjustText

# Uninstall stable versions of gimmemotifs and genomepy
$ conda uninstall gimmemotifs

# Upgrade genomepy and gimmemotifs to development version
$ pip install git+https://github.com/vanheeringen-lab/gimmemotifs.git@develop
```

* Run `gimme` to create a new GimmeMotifs config.

```
$ gimme
```

* Edit the file `~/.config/gimmemotifs/gimmemotifs.cfg`, and change the `ncpus` parameter.

* Download the genome of interest.

```
$ genomepy install Xenopus_tropicalis_v9.1 NCBI
```


### Built binding network

* Example:
```
$ python ../grns/binding.py -r data/krt_enhancer.bed \
                            -o results \
                            -a /home/qxu/.local/share/genomes/hg38/hg38_gffbed_piroteinCoding.bed \
                            -g hg38 \
                            -p ../data/gimme.vertebrate.v5.1.pfm
```
* Input

```
-r The enhancer BED file with enhancers (200bp) with RPKM (or equivalent) value in 4th column;
-o The folder to save results;
-a 12 columns BED file with gene annotation;
-g Genome;
-p Motifs file (optional; if provided there should also be a motif2factors.txt).
-f Filter promoters, input should be either 'True' or 'False'. (Default setting: True; if 'True', the function will filtered all promoter peaks (+-2k from TSS) in provided enhancer peaks.)
-d Keep temporary files, input should be either 'True' or 'False'. (Default setting: True)
```

### Built interaction network

* Example:
```
$ python ../grns/interaction.py -e data/KRT_rep1_TPM.txt data/KRT_rep2_TPM.txt \
                                -o results \
                                -a /home/qxu/.local/share/genomes/hg38/hg38_gffbed_piroteinCoding.bed \
                                -g hg38 \
                                -b results/binding.predicted.h5 \
                                -c /home/qxu/projects/regulatoryNetwork/history/cell_trans/human_gene_correlation/expressioncorrelation.txt \
                                -p ../data/gimme.vertebrate.v5.1.pfm
```
* Input
```
-e One or more gene expression file(s), 1st column should contain gene name, and a column should be named TPM; 
-o The folder to save results;
-a 12 columns BED file with gene annotation;
-g Genome;
-b The binding network from binding.py;
-c All gene correlation file;
-p motifs file (optional; if provided there should also be a motif2factors.txt).
```

### Built GRN

* Example:
```
$ python ../grns/network.py -f results/full_features.h5 -o results
```
* Input
```
-f The interaction network from interaction.py;
-o The folder to save results.
```

### Infer influence score

* Example:
```
$ python ../grns/influence.py -a results/full_network.txt \
                            -e data/FB_rep1_TPM.txt \
                            -d data/FB2KRT_degenes.csv \
                            -o results/FB2KRT.txt
```
* Input
```
-b The network in first cell (optional);
-a The network in second cell;
-e The gene expression in first cell (optional);
-d The differential expression table between two cells; 
-o The result file.

```

## Help

* The preferred way to get support is through the Github issues page

