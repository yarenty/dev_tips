# Kolmogorov-Arnold Networks (KANs) for Time Series Forecasting



https://arxiv.org/pdf/2404.19756



Discover the Kolmogorov-Arnold Networks (KANs) and apply them for time series forecasting using Python
Marco Peixeiro
Towards Data Science
Marco Peixeiro


https://towardsdatascience.com/kolmogorov-arnold-networks-kans-for-time-series-forecasting-9d49318c3172

Photo by Eduardo Bergen on Unsplash
The multilayer perceptron (MLP) is one of the foundational structures of deep learning models. It is also the building block of many state-of-the-art forecasting models, like N-BEATS, NHiTS and TSMixer.

https://towardsdatascience.com/the-easiest-way-to-forecast-time-series-using-n-beats-d778fcc2ba60

https://towardsdatascience.com/all-about-n-hits-the-latest-breakthrough-in-time-series-forecasting-a8ddcb27b0d5



https://arxiv.org/pdf/2201.12886



https://towardsdatascience.com/tsmixer-the-latest-forecasting-model-by-google-2fd1e29a8ccb



On April 30, 2024, the paper KAN: Kolmogorov-Arnold Network was published, and it has attracted the attention of many practitioners in the field of deep learning. There, the authors propose an alternative to the MLP: the Kolmogorov-Arnold Network or KAN.

Instead of using weights and fixed activation functions, the KAN uses learnable functions that are parametrized as splines. The researchers suggest the KAN can thus be more accurate with less trainable parameters than MLPs.

In this article, we first explore splines, as they help us understand the architecture and key elements of KAN. Then, we make a deep dive inside the inner workings of KAN. Finally, we apply KAN to time series forecasting, and evaluate its performance against the standard MLP and the N-BEATS model.