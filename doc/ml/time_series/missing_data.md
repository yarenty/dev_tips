


## How to: Handle Missing Data for Time Series

https://towardsdatascience.com/how-to-handle-missing-data-for-time-series-680810f648ed



There is no such thing as a perfect dataset. Every data scientist knows that feeling during data exploration when they call:

Most ML models cannot process NaN or null values, so it is important that if your features or target contain them, they are dealt with appropriately before attempting to fit a model to the data.

1. Drop nulls
   This is probably the simplest and most straightforward way to handle missing data: Just get rid of it.

# Drop any and all nulls across all columns
df.dropna(inplace=True)
By default, pandas’ dropna function searches for nulls across the board (in all columns) and drops any row where there is a null in any column. However, this can be modified using various parameters.

