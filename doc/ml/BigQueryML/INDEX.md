
# What is BigQuery ML?

BigQuery ML lets you create and execute machine learning models in BigQuery using GoogleSQL queries. BigQuery ML democratizes machine learning by letting SQL practitioners build models using existing SQL tools and skills. BigQuery ML increases development speed by eliminating the need to move data.

BigQuery ML functionality is available by using:

- The Google Cloud console
- The bq command-line tool
- The BigQuery REST API
- An external tool such as a Jupyter notebook or business intelligence platform

Machine learning on large datasets requires extensive programming and knowledge of ML frameworks. These requirements restrict solution development to a very small set of people within each company, and they exclude data analysts who understand the data but have limited machine learning knowledge and programming expertise.

BigQuery ML empowers data analysts to use machine learning through existing SQL tools and skills. Analysts can use BigQuery ML to build and evaluate ML models in BigQuery. Analysts don't need to export small amounts of data to spreadsheets or other applications or wait for limited resources from a data science team.

## Supported models in BigQuery ML
A model in BigQuery ML represents what an ML system has learned from the training data.

BigQuery ML supports the following types of models:

- Linear regression for forecasting; for example, the sales of an item on a given day. Labels are real-valued (they cannot be +/- infinity or NaN).
- Binary logistic regression for classification; for example, determining whether a customer will make a purchase. Labels must only have two possible values.
- Multiclass logistic regression for classification. These models can be used to predict multiple possible values such as whether an input is "low-value," "medium-value," or "high-value." Labels can have up to 50 unique values. In BigQuery ML, multiclass logistic regression training uses a multinomial classifier with a cross-entropy loss function.
- K-means clustering for data segmentation; for example, identifying customer segments. K-means is an unsupervised learning technique, so model training does not require labels nor split data for training or evaluation.
- Matrix Factorization for creating product recommendation systems. You can create product recommendations using historical customer behavior, transactions, and product ratings and then use those recommendations for personalized customer experiences.
- Time series for performing time-series forecasts. You can use this feature to create millions of time series models and use them for forecasting. The model automatically handles anomalies, seasonality, and holidays.
- Boosted Tree for creating XGBoost based classification and regression models.
- Deep Neural Network (DNN) for creating TensorFlow-based Deep Neural Networks for classification and regression models.
- Vertex AI AutoML Tables to perform machine learning with tabular data using simple processes and interfaces.
- TensorFlow model importing. This feature lets you create BigQuery ML models from previously trained TensorFlow models, then perform prediction in BigQuery ML.
- Autoencoder for creating Tensorflow-based BigQuery ML models with the support of sparse data representations. The models can be used in BigQuery ML for tasks such as unsupervised anomaly detection and non-linear dimensionality reduction.



![ML cheetsheet](../../assets/ml-model-cheatsheet.svg)







| Functionality	                                                   | Snowflake                                                                              |	BigQuery
|------------------------------------------------------------------|----------------------------------------------------------------------------------------|----
| Separation of storage and compute	                               | Yes	                                                                                   | Yes
| Supported cloud infrastructure	                                  | AWS, Azure, Google Cloud	                                                              | Google Cloud only
| Isolated tenancy – option for dedicated resources	               | – Multi-tenant pooled resources <br/>– Isolated tenancy available via “VPS” tier	      | – Multi-tenant pooled resources <br/>– “VPC Service Controls” can be used to limit connectivity to your VPC
| Control vs abstraction of compute	                               | – Configurable cluster size (1-128 nodes, 256 and 512 in preview)                      | – No control over compute types	Serverless, no control over compute (BigQuery determines how many “slots” each query needs)
| Elasticity – Scaling for larger data volumes and faster queries	 | Cluster resize; No choice of node size	                                                | Fully abstracted – BigQuery decides how many “slots” to allocate for each query. No way to “force” more compute for faster queries.
| Elasticity – Scaling for higher concurrency	                     | 8 concurrent queries per warehouse by default, <br/> Autoscaling up to 10 warehouses.	 | Limited to 100 concurrent users by default
| Indexes	                                                         | “Search optimization service” indexes fields for accelerated point lookup queries, at additional cost; Materialized views | None
| Compute tuning	                                                  | Compute is chosen from a fixed T-shirt size list which abstracts underlying # of nodes, CPU, RAM and SSD	| No choice over how many slots are allocated for queries
| Storage format	                                                  | Columnar micro-partitioned & compressed storage	| Columnar & compressed storage (code named “Capacitor”)
| Table-level partition & pruning techniques	                      | Data is automatically divided into micro-partitions. Pruning at micro-partition level. Table clustering (cluster keys)	| User-defined Table-level partitions. Pruning at partition level.
| Result cache                                                     |	Yes	| Yes
| Warm cache (SSD)	                                                | Yes, at micro–partition level granularity.	| No
| Support for Semi-structured data & JSON functions within SQL	    | Yes	| Yes
| Low-latency dashboards	                                          | Dozens of second load times at 100s of GB scale	| Dozens of second load times at 100s of GB scale
| Enterprise BI	                                                   | Mature and broad Enterprise DW featureset	| Mature and broad Enterprise DW featureset
| Data Apps <br/> (Customer-facing, low latency, high concurrency) |– Dozens of second load times at 100s of GB scale. <br/> – Scale-out to more clusters required starting from dozens of concurrent queries. | – Dozens of second load times at 100s of GB scale. <br/>– Supports up to 100 concurrent queries.
| Ad hoc	                                                          | Decoupled storage/compute architecture allows to spin up ad-hoc resources	| Serverless architecture enables ad-hoc without worrying about compute



When testing a TPC-DS workload, research firm GIGAOM found that Snowflake consistently performed better than BigQuery





[LINKS](links.md)