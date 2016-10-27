# CIBERSORT
- Using CIBERSORT to estimate cell type composition.

Using our adipose tissue signature matrix to estimate cell type composition of adipose tissue.

The adipose tissue signature matrix was constructed from independent purified cell type RNA-seq data from macrophages, adipocytes, endothelial cells and CD4+ t-cells – Some of the most dominant cell types present in adipose tissue.

The signature matrix was constructed using Counts Per Million (CPM) gene expression levels that have not been filtered for expression. Each deconvolution happens on a sample by sample basis, therefore it is not necessary to filter genes for example that are expressed in at least 10% of samples. Additionally, rare cell types will be represented by lowly expressed genes relative to other genes expressed in more common cell types (linearity assumption of all cell estimation methods) so filtering lowly expressed genes could bias cell type estimates upwards.

It is therefore important that you take the raw gene count matrix (obtained from featureCount for example) and transform them into CPM using the following R code:

expr[,2:ncol(expr)]=apply(expr,2,function(x) (x/sum(x)) *1e6)

where: 

expr is an NxM matrix with the first column being gene (ensembl ID) and all other columns being adipose RNA-seq samples (raw gene counts).

Cell type deconvolution will be sensitive to the method/normalization used and therefore if we observe vast differences in cell estimates I may reproduce the signature matrix using a pipeline closer to your RNA-seq study and ask you to re-run.

 
CIBERSORT to estimate cell fractions:
 
R –-no-restore
library(Rserve)
Rserve()
q()

java -jar CIBERSORT.jar -M DeCODE.adiposeSamples.txt -B Adipose.sigMatrix.txt –n 1000 >> Decode.Adipose.CellEsts.txt


Where:	

 DeCODE.adiposeSamples.txt - NxM matrix with the first column being gene (ensembl ID) and all other columns being samples.

Adipose.sigMatrix.txt – signature matrix containing median CPM expression levels of genes used to estimate relative cell type proportions.

This will output the cell types, with some additional information in the header (This can be discarded).

Please send these cell estimates to us so we can see if they are similar to our predictions in our biopsy data. If that is the case, we ask you to continue this pipeline.


 
Correlation of cell type estimates with obesity metrics:

Please test the following correlations with BMI both with and without removing outlier cell estimates (+- 3 S.D from the mean) for each cell type.

All macrophages (M1+M2 macrophages) 
Adipocytes vs BMI
You can also test the other cell types present in the signature matrix if you observe they are present in your samples (not mostly zero).

If you have any other obesity metric, such as central adiposity (We have used DEXA scans for this) we would also be grateful to see a correlation with cell type proportion.

