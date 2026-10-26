# Ex.No: 1B                     CONVERSION OF NON STATIONARY TO STATIONARY DATA
# Date: 

### AIM:
To perform regular differncing,seasonal adjustment and log transformatio on international airline passenger data
### ALGORITHM:
1. Import the required packages like pandas and numpy
2. Read the data using the pandas
3. Perform the data preprocessing if needed and apply regular differncing,seasonal adjustment,log transformation.
4. Plot the data according to need, before and after regular differncing,seasonal adjustment,log transformation.
5. Display the overall results.
### PROGRAM:
```
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from statsmodels.tsa.seasonal import seasonal_decompose
​
# Load dataset
data = pd.read_csv("World-Stock-Prices-Dataset.csv")
​
# Clean column names
data.columns = data.columns.str.strip().str.lower()
print("Columns:", data.columns)
​
# Convert date column to datetime
data['date'] = pd.to_datetime(data['date'], errors='coerce')
​
# Drop invalid dates
data = data.dropna(subset=['date'])
​
# Filter for a specific stock ticker (e.g., 'aapl' for Apple) if the dataset contains multiple tickers
if 'ticker' in data.columns:
    ticker_symbol = 'aapl'
    data = data[data['ticker'].str.lower() == ticker_symbol]
    print(f"Filtered for ticker: {ticker_symbol.upper()}")
​
# Set index and sort by date
data.set_index('date', inplace=True)
data = data.sort_index()
​
# Remove duplicate dates if any
data = data[~data.index.duplicated(keep='first')]
​
# Choose stock price column (e.g., 'close' or 'adj_close')
col = 'close'
print("Using column:", col)
​
# Regular Differencing
data['price_diff'] = data[col] - data[col].shift(1)
​
# Seasonal Decomposition (period=252 for trading days in a year, or 30 for monthly/5 for weekly depending on frequency)
result = seasonal_decompose(data[col].dropna(), model='additive', period=30)
data['price_sea_diff'] = result.resid
​
# Log Transformation
data['price_log'] = np.log(data[col])
​
# Log Differencing
data['price_log_diff'] = data['price_log'] - data['price_log'].shift(1)
​
# Seasonal decomposition on log differenced
result2 = seasonal_decompose(data['price_log_diff'].dropna(), model='additive', period=30)
data['price_log_seasonal_diff'] = result2.resid
​
# Plotting
plt.figure(figsize=(16, 16))
​
plt.subplot(6, 1, 1)
plt.plot(data[col])
plt.title('Original Stock Price')
​
plt.subplot(6, 1, 2)
plt.plot(data['price_diff'])
plt.title('Regular Differencing')
​
plt.subplot(6, 1, 3)
plt.plot(data['price_sea_diff'])
plt.title('Seasonal Adjustment (Residuals)')
​
plt.subplot(6, 1, 4)
plt.plot(data['price_log'])
plt.title('Log Transformation')
​
plt.subplot(6, 1, 5)
plt.plot(data['price_log_diff'])
plt.title('Log + Differencing')
​
plt.subplot(6, 1, 6)
plt.plot(data['price_log_seasonal_diff'])
plt.title('Final Stationary (Log + Seasonal Diff)')
​
plt.tight_layout()
plt.show()
```


### OUTPUT:
<img width="792" height="142" alt="image" src="https://github.com/user-attachments/assets/b56e8bf6-7054-43d6-ba19-12494c50d0a3" />


REGULAR DIFFERENCING:
<img width="1231" height="205" alt="image" src="https://github.com/user-attachments/assets/d79395f2-2072-4a12-9371-90d2909001f5" />




SEASONAL ADJUSTMENT:
<img width="1251" height="202" alt="image" src="https://github.com/user-attachments/assets/b6437473-25ea-4b83-84ea-957f85c9abb7" />



LOG TRANSFORMATION:
<img width="1232" height="210" alt="image" src="https://github.com/user-attachments/assets/98625acc-df83-4f48-a1f2-f990a1a9033b" />




### RESULT:
Thus we have created the python code for the conversion of non stationary to stationary data on international airline passenger
data.
