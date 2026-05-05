## Linfa

https://github.com/rust-ml/linfa


`linfa` aims to provide a comprehensive toolkit to build Machine Learning applications with Rust.

Kin in spirit to Python's `scikit-learn`, it focuses on common preprocessing tasks and classical ML algorithms for your everyday ML tasks.

**[Website](https://rust-ml.github.io/linfa/) | [Community chat](https://rust-ml.zulipchat.com/)**

[Current state](https://github.com/rust-ml/linfa#current-state)

Where does `linfa` stand right now? [Are we learning yet?](http://www.arewelearningyet.com/)

`linfa` currently provides sub-packages with the following algorithms:


[clustering](https://github.com/rust-ml/linfa/blob/master/algorithms/linfa-clustering)
Clustering of unlabeled data; contains K-Means, Gaussian-Mixture-Model, DBSCAN and OPTICS

[kernel](https://github.com/rust-ml/linfa/blob/master/algorithms/linfa-kernel)
Maps feature vector into higher-dimensional space

[linear](https://github.com/rust-ml/linfa/blob/master/algorithms/linfa-linear)
Contains Ordinary Least Squares (OLS), Generalized Linear Models (GLM)

[elasticnet](https://github.com/rust-ml/linfa/blob/master/algorithms/linfa-elasticnet)
Linear regression with elastic net constraints

[logistic](https://github.com/rust-ml/linfa/blob/master/algorithms/linfa-logistic)
Builds two-class logistic regression models

[reduction](https://github.com/rust-ml/linfa/blob/master/algorithms/linfa-reduction)
Diffusion mapping and Principal Component Analysis (PCA)

[trees](https://github.com/rust-ml/linfa/blob/master/algorithms/linfa-trees)
Linear decision trees

[svm](https://github.com/rust-ml/linfa/blob/master/algorithms/linfa-svm)
Classification or regression analysis of labeled datasets

[hierarchical](https://github.com/rust-ml/linfa/blob/master/algorithms/linfa-hierarchical)
Agglomerative hierarchical clustering
Cluster and build hierarchy of clusters

[bayes](https://github.com/rust-ml/linfa/blob/master/algorithms/linfa-bayes)
Naive Bayes
Contains Gaussian Naive Bayes

[ica](https://github.com/rust-ml/linfa/blob/master/algorithms/linfa-ica)
Independent component analysis
Contains FastICA implementation

[pls](https://github.com/rust-ml/linfa/blob/master/algorithms/linfa-pls)
Partial Least Squares
Contains PLS estimators for dimensionality reduction and regression

[tsne](https://github.com/rust-ml/linfa/blob/master/algorithms/linfa-tsne)
Dimensionality reduction
Contains exact solution and Barnes-Hut approximation t-SNE

[preprocessing](https://github.com/rust-ml/linfa/blob/master/algorithms/linfa-preprocessing)
Normalization & Vectorization
Contains data normalization/whitening and count vectorization/tf-idf

[nn](https://github.com/rust-ml/linfa/blob/master/algorithms/linfa-nn)
Nearest Neighbours & Distances
Spatial index structures and distance functions


If this strikes a chord with you, please take a look at the [roadmap](https://github.com/rust-ml/linfa/issues/7) and get involved!





linfa currently provides sub-packages with the following algorithms:

| Name	| Purpose	                                | Status	| Category	| Notes |
|----|-----------------------------------------|---|----|----|
| clustering | Data clustering                         |	Tested / Benchmarked	| Unsupervised learning	| Clustering of unlabeled data; contains K-Means, Gaussian-Mixture-Model, DBSCAN and OPTICS |
| kernel	| Kernel methods for data transformation	 | Tested	|Pre-processing	| Maps feature vector into higher-dimensional space
|linear	| Linear regression	                      |Tested	|Partial fit	|Contains Ordinary Least Squares (OLS), Generalized Linear Models (GLM)
|elasticnet	| Elastic Net|	Tested	|Supervised learning	|Linear regression with elastic net constraints
|logistic	|Logistic regression	|Tested	|Partial fit	|Builds two-class logistic regression models
| reduction	| Dimensionality reduction	|Tested	|Pre-processing	|Diffusion mapping and Principal Component Analysis (PCA)
| trees	|Decision trees	|Tested / Benchmarked	|Supervised learning	|Linear decision trees
| svm	|Support Vector Machines	|Tested	|Supervised learning	|Classification or regression analysis of labeled datasets
| hierarchical	|Agglomerative hierarchical clustering	|Tested	|Unsupervised learning	|Cluster and build hierarchy of clusters
| bayes	|Naive Bayes	|Tested	|Supervised learning	|Contains Gaussian Naive Bayes
| ica	| Independent component analysis	| Tested	|Unsupervised learning	|Contains FastICA implementation
| pls	|Partial Least Squares	|Tested	|Supervised learning	|Contains PLS estimators for dimensionality reduction and regression
| tsne	|Dimensionality reduction	|Tested	|Unsupervised learning|	Contains exact solution and Barnes-Hut approximation t-SNE
| preprocessing	|Normalization & Vectorization	|Tested / Benchmarked	|Pre-processing	|Contains data normalization/whitening and count vectorization/tf-idf
| nn	|Nearest Neighbours & Distances	|Tested / Benchmarked	|Pre-processing	|Spatial index structures and distance functions
| ftrl	|Follow The Reguralized Leader - proximal	|Tested / Benchmarked	|Partial fit	|Contains L1 and L2 regularization. Possible incremental update
