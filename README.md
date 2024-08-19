# Granger Causaity between DXY and Gold (DXY cause Gold)
# Python
import yfinance as yf
import pandas as pd
from statsmodels.tsa.stattools import adfuller

# Download historical gold futures data from Yahoo Finance with daily frequency
gold_data = yf.download('GC=F', start='2010-01-01', end='2024-06-01', interval='1d')

# Extract the 'Adj Close' column, which represents the adjusted closing prices, and drop any missing values
gold_prices = gold_data['Adj Close'].dropna()

# Perform the Augmented Dickey-Fuller (ADF) test on the gold prices to check for stationarity
adf_test = adfuller(gold_prices)

# Print the ADF statistic, which indicates the level of stationarity
print(f'ADF Statistic: {adf_test[0]}')
# Print the p-value, which helps in determining the significance of the ADF statistic
print(f'p-value: {adf_test[1]}')
# Print the critical values for different significance levels (1%, 5%, 10%) to compare with the ADF statistic
for key, value in adf_test[4].items():
    print(f'Critical Value {key}: {value}')

# Apply first differencing to the gold prices to remove trends and make the series stationary
gold_diff = gold_prices.diff().dropna()

# Perform the ADF test on the differenced gold prices
adf_test_diff = adfuller(gold_diff)

# Print the ADF statistic after differencing
print(f'ADF Statistic after differencing: {adf_test_diff[0]}')
# Print the p-value after differencing
print(f'p-value after differencing: {adf_test_diff[1]}')
# Print the critical values after differencing for comparison
for key, value in adf_test_diff[4].items():
    print(f'Critical Value {key}: {value}')

# Download historical data for the US Dollar Index (DXY) with daily frequency
dxy_data = yf.download('DX-Y.NYB', start='2010-01-01', end='2024-06-01', interval='1d')

# Extract the 'Adj Close' column for DXY and drop any missing values
dxy_prices = dxy_data['Adj Close'].dropna()

# Perform the ADF test on the DXY prices to check for stationarity
adf_test_dxy = adfuller(dxy_prices)

# Print the ADF statistic for DXY
print(f'ADF Statistic: {adf_test_dxy[0]}')
# Print the p-value for DXY
print(f'p-value: {adf_test_dxy[1]}')
# Print the critical values for DXY to compare with the ADF statistic
for key, value in adf_test_dxy[4].items():
    print(f'Critical Value {key}: {value}')

# Apply first differencing to the DXY prices to make the series stationary
dxy_diff = dxy_prices.diff().dropna()

# Perform the ADF test on the differenced DXY prices
adf_test_diff_dxy = adfuller(dxy_diff)

# Print the ADF statistic after differencing for DXY
print(f'ADF Statistic after differencing: {adf_test_diff_dxy[0]}')
# Print the p-value after differencing for DXY
print(f'p-value after differencing: {adf_test_diff_dxy[1]}')
# Print the critical values after differencing for DXY
for key, value in adf_test_diff_dxy[4].items():
    print(f'Critical Value {key}: {value}')

# Combine the differenced gold and DXY data into a single DataFrame
data_combined = pd.concat([gold_diff, dxy_diff], axis=1).dropna()
# Rename the columns for clarity
data_combined.columns = ['gold_diff', 'dxy_diff']

# Print the first few rows of the combined DataFrame to check the data
print(data_combined.head())
# Print the last few rows of the combined DataFrame to check the data
print(data_combined.tail())

from statsmodels.tsa.stattools import grangercausalitytests

# Perform Granger Causality tests to determine if one time series can predict another
result = grangercausalitytests(data_combined[['gold_diff', 'dxy_diff']], maxlag=200)

# Print the results of the Granger Causality tests
print(result)
