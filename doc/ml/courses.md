# top 5 2022 couses

## 1. Virgilio
   
   Virgilio is dubbed as the new mentor for data science e-learning and aims for everyone to have a chance to learn data science. Virgilio also tries to create a path for the learner to have a structured study to avoid overwhelmed confusion during data science study.
   The open-source project was structured with three layers to accommodate everyone's needs. The layer was called Paradiso for high-level guide, Purgatorio for entry-level, and Inferno for advanced-level.


The learning starts from the Paradiso level where the content is all about the theory and why you should learn data science (no coding at all), for example:
What are machine learning and the differences between AI

Do you need machine learning?

The use cases

Teaching strategy. and many more. Paradiso is the perfect start for people who start their data science journey and understand the field better.

From the Paradiso level, we move to the Purgatorio level. This level would cover the basics of data scientists from the fundamental to the hands-on activity such as:
* Mathematics and Statistic Fundamental
* Programming Python Fundamental
* Problem definition
* Data Exploration
* Machine Learning Training
And many more. 
You would learn everything you need to start in the data science field. Don't worry about the structure because Purgatorio also starts from fundamental to more basic usage.

Finally, the advanced level is the Inferno level, where this part is intended for the advanced user. This section would teach you a specific application for data science:
* Time Series
* Computer Vision
* Natural Language Processing

Additionally, the Inferno level provides learning material for specific data science tools and libraries. The list would grow in time, so keep checking out the project.

Virgilio's project was developed by various core teams and contributors who were experts in the field. If you are interested, try to chat with the team here, especially contributing to their cause.


## 2. MLCourse
   
MLCourse is an open-source project initiative led by Yury Kashnitsky from OpenDataScience to learn more about machine learning where learners could have the perfect balance of theoretical and practical skills. Just like the name implies, the MLCourse is a compilation of courses project which we could follow with self-paced.

However, the courses are slightly intended for people who have basic data science skills such as Python and Math. But, it doesn't mean beginners could not try out the courses — their guide is very insightful, after all.
MLCourse contains ten topics for people to learn which intended to be followed in a structure; they are:
   
   * EDA with Pandas
   * Visual Data Analysis
   * Classification, Decision Tree, and K-NN
   * Ordinary Least Squares and Linear Model
   * Bagging
   * Feature Engineering and Feature Selection
   * Unsupervised Analysis
   * Optimization
   * Time Series
   * Gradient Boosting
   
   Every topic contains an easy-to-follow guide, example notebook, assignment, and video course.
   
The downside of MLCourse is that the development stopped in 2019 for the English language (the Russian Language is resurrected in 2022). However, the material is still relevant for our current data science field — especially for beginners.


## 3. ProjectLearn
   
ProjectLearn is an open-source project that provides a curated list of tutorial projects. The creator of ProjectLearn aims for more hands-on application learning rather than theoretical, so you could expect to learn a specific skill set rather than a general one.
   ProjectLearn is not specific to data science because you could also learn web, mobile, and game development. However, there is a special section for Machine Learning and AI, which is what we want.

ML & AI Section (Image by Author)
Most of the project is an external link to another article or videos, but these projects are already curated and would be perfect for you who want to explore what you could do with machine learning.

## 4. Deepkapha

   Deepkapha is an open-source project that curated many Artificial Intelligence and Deep Learning tutorial for people to learn from. When I look at Deepkapha, I feel the project is intended for people who have a basic knowledge of data science and programming, so it's better to explore Deepkapha when you are ready.
   Many Deepkapha focuses on Deep Learning and various framework tutorial, which is perfect if you want to learn the Deep Learning concept and the differences between frameworks. However, you could still explore much learning material, although they are not that specific.
   One other section that I consider special is a Deep Learning blog collection which consists of various writers and blogs for deep learning. The collection is so complete that it could take days to explore all the blogs.

## 5. Best-of ML Python

   Best-of ML Python is part of the Best-of open-source project that curated various open-source packages and tools daily. The Best-of ML Python is specific for curated open-source machine learning packages for Python programming language.
   The Best-of series did not specifically give a tutorial on how-to or learning basic concepts. Still, instead, they categorized all the awesome Python packages for us to try out.

