# stock_chart

# Process
User inputs stock symbol for company
The stock data gets downloaded from Yahoo Finance
SMA for 10 and 30 day intervals and EMA for 20 day intervals are calculated
SMA is simple moving average, which is the average closing price of a particular stock for n days (in this case 10 and 30 days)
EMA is very similar to SMA, except more recent days are weighed more heavily
The program will generate a chart displaying a stock's change in these 3 stats over the last 6 months

# How to use this chart to analyze stocks
This program does not predict future outcomes, but signals movements and trends that are happening for a company
It is important to note that moving averages tend to lag behind the actual stock price movement
An indcator of a stock rise that can be seen by using this chart is when a stock's price rises above its SMA, especially if volume is increasing
Beware of this, as this may mean that the stock is overvalued and rising too quickly, but more often it means that a company is doing well
Additionally, a signal that could indicate that a stock is on the decline is when the stock price dips below its SMA and EMAs

An additional large factor of note is when the short term SMA surpasses the long term SMA, suggesting that momentum is shifting upwards for the company
Similarly, if the 10 day SMA falls below the 30 day SMA, this can be seen as a very bad sign
This could signify that the stock is on the decline and it could be time to think about selling

As seen on the top right corner of the graph, the blue line is the closing price, the orange and green lines are the 10 and 30 day SMA's, respectively, and the red line is the 20-day EMA

import yfinance as yf
import pandas as pd
import matplotlib.pyplot as mpl

# Allow user to input stock symbol
symbol = input("Enter stock symbol: ").upper()
data = yf.download(symbol, period="6mo")

# Calculating SMAs and EMAs
data["SMA10"] = data["Close"].rolling(window=10).mean()
data["SMA30"] = data["Close"].rolling(window=30).mean()
data["EMA20"] = data["Close"].ewm(span=20, adjust=False).mean()

# Plotting the data using MatPlotLib (MPL)
mpl.figure(figsize=(14, 7))
mpl.plot(data["Close"], label="Close Price", linewidth=1.5)
mpl.plot(data["SMA10"], label="SMA 10")
mpl.plot(data["SMA30"], label="SMA 30")
mpl.plot(data["EMA20"], label="EMA 20")
mpl.title(f"{symbol} Moving Averages")
mpl.xlabel("Date")
mpl.ylabel("Price")
mpl.legend()
mpl.grid(True)
mpl.tight_layout()
mpl.show()
