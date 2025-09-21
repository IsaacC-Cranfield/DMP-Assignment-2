# This code downloads Apple stock data for 2023, calculates daily returns and rolling volatility,
# and generates two graphs: one for stock price and another for volatility.

import yfinance as yf
import pandas as pd
import matplotlib.pyplot as plt
import matplotlib.dates as mdates

# 1. Download Apple stock data (Jan–Dec 2023)
apple = yf.download("AAPL", start="2023-01-01", end="2023-12-31")

# 2. Calculate daily returns
apple['Daily Return'] = apple['Close'].pct_change()

# 3. Calculate rolling volatility (30-day window, annualised)
apple['Volatility (30d)'] = apple['Daily Return'].rolling(window=30).std() * (252 ** 0.5)

fig, axs = plt.subplots(2, 1, figsize=(12, 12))

# --- Graph 1: Apple Stock Price ---
axs[0].plot(apple.index, apple['Close'], label='Apple Stock Price', color='green')
axs[0].set_title("Apple (AAPL) Stock Price - Jan to Dec 2023")
axs[0].set_xlabel("Date")
axs[0].set_ylabel("Price (USD)")
axs[0].legend()
axs[0].grid(True)
axs[0].xaxis.set_major_locator(mdates.MonthLocator(interval=1))
axs[0].xaxis.set_major_formatter(mdates.DateFormatter("%b"))

# --- Graph 2: Apple Stock Volatility ---
axs[1].plot(apple.index, apple['Volatility (30d)'], label='30-Day Rolling Volatility (Annualised)', color='blue')
axs[1].set_title("Apple (AAPL) Stock Volatility - Jan to Dec 2023")
axs[1].set_xlabel("Date")
axs[1].set_ylabel("Volatility (Annualised)")
axs[1].legend()
axs[1].grid(True)
axs[1].xaxis.set_major_locator(mdates.MonthLocator(interval=1))
axs[1].xaxis.set_major_formatter(mdates.DateFormatter("%b"))

plt.tight_layout()
plt.show()
