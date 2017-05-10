# BIOF395: Classification of taste abstracts and expression analysis


This analysis contains the following files:

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
