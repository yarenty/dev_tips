- This paper introduces DataMan, a data manager for pre-training large language models (LLMs), addressing the need for comprehensive data selection guidelines .
- DataMan annotates a 447B token corpus with 14 quality criteria and domain type, derived from LLMs' self-identified performance benefits, and uses this to train a 1.3B-parameter language model .
- Experiments validate DataMan's approach, demonstrating improvements in in-context learning (ICL), perplexity, and instruction-following, surpassing state-of-the-art baselines and highlighting the importance of quality ranking .
**Introduction** 
- Data scaling laws have made pre-training data selection increasingly important for LLMs, but current methods lack comprehensive guidelines .
- This paper addresses the challenge of selecting ideal pre-training data by providing guidelines including quality criteria, application domains, and a Data Manager (DataMan) .
- The approach involves "reverse thinking," prompting LLMs to identify criteria that benefit performance, deriving quality criteria from text perplexity anomalies, and supporting domain mixing .
**Annotating Text by DataMan** 
- DataMan comprehensively annotates 14 quality criteria and domain types to enable data selection and mixing .
- A super LLM is used as the annotation function, recording results for each criterion and domain, and a fine-tuning dataset is created for DataMan .
- The quality criteria and domain types are defined by prompting the LLM to self-identify performance-benefiting criteria, using documents with top and bottom perplexity (PPL) scores .
**Why Use Pointwise Rating?** 
- Pointwise rating was chosen over pairwise rating due to the limitations of pairwise rating when quality differences are marginal .
- Pointwise rating error is bounded, as the model minimizes loss through training, which also reduces rating error .
- Rankings from pointwise ratings highlight top-quality documents and accurately categorize domains, aiding domain-specific continue pre-training .
**Training the DataMan Model** 
- Documents from the SlimPajama corpus were collected to train DataMan, with the Super LLM providing 14 quality ratings and domain types for each document, creating a fine-tuning dataset .
- The DataMan model was fine-tuned using Qwen2-1.5B with text generation loss, achieving near 80% average test accuracy across all criteria .
- The DataMan model achieves an Overall Score test accuracy of 81.3% and a domain recognition test accuracy of 86% .
**Managing Data by DataMan** 
- The DataMan model is applied to select a high-quality, diverse document subset from the pre-training corpus, annotating each document with 14 quality ratings and domain types .
- Top-k sampling without replacement is performed for each quality criterion across source and domain distribution, maximizing sample representativeness while ensuring diversity .
- Overall Score ratings are used to steer language modeling toward reward-weighted regression, expanding maximum likelihood estimation loss with a data reward mechanism .
**Experiments** 
- Experiments empirically validate the DataMan method by training the language model from scratch using the 447B token DataPajama for pre-training .
- Data selection methods include Uniform sampling, Data Selection with Importance Resampling (DSIR), Perplexity Filtering, Qurating sampling and DataMan sampling .
- Evaluation metrics include perplexity, in-context learning (ICL) performance, and instruction-following capabilities using AlpacaFarm .
**Results** 
- Traditional methods like DSIR and perplexity filtering perform worse than random uniform sampling, indicating their ineffectiveness for data selection .
- Clear quality criteria such as Qurating's improve performance, while the criteria mix fails due to a lack of complementarity .
- DataMan's criteria surpass the SOTA baseline, with Overall Score performing best, highlighting the necessity of quality ranking and validating the feasibility of combining criteria .
**DataMan’s instruction-following abilities also well** 
- Sample-with-DataMan consistently surpasses the SOTA Baseline with the Overall Score reaching an impressive win rate at 78.5% .
- DataMan's domain recognition helps achieve superior ICL performance in specific domains, validating DataMan's capability for domain mixing .
**Analysis of Quality Ratings** 
- Quality ratings are primarily concentrated at 4 and 5, indicating generally high sample quality, but Knowledge Novelty and Creativity have lower average scores .
- Most quality criteria do not show a significant correlation with perplexity, except for Structural standardization, Professionalism, and Creativity, indicating that DataMan's method is independent of traditional perplexity metrics .
**Data Inspection** 
- Original documents from each source under DataMan’s quality criteria and ratings were examined, revealing significant quality differences despite representing small snippets .
**Conclusion** 
- DataMan, a comprehensive data manager, facilitates pre-training data selection and mixing through quality rating and domain recognition .
- Models trained with DataMan demonstrate significant improvements in language modeling, task generalization, and instruction following .
- Extensive experiments provide valuable insights for studying the relationship between data and large language models .
**Limitations** 
- DataMan’s reliance on LLMs may inherit biases, and its inference accuracy requires improvement via a large-scale collection of diverse documents .
- Using SlimPajama alone limits result reliability, and the model size is restricted by data and training resources .
- Considerable costs could hinder further research, but DataMan remains a powerful tool for data selection and mixing .
