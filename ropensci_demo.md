## rOpenSci Demo

[rOpenSci](http://ropensci.org/)

### Getting data from the literature - PLOS

This example demonstrates how you can easily get literature data from Public Library of Science from R. 

#### Install rplos


```r
install.packages(c("rplos", "tm"))
```


#### Load rplos


```r
library(rplos)
```


#### Search for mentions of Fisher in the author field, returning title and author fields, searching in full papers (not including figure captions, etc.), returning only 25 results.


```r
out <- searchplos(terms = "author:\"Fisher\"", fields = "title,author", toquery = "doc_type:full", 
    limit = 25)
head(out)  # first six rows
```

```
##                                               author
## 1                      Qianru Zhang; Kimberly Fisher
## 2                                   Matthew C Fisher
## 3                  Masashi Yoshimura; Brian L Fisher
## 4                    Jill A Fisher; Corey A Kalbaugh
## 5 Cindy Vallières; Nicholas Fisher; Brigitte Meunier
## 6                    Adam C Fisher; Matthew P DeLisa
##                                                                                                                                     title
## 1                           Tight Junction-Related Barrier Contributes to the Electrophysiological Asymmetry across Vocal Fold Epithelium
## 2                                                                                       Silent Springs: Why Are All the Frogs “Croaking”?
## 3 A Revision of Male Ants of the Malagasy Amblyoponinae (Hymenoptera: Formicidae) with Resurrections of the Genera Stigmatomma and Xymmer
## 4                                       United States Private-Sector Physicians and Pharmaceutical Contract Research: A Qualitative Study
## 5                                                     Reconstructing the Qo Site of Plasmodium falciparum bc1 Complex in the Yeast Enzyme
## 6                                  Laboratory Evolution of Fast-Folding Green Fluorescent Protein Using Secretory Pathway Quality Control
```


#### Quickly visualize variation in frequency of word usage in PLOS journals


```r
library(ggplot2)
plosword(list("monkey", "Helianthus", "sunflower", "protein", "whale"), vis = TRUE)$plot + 
    theme_grey(base_size = 18)
```

![plot of chunk plosword](figure/plosword.png) 


#### Get abstracts of 500 papers, and use the tm package for text mining. 

Get 500 abstracts from PLOS One only. The `*:*` is special syntax to denote *give back everything*


```r
out <- searchplos(terms = "*:*", fields = "abstract", toquery = list("cross_published_journal_key:PLoSONE", 
    "doc_type:full"), limit = 500)
out$abstract[1:3]  # take a peek
```

```
## [1] "The membrane associated guanylate kinase (MAGUK) family member, human Discs Large 1 (hDlg1) uses a PDZ domain array to interact with the polarity determinant, the Adenomatous Polyposis Coli (APC) microtubule plus end binding protein. The hDLG1-APC complex mediates a dynamic attachment between microtubule plus ends and polarized cortical determinants in epithelial cells, stem cells, and neuronal synapses. Using its multi-domain architecture, hDlg1 both scaffolds and regulates the polarity factors it engages. Molecular details underlying the hDlg1-APC interaction and insight into how the hDlg1 PDZ array may cluster and regulate its binding factors remain to be determined. Here, I present the crystal structure of the hDlg1 PDZ2-APC complex and the molecular determinants that mediate APC binding. The hDlg1 PDZ2-APC complex also provides insight into potential modes of ligand-dependent PDZ domain clustering that may parallel Dlg scaffold regulatory mechanisms. The hDlg1 PDZ2-APC complex presented here represents a core biological complex that bridges polarized cortical determinants with the dynamic microtubule cytoskeleton."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     
## [2] "Background: HIV replication in mononuclear phagocytes is a multi-step process regulated by viral and cellular proteins with the peculiar feature of virion budding and accumulation in intra-cytoplasmic vesicles. Interaction of urokinase-type plasminogen activator (uPA) with its cell surface receptor (uPAR) has been shown to favor virion accumulation in such sub-cellular compartment in primary monocyte-derived macrophages and chronically infected promonocytic U1 cells differentiated into macrophage-like cells by stimulation with phorbol myristate acetate (PMA). By adopting this latter model system, we have here investigated which intracellular signaling pathways were triggered by uPA/uPAR interaction leading the redirection of virion accumulation in intra-cytoplasmic vesicles. Results: uPA induced activation of RhoA, PKCδ and PKCε in PMA-differentiated U1 cells. In the same conditions, RhoA, PKCδ and PKCε modulated uPA-induced cell adhesion and polarization, whereas only RhoA and PKCε were also responsible for the redirection of virions in intracellular vesicles. Distribution of G and F actin revealed that uPA reorganized the cytoskeleton in both adherent and polarized cells. The role of G and F actin isoforms was unveiled by the use of cytochalasin D, a cell-permeable fungal toxin that prevents F actin polymerization. Receptor-independent cytoskeleton remodeling by Cytochalasin D resulted in cell adhesion, polarization and intracellular accumulation of HIV virions similar to the effects gained with uPA. Conclusions: These findings illustrate the potential contribution of the uPA/uPAR system in the generation and/or maintenance of intra-cytoplasmic vesicles that actively accumulate virions, thus sustaining the presence of HIV reservoirs of macrophage origin. In addition, our observations also provide evidences that pathways controlling cytoskeleton remodeling and activation of PKCε bear relevance for the design of new antiviral strategies aimed at interfering with the partitioning of virion budding between intra-cytoplasmic vesicles and plasma membrane in infected human macrophages."
## [3] "Background: Cells of the innate immune system including monocytes and macrophages are the first line of defence against infections and are critical regulators of the inflammatory response. These cells express toll-like receptors (TLRs), innate immune receptors which govern tailored inflammatory gene expression patterns. Monocytes, which produce pro-inflammatory mediators, are readily recruited to the central nervous system (CNS) in neurodegenerative diseases. Methods: This study explored the expression of receptors (CD11b, TLR2 and TLR4) on circulating monocyte-derived macrophages (MDMs) and peripheral blood mononuclear cells (PBMCs) isolated from healthy elderly adults who we classified as either IQ memory-consistent (high-performing, HP) or IQ memory-discrepant (low-performing, LP). Results: The expression of CD11b, TLR4 and TLR2 was increased in MDMs from the LP group when compared to HP cohort. MDMs from both groups responded robustly to treatment with the TLR4 activator, lipopolysaccharide (LPS), in terms of cytokine production. Significantly, MDMs from the LP group displayed hypersensitivity to LPS exposure. Interpretation: Overall these findings define differential receptor expression and cytokine profiles that occur in MDMs derived from a cohort of IQ memory-discrepant individuals. These changes are indicative of inflammation and may be involved in the prodromal processes leading to the development of neurodegenerative disease."
```


Load the tm package, and create a document library


```r
library(tm)
(corpus <- Corpus(VectorSource(out$abstract)))
```

```
## A corpus with 500 text documents
```


Create a term-document matrix from the corpus, and inspect it. 


```r
tdm <- DocumentTermMatrix(corpus, control = list(removePunctuation = TRUE, stopwords = TRUE, 
    removeNumbers = TRUE))
inspect(tdm[1:5, 1:5])
```

```
## A document-term matrix (5 documents, 5 terms)
## 
## Non-/sparse entries: 0/25
## Sparsity           : 100%
## Maximal term length: 9 
## Weighting          : term frequency (tf)
## 
##     Terms
## Docs aaa aag aanhui abcc abdominal
##    1   0   0      0    0         0
##    2   0   0      0    0         0
##    3   0   0      0    0         0
##    4   0   0      0    0         0
##    5   0   0      0    0         0
```


Various operations on document term matrices


```r
# find terms that occur at least five times across documents
findFreqTerms(tdm, 250)
```

```
## [1] "cell"       "cells"      "expression" "results"
```

```r

# find associations (terms which correlate) with at least 0.3 correlation
# for the term 'result'
findAssocs(tdm, "cells", 0.3)
```

```
##     stem    adapt     cell     cdcd effector   sorted 
##     0.46     0.38     0.36     0.34     0.33     0.31
```