Best-Of ML Python Project list (GIF by Author)
As you can see from the GIF above, the list is abundant and segmented depending on what you need. Almost everything you need to learn a specific subject via Python package is present so try to explore as much as possible.



Conclusions
Learning data science is not easy and could be confusing when we don't know where to start. That is why in this article, I want to present my top open-source project to learn data science. The projects are:
Virgilio
MLCourse
ProjectLearn
Deepkapha
Best-of ML Python



## Neural Networks: Zero to Hero A course by Andrej Karpathy

https://karpathy.ai/zero-to-hero.html


A course by Andrej Karpathy on building neural networks, from scratch, in code.
We start with the basics of backpropagation and build up to modern deep neural networks, like GPT. In my opinion language models are an excellent place to learn deep learning, even if your intention is to eventually go to other areas like computer vision because most of what you learn will be immediately transferable. This is why we dive into and focus on languade models.
Prerequisites: solid programming (Python), intro-level math (e.g. derivative, gaussian).

Learning is easier with others, come say hi in our Discord channel:

Syllabus
2h25m The spelled-out intro to neural networks and backpropagation: building micrograd
This is the most step-by-step spelled-out explanation of backpropagation and training of neural networks. It only assumes basic knowledge of Python and a vague recollection of calculus from high school.
1h57m The spelled-out intro to language modeling: building makemore
We implement a bigram character-level language model, which we will further complexify in followup videos into a modern Transformer language model, like GPT. In this video, the focus is on (1) introducing torch.Tensor and its subtleties and use in efficiently evaluating neural networks and (2) the overall framework of language modeling that includes model training, sampling, and the evaluation of a loss (e.g. the negative log likelihood for classification).
1h15m Building makemore Part 2: MLP
We implement a multilayer perceptron (MLP) character-level language model. In this video we also introduce many basics of machine learning (e.g. model training, learning rate tuning, hyperparameters, evaluation, train/dev/test splits, under/overfitting, etc.).
1h55m Building makemore Part 3: Activations & Gradients, BatchNorm
We dive into some of the internals of MLPs with multiple layers and scrutinize the statistics of the forward pass activations, backward pass gradients, and some of the pitfalls when they are improperly scaled. We also look at the typical diagnostic tools and visualizations you'd want to use to understand the health of your deep network. We learn why training deep neural nets can be fragile and introduce the first modern innovation that made doing so much easier: Batch Normalization. Residual connections and the Adam optimizer remain notable todos for later video.
1h55m Building makemore Part 4: Becoming a Backprop Ninja
We take the 2-layer MLP (with BatchNorm) from the previous video and backpropagate through it manually without using PyTorch autograd's loss.backward(): through the cross entropy loss, 2nd linear layer, tanh, batchnorm, 1st linear layer, and the embedding table. Along the way, we get a strong intuitive understanding about how gradients flow backwards through the compute graph and on the level of efficient Tensors, not just individual scalars like in micrograd. This helps build competence and intuition around how neural nets are optimized and sets you up to more confidently innovate on and debug modern neural networks.
56m Building makemore Part 5: Building a WaveNet
We take the 2-layer MLP from previous video and make it deeper with a tree-like structure, arriving at a convolutional neural network architecture similar to the WaveNet (2016) from DeepMind. In the WaveNet paper, the same hierarchical architecture is implemented more efficiently using causal dilated convolutions (not yet covered). Along the way we get a better sense of torch.nn and what it is and how it works under the hood, and what a typical deep learning development process looks like (a lot of reading of documentation, keeping track of multidimensional tensor shapes, moving between jupyter notebooks and repository code, ...).
1h56m Let's build GPT: from scratch, in code, spelled out.
We build a Generatively Pretrained Transformer (GPT), following the paper "Attention is All You Need" and OpenAI's GPT-2 / GPT-3. We talk about connections to ChatGPT, which has taken the world by storm. We watch GitHub Copilot, itself a GPT, help us write a GPT (meta :D!) . I recommend people watch the earlier makemore videos to get comfortable with the autoregressive language modeling framework and basics of tensors and PyTorch nn, which we take for granted in this video.



