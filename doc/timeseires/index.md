
# Prophet


https://facebook.github.io/prophet/


https://medium.com/@cuongduong_35162/facebook-prophet-in-2023-and-beyond-c5086151c138


https://github.com/facebook/prophet/tree/main/examples



https://www.imperva.com/blog/anomaly-detection-at-scale-using-sql-and-facebooks-prophet-forecasting-algorithm/
Anomaly Detection at Scale Using SQL and Facebook’s Prophet Forecasting Algorithm



https://medium.com/@reza.rajabi/outlier-and-anomaly-detection-using-facebook-prophet-in-python-3a83d58b1bdf
Outlier and Anomaly Detection using Facebook Prophet in Python



https://www.kaggle.com/code/chienhsianghung/anomaly-outlier-detection-isolationforest-prophet
Anomaly/Outlier Detection IsolationForest/Prophet




# SKTime

https://www.sktime.net/en/latest/

A unified framework for machine learning with time series.




# Neuralprophet

https://neuralprophet.com/
Explainable Forecasting at Scale
NeuralProphet bridges the gap between traditional time-series models and deep learning methods. It's based on PyTorch and can be installed using pip.

```python
from neuralprophet import NeuralProphet
import pandas as pd

df = pd.read_csv('toiletpaper_daily_sales.csv')

m = NeuralProphet()

metrics = m.fit(df, freq="D")

forecast = m.predict(df)
```




# Nixtla
https://nixtlaverse.nixtla.io/statsforecast/index.html


StatsForecast offers a collection of popular univariate time series forecasting models optimized for high performance and scalability.


# time series
2y old
https://github.com/jmacadie/timeseries


# TSDownSample

https://github.com/predict-idlab/tsdownsample

Extremely fast time series downsampling 📈 for visualization, written in Rust.


