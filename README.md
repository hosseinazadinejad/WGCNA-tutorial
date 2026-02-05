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

**scale free topology and soft threshold in WGCNA**

WGCNA starts from gene–gene correlation and turns it into connection strength.
The three main network types

1. unsigned

- Positive and negative correlations are treated equally
- Strong negative correlation = strong connection
- Useful when direction doesn’t matter
  
**Rarely ideal for biological interpretation**

2. signed

- Only positive correlations form strong connections
- Negative correlations → near zero adjacency
- Preserves biological directionality

3. signed hybrid

- Positive correlations → weighted connections
- Negative correlations → completely removed
- No artificial rescaling


 This is often preferred for:

- microarray data
- clean biological interpretation

Very popular in gene expression analysis.

Here, a signed hybrid network was constructed, in which only positive gene–gene correlations contribute to network connectivity, while negative correlations are excluded.

WGCNA wants the gene network to behave like a scale-free network.

That means:

- Most genes have few connections
- A few hub genes have many connections
  
To get this behavior, WGCNA raises correlations to a power (β), but which power should we choose?
That’s exactly what soft-threshold indices help us decide.

It tries many β values (1:20) and for each β it asks:

“If we build a network using this β, how close is it to a scale-free network?”

**Structure of sft$fitIndices**

Each row = one β value
Each column = one diagnostic metric

Column 1 — Soft-thresholding power (β)
it’s the candidate power

Column 2 — Scale-free topology model fit (R²)

This answers:

“How well does the network degree distribution follow a power law?”


-	Values range from 0 to 1
-	Higher = better scale-free behavior

Column 3 — Slope of log(k) vs log(p(k))

For a true scale-free network:

-	The slope must be negative
-	Positive slope = biologically meaningless network

Column 5 — Mean connectivity

On average, how many connections does each gene have?”

As β increases:

-	Correlations are raised to higher powers
-	Weak correlations shrink
-	Network becomes sparser

So:
-	Very high β → disconnected network
-	Very low β → too dense, noisy network


WGCNA fits a linear model, but not on the original data.

it fit a linear model on a log–log transformed distribution:

log(p(k)) = a + b *log(k)

-	R² → how well points follow a straight line
-	Slope (b) → the direction of that line


The slope must be negative because in a scale-free network:

Nodes with high connectivity are rare,
nodes with low connectivity are common

If slope were positive, it would mean:

Highly connected genes are more frequent than low-connected ones
→ biologically and mathematically wrong.

Why MUST the slope be negative?

Connectivity (k)	Number of genes
Low                	Many
High            	Very few

So as k increases, p(k) decreases.

On a log–log plot:

x increases → y decreases → downward sloping line

**Topological overlap matrix similarity and dissimilarity**


Topological overlap matrix(TOM) measures how similar two genes are based on their shared network neighborhood, not just their direct correlation.
Key ideas
two genes are considered strongly related if:
they are directly connected, and 
they are connected to many same genes.

From adjacency to TOM:
correlation matrix -> similarity between genes
adjacency matrix -> correlation raised to soft-threshold power
TOM -> incorporates shared neighbors

dissTOM is a distance matrix for gene clustering.
