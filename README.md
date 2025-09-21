# DMP-Assignment-2

# Install yfinance if not already installed
# !pip install yfinance

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

# --- Graph 1: Apple Stock Price ---
plt.figure(figsize=(12,6))
plt.plot(apple.index, apple['Close'], label='Apple Stock Price', color='green')
plt.title("Apple (AAPL) Stock Price - Jan to Dec 2023")
plt.xlabel("Date")
plt.ylabel("Price (USD)")
plt.legend()
plt.grid(True)

# Format x-axis for readability
plt.gca().xaxis.set_major_locator(mdates.MonthLocator(interval=1))
plt.gca().xaxis.set_major_formatter(mdates.DateFormatter("%b"))
plt.show()

# --- Graph 2: Apple Stock Volatility ---
plt.figure(figsize=(12,6))
plt.plot(apple.index, apple['Volatility (30d)'], label='30-Day Rolling Volatility (Annualised)', color='blue')
plt.title("Apple (AAPL) Stock Volatility - Jan to Dec 2023")
plt.xlabel("Date")
plt.ylabel("Volatility (Annualised)")
plt.legend()
plt.grid(True)

# Format x-axis for readability
plt.gca().xaxis.set_major_locator(mdates.MonthLocator(interval=1))
plt.gca().xaxis.set_major_formatter(mdates.DateFormatter("%b"))
plt.show()
