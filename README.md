# richR <a href="https://travis-ci.org/guokai8/richR"><img src="https://travis-ci.org/guokai8/richR.svg" alt="Build status"></a>  [![Project Status:](http://www.repostatus.org/badges/latest/active.svg)](http://www.repostatus.org/#active)  [![](https://img.shields.io/badge/devel%20version-0.0.18-green.svg)](https://github.com/guokai8/richR) 
## Description
_richR_ provide functions _richGO_, _richKEGG_,and _enrich_ to do functional enrichment analysis. 
## Installation
```
library(devtools)
install_github("guokai8/richR")
``` 
## Quick tour
```{r}
set.seed(123)   
library(richR)   
# To check the available species !!!
showData()   
# Make the GO and KEGG Pathway data for your analysis
# find suitable species name by using showensemble()    
hsa_go <- buildAnnot(species="human",keytype="SYMBOL",anntype = "GO")
hsa_ko <- buildAnnot(species = "human",keytype="SYMBOL", anntype = "KEGG")
```   
### Support KEGG Module and MSIGDB anntation
```
hsamgi <- buildMSIGDB(species="human",keytype="SYMBOL",anntype="GO")
hsakom <- buildAnnot(species = "human",keytype="SYMBOL", anntype = "KEGGM")
```
____   

### You can make annotation data for all species from _bioAnno_ package   
* If had _bioAnno_ installed, you can build annotation package with it  
```{r}
# library(bioAnno)
# fromKEGG(species="ath")
# athgo<-buildOwn(dbname="org.ath.eg.db",anntype="GO")  
# athko<-buildOwn(dbname="org.ath.eg.db",anntype="KEGG") 
# Please go over the bioAnno package webpage ("https://github.com/guokai8/bioAnno") to learn more
```   
----

### Simple example for enrichment analysis

```{r,fig.height=6,fig.width=6,fig.align="center",dpi=100}
gene <- sample(unique(hsa_go$GeneID),1000)
res <- richGO(gene,godata = hsa_go,ontology ="BP")
head(res)
ggbar(res,top = 20,usePadj = F)
```       

```{r,fig.height=6,fig.width=6,fig.align="center"}
resko<-richKEGG(gene,hsa_ko,pvalue=0.05)
head(resko)
ggdot(resko,top=10,usePadj = F)
```   
### Support DAVID analysis (Online)
```
gene <- sample(unique(hsako$GeneID),1000)
res <- richDAVID(gene,keytype="ENTREZID",species="human")
```
#### You can also get network graphic for any type of enrichment analysis result and also combine different enrichment result
```{r,fig.height=6,fig.width=6,fig.align="center",dpi=100}
ggnetplot(res,top=20)
ggnetwork(res,top=20,weightcut = 0.01)

```   
### Directly support dplyr filter, select, mutate,group_by ... functions
```
filter(resko,Padj<0.05)%>%head()
select(resko,Term)
```
### Generate figures with mutiple enrichment results for groups
```
gene1 <- sample(unique(hsako$GeneID),1000)
gene2 <- sample(unique(hsako$GeneID),1000)
resko1 <- richKEGG(gene1,kodata = hsako)
resko2 <- richKEGG(gene2,kodata = hsako)
res <- compareResult(list(S1=resko1,S2=resko2))
comparedot(res)
```
### Generate figures with different enrichment results
```
ggnetmap(list(res,resko),top=50,visNet=TRUE,smooth=FALSE)
```
### Contact information
For any questions please contact guokai8@gmail.com
