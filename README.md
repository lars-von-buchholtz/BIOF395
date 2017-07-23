# BIOF395: Classification of taste abstracts and expression analysis

This analysis was done as a self-designed and executed project for the FAES course BIOF-395 on Text Mining and Natural Language Processing. Part of the analysis was done in Python, the other part in R. The project identifies PubMed abstracts dealing with taste bud cell physiology and extracts gene expression information from them.  The project starts with data collection of ~35,000 PubMed abstracts containing the term 'taste', parsing and storing the data in an SQL lite database, followed by data cleanup, exploration and feature extraction using the sklearn package. While this search method manages to detect all research abstracts dealing with taste bud physiology, it is highly contaminated by abstracts dealing with e.g. pharmacology, food technology, nutrition or learning paradigms. Therefore, we performed dimensionality reduction and unsupervised clustering in order to identify clusters dealing with taste bud physiology. Based on the unsupervised clustering results, additional keyword criteria were added to obtain a bona fide training and test set of abstracts to design a Random Forest machine learning model that allows us to more reliably detect the different classes of abstracts. Finally, all abstracts belonging to the 'taste bud physiology' class are analysed for the presence of gene names and a list of genes potentially expressed in taste bud cells is generated.

<hr>

This analysis contains the following files:

- powerpoint presentation `finalreport.pptx` which illustrates the analysis
- SQLite database `taste.db` contains tables:
  - `articles` (with Pubmed ID, title,abstract text,publication date)
  - `searches` (with Pubmed ID, and a logical column that indicates if the column name was found in this abstract)
  - `synonympairs` (with mouse gene information from MGI with synonym name and gene symbol pairs)
  
- iPython notebook `make taste db.ipynb`: searches the Pubmed database with the term `taste`, parses the results with an implementation of the Entrez object in BioPython, and populates the `articles` table in the `taste.db` SQLite database. It also parses the mouse gene/synonym information in the file `mouse.txt` which was downloaded from MGI (http://www.informatics.jax.org/downloads/reports/MRK_List2.rpt) and parses it into the table `synonympairs`.
- iPython notebook `analyse as whole.ipynb`: exploratory analysis of the corpus obtained from taste abstracts, this notebook is not necessary for any other parts of the analysis
- iPython notebook `analyse as docs.ipynb`: processes the abstract texts and extracts a TF-IDF matrix. It generated the following files:
  - `matrixfull.mtx`: the TF-IDF matrix as a sparse matrix in Matrix Market format
  - `allpmids.txt`: row names for the matrix, all taste Pubmed IDs
  - `allterms.txt`: column names for the matrix, all terms for the TF-IDF
- R markdown file `lsa1.Rmd`: performs latent semantic analysis (LSA) including Singular Value Decomposition (SVD), t-SNE plotting, hierarchical and k-means clustering. Contains functions for exploratory analysis and illustrates the distribution of important search terms such as genes expressed in taste bud cells. Finally, this code does a semi-supervised classification task by Random Forest machine learning models. It writes a list of Pubmed IDS 'tastebudpmids.txt' that contains all abstracts classified in class J: 'taste bud physiology'.
- an html version `lsa1.html` of the R markdown file which can also be viewed at http://rpubs.com/lvonbuchholtz/BIOF395
- iPython notebook `Expression analysis.ipynb`: pulls title and abstract texts from the Pubmed IDs in 'tastebudpmids.txt' and filters it for sentences that contain variations of the word 'express', then searches these sentences for the gene synonyms in the `taste.db` database. It consolidates the results into a table with gene name and count of abstracts it is mentioned in and saves it as `tastegenes.csv`

