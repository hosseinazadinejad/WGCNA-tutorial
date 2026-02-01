Hello. Welcome to my first GitHub repository. In this tutorial, we will practice WGCNA analysis together. Here, I use R version 4.5.1 The data we will analyze together is available at the link below:

https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE24279

It's a miRNA microarray dataset  consist of 185 samples of pancreatic ductal adenocarcinoma, chronic pancreatitis disease, and healthy individuals provided by Keller et al.(2012)

We published a research paper based on this dataset on 2025 which can be accessed through this link:

https://www.sciencedirect.com/science/article/abs/pii/S2773044125000361

please take a look at it in your free time.

Now, let's take a look at WGCNA :

WGCNA (Weighted Gene co-expression network analysis) is a systems biology method to identify groups of genes(modules) that show similar expression patterns across samples.instead of analyzing genes one by one, WGCNA focuses on gene-gene relationships, allowing researchers uncover biologically meaningful networks and relate them to clinical or experimental traits.

**Core ideas behind WGCNA**
- Genes with similar expression profiles are likely to be functionally related 
- Pairwise gene correlations are used to construct a weighted network
- Highly connected genes cluster into modules
- Modules can be correlated with external traits(like disease status,clinical variables)
- Each module is summarized by a module eigengene, representing it's overall expression pattern
  **Typical WGCNA workflow**
  1. Preprocess and normalize expression data
  2. construct a weighted gene co-expression network
  3. Detect gene modules using hierarchical clustering
  4. Merge similar modules
  5. Relate modules to external traits

**Common applications**
- Biomarker discovery
- Disease subtype characterization
- Systems-level interpretation of omics data
