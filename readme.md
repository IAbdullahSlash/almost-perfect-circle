# Professional Stock Analysis Software - Upstox Edition

A comprehensive desktop application for analyzing Indian stock market data using the Upstox API. Built with PyQt5 and advanced technical indicators, this software provides professional-grade charting, real-time data, and market index browsing capabilities.

---

## Table of Contents

1. [Project Overview](#project-overview)
2. [Theoretical Foundation](#theoretical-foundation)
3. [Technical Architecture](#technical-architecture)
4. [Features](#features)
5. [Installation & Setup](#installation--setup)
6. [Usage Guide](#usage-guide)
7. [Project Structure](#project-structure)
8. [API Integration](#api-integration)
9. [Technical Indicators Explained](#technical-indicators-explained)
10. [Troubleshooting](#troubleshooting)
11. [Dependencies](#dependencies)

---

## Project Overview

This project is a **professional-grade desktop stock analysis platform** designed for Indian investors and traders. It integrates with the **Upstox API** to fetch real-time stock data from the National Stock Exchange (NSE) and the Bombay Stock Exchange (BSE).

### Key Objectives:
- Provide easy access to major Indian stock indices (Nifty 50, Bank Nifty, Sensex)
- Display live market data for all constituent stocks
- Analyze individual stocks with advanced technical indicators
- Offer intuitive charting and visualization
- Enable data export for further analysis
- Support manual stock symbol search

### Target Users:
- Individual investors
- Day traders
- Technical analysis enthusiasts
- Financial analysts

---

## Theoretical Foundation

### Stock Market Indices

#### 1. **Nifty 50**
- **Composition**: 50 large-cap companies listed on NSE
- **Representation**: Covers ~95% of the market capitalization of NSE
- **Sectors**: Diversified across finance, IT, pharma, energy, FMCG, etc.
- **Use Case**: Benchmark for overall market health and economic indicators

#### 2. **Bank Nifty**
- **Composition**: 12 banking sector stocks
- **Representation**: Tracks banking sector performance
- **Importance**: Banking sector is crucial for understanding credit conditions and economic growth
- **Volatility**: Generally higher volatility than Nifty 50

#### 3. **Sensex (BSE Sensex)**
- **Composition**: 30 large-cap companies listed on BSE
- **Representation**: Established, blue-chip companies
- **Correlation**: Highly correlated with Nifty 50 as they contain similar stocks
- **Historical Significance**: India's oldest stock market index

### Technical Analysis Concepts

Technical analysis is the study of historical price and volume data to predict future price movements. The software implements several key indicators:

#### **1. Moving Averages (MA)**
- **MA 20**: 20-day average price - detects short-term trends
- **MA 50**: 50-day average price - detects intermediate trends
- **Interpretation**: 
  - Price above MA = Uptrend
  - Price below MA = Downtrend
  - Golden Cross (MA 20 > MA 50) = Bullish signal
  - Death Cross (MA 20 < MA 50) = Bearish signal

#### **2. Bollinger Bands**
- **Calculation**: Middle Band (20-day MA) ± (2 × 20-day Standard Deviation)
- **Purpose**: Identify overbought/oversold conditions
- **Interpretation**:
  - Price touching upper band = Overbought (potential sell)
  - Price touching lower band = Oversold (potential buy)
  - Band width = Market volatility

#### **3. RSI (Relative Strength Index)**
- **Range**: 0-100
- **Calculation**: RSI = 100 - (100 / (1 + RS)), where RS = Average Gain / Average Loss
- **Thresholds**:
  - RSI > 70 = Overbought condition
  - RSI < 30 = Oversold condition
- **Use**: Identify reversal points and momentum changes

#### **4. MACD (Moving Average Convergence Divergence)**
- **Components**:
  - MACD Line = 12-day EMA - 26-day EMA
  - Signal Line = 9-day EMA of MACD
  - Histogram = MACD - Signal Line
- **Signals**:
  - MACD crosses above Signal = Bullish
  - MACD crosses below Signal = Bearish
  - Histogram magnitude = Momentum strength

#### **5. Volume Analysis**
- **Purpose**: Confirm price movements
- **Interpretation**:
  - High volume on price increase = Strong bullish conviction
  - High volume on price decrease = Strong bearish conviction
  - Low volume = Weak move (may reverse)

---

## Technical Architecture

### System Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│         Desktop Application Layer (PyQt5 GUI)           │
│  ┌──────────────────────────────────────────────────┐   │
│  │  Main Window (main_desktop.py)                   │   │
│  │  ├─ Authentication Section                       │   │
│  │  ├─ Index Selection (Nifty 50, Bank Nifty, ...)│   │
│  │  ├─ Stock Search & Analysis                      │   │
│  │  ├─ Technical Indicators Toggle                  │   │
│  │  └─ Chart Visualization (Advanced Chart Widget) │   │
│  └──────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────┐   │
│  │  Index Stock Dialog (index_stock_dialog.py)      │   │
│  │  ├─ Live Stock Data Table                        │   │
│  │  ├─ Real-time Price Updates                      │   │
│  │  ├─ Stock Filter/Search                          │   │
│  │  └─ Analyze Buttons                              │   │
│  └──────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────┐   │
│  │  Chart Widget (Advanced Charting)                │   │
│  │  ├─ Price Chart with Moving Averages             │   │
│  │  ├─ Volume Chart                                 │   │
│  │  ├─ RSI Indicator                                │   │
│  │  └─ MACD Indicator                               │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│        Business Logic & Data Processing Layer           │
│  ┌──────────────────────────────────────────────────┐   │
│  │  Upstox Stock Provider (upstox_provider.py)      │   │
│  │  ├─ OAuth2 Authentication                        │   │
│  │  ├─ Historical Data Fetching                     │   │
│  │  ├─ Real-time Quote Retrieval                    │   │
│  │  ├─ Technical Indicator Calculation              │   │
│  │  └─ Data Transformation (Pandas)                 │   │
│  └──────────────────────────────────────────────────┘   │
│  ┌──────────────────────────────────────────────────┐   │
│  │  Threading & Async Operations                    │   │
│  │  ├─ DataFetchThread (Background data loading)    │   │
│  │  └─ StockDataFetchThread (Index data loading)    │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
                          ↓
┌─────────────────────────────────────────────────────────┐
│           External APIs & Data Sources                  │
│  ┌──────────────────────────────────────────────────┐   │
│  │  Upstox API (https://api.upstox.com/v2)         │   │
│  │  ├─ Login/Authorization Endpoints                │   │
│  │  ├─ Historical Candle Data Endpoints             │   │
│  │  ├─ Market Quote Endpoints                       │   │
│  │  └─ Instrument Information Endpoints             │   │
│  └──────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### Data Flow Diagram

```
User Interaction
    ↓
Authentication (OAuth2)
    ↓
Select Index / Search Stock
    ↓
DataFetchThread (Background)
    ↓
API Request to Upstox
    ↓
Historical Data Retrieved
    ↓
Calculate Technical Indicators (NumPy/Pandas)
    ↓
Format Data (DataFrame)
    ↓
Emit Signal to GUI
    ↓
Render Charts (PyQtGraph)
    ↓
Display to User
```

### File Organization

```
Project Root
├── main_desktop.py              # Main application GUI
├── index_stock_dialog.py         # Stock list dialog for indices
├── upstox_provider.py            # Upstox API wrapper
├── run_desktop.py                # Application launcher
├── requirements_dashboard.py     # Dependency checker (optional)
└── README.md                     # This file
```

---

## Features

### 1. Authentication & Security
- OAuth2-based Upstox authentication
- Secure token management
- Manual token input for existing tokens
- Session persistence during application runtime

### 2. Index Browsing
- **Nifty 50**: 50 large-cap stocks
- **Bank Nifty**: 12 banking stocks
- **Sensex**: 30 blue-chip stocks
- Live data display for all constituent stocks
- Real-time price, change, and volume data

### 3. Stock Search & Analysis
- Manual symbol search with period selection
- Support for 1 month, 3 months, 6 months, 1 year, 2 years analysis
- Autocomplete stock symbols
- Rapid analysis with a single search

### 4. Advanced Charting
- **Price Chart**: Close price with moving averages
- **Volume Chart**: Trading volume over time
- **RSI Chart**: Momentum indicator with overbought/oversold zones
- **MACD Chart**: Trend-following momentum indicator
- Interactive zooming and panning
- Grid and legend support

### 5. Technical Indicators
- **Moving Averages**: 20-day, 50-day
- **Bollinger Bands**: Volatility measure with upper/lower bands
- **RSI**: Relative Strength Index (0-100 scale)
- **MACD**: Moving Average Convergence Divergence
- Toggle indicators on/off for cleaner charts

### 6. Data Export
- Export analyzed data to CSV format
- Timestamped filenames for organization
- Includes all calculated indicators

### 7. User Interface
- Professional, intuitive design
- Real-time status bar updates
- Progress indicators for data loading
- Color-coded status (green=good, red=issues)
- Responsive table layouts

---

## Installation & Setup

### Prerequisites
- Python 3.7 or higher
- pip (Python package manager)
- Active internet connection
- Upstox account and developer credentials

### Step 1: Get Upstox Credentials

1. Visit [Upstox Developer Console](https://upstox.com/developer/apps)
2. Create a new app with these settings:
   - **Redirect URI**: `http://localhost:5000/callback`
   - Note your `Client ID` and `Client Secret`

### Step 2: Update Credentials in Code

Edit `upstox_provider.py` and update:

```python
self.client_id = "YOUR_CLIENT_ID"
self.client_secret = "YOUR_CLIENT_SECRET"
self.redirect_uri = "http://localhost:5000/callback"
```

**Important**: These must EXACTLY match your Upstox app settings (case-sensitive, including redirect URI format).

### Step 3: Install Dependencies

**Option A - Automatic Installation**
```bash
python run_desktop.py
```
The launcher will automatically detect and install missing packages.

**Option B - Manual Installation**
```bash
pip install PyQt5>=5.15.0 pyqtgraph>=0.13.0 matplotlib>=3.5.0 pandas>=1.5.0 numpy>=1.21.0 requests>=2.28.0 urllib3>=1.26.0
```

**One-liner Installation**
```bash
pip install PyQt5>=5.15.0 pyqtgraph>=0.13.0 matplotlib>=3.5.0 pandas>=1.5.0 numpy>=1.21.0 requests>=2.28.0 urllib3>=1.26.0
```

### Step 4: Run the Application

```bash
python run_desktop.py
```

Or directly:

```bash
python main_desktop.py
```

---

## Usage Guide

### Initial Setup (First Time)

1. **Launch Application**
   ```bash
   python run_desktop.py
   ```

2. **Authenticate**
   - Click "🔑 Authenticate with Upstox" button
   - Browser opens with Upstox login page
   - Log in with your Upstox credentials
   - After successful login, you'll be redirected to Google.com
   - Copy the authorization code from the URL (e.g., `6tapmE123abc456`)
   - Paste it in the dialog box and click OK

3. **Verify Authentication**
   - Status should change to "✅ Authenticated"
   - Text should turn green

### Browsing Indices

1. **Select an Index**
   - Click one of: "📈 Nifty 50", "🏦 Bank Nifty", "📊 Sensex"
   
2. **View Stock List**
   - Dialog opens showing all stocks in that index
   - Real-time data loads automatically
   - See columns: Symbol, Company, Current Price, Change, Volume

3. **Filter Stocks**
   - Use the search box to filter by symbol or company name
   - Example: Type "INFY" to find Infosys

4. **Analyze a Stock**
   - Click "📊 Analyze" button for any stock
   - Application loads detailed chart and indicators

### Analyzing Individual Stocks

1. **Search for a Stock**
   - Enter stock symbol in "Manual Search" field
   - Example symbols: `RELIANCE`, `TCS`, `INFY`, `HDFCBANK`

2. **Select Analysis Period**
   - Choose from: 1 Month, 3 Months, 6 Months, 1 Year (default), 2 Years

3. **Toggle Indicators**
   - Check/uncheck: MA 20, MA 50, Bollinger Bands
   - Update chart in real-time

4. **Click "🔍 Analyze Stock"**
   - Loading bar appears
   - Chart and indicators display when ready

5. **Interpret the Charts**
   - **Price Chart (Top)**: Main price movement with moving averages
   - **Volume Chart**: Trading activity intensity
   - **RSI Chart**: Overbought (>70) and oversold (<30) zones
   - **MACD Chart**: Trend confirmation and momentum

### Exporting Data

1. **Analyze a Stock** (as per above steps)

2. **Go to Menu → File → Export Data**

3. **File saved as**: `SYMBOL_upstox_data_YYYYMMDD_HHMMSS.csv`

4. **Use in Excel/Python** for further analysis

---

## Project Structure

### main_desktop.py
**Purpose**: Main GUI application and user interface

**Key Classes**:
- `AuthDialog`: OAuth2 authentication dialog
- `DataFetchThread`: Background thread for data fetching
- `AdvancedChartWidget`: Multi-chart visualization widget
- `StockAnalysisMainWindow`: Main application window

**Key Methods**:
- `authenticate()`: Handles Upstox OAuth2 flow
- `search_stock()`: Fetches and displays stock analysis
- `export_data()`: Exports analyzed data to CSV

**UI Components**:
- Authentication section with OAuth2 and manual token input
- Index buttons (Nifty 50, Bank Nifty, Sensex)
- Stock search with period selector
- Technical indicator toggles
- Multi-layer chart display
- Status bar for feedback

### index_stock_dialog.py
**Purpose**: Dialog for browsing and filtering index stocks

**Key Classes**:
- `StockDataFetchThread`: Fetches live data for multiple stocks
- `IndexStockDialog`: Stock list dialog with live data

**Key Methods**:
- `load_stock_data()`: Initiates data fetching for all stocks
- `add_stock_row()`: Adds row to stock table
- `filter_stocks()`: Filters table by search text
- `select_stock()`: Handles stock selection for analysis

**Features**:
- Live data table with sorting
- Search/filter functionality
- Refresh button for real-time updates
- Analyze buttons for quick stock selection
- Color-coded price changes (green/red)

### upstox_provider.py
**Purpose**: Upstox API wrapper and data processing

**Key Classes**:
- `UpstoxStockProvider`: Main API client

**Key Methods**:
- `get_login_url()`: Generates OAuth2 login URL
- `get_access_token()`: Exchanges auth code for token
- `get_historical_data()`: Fetches candlestick data
- `get_stock_data()`: Complete data retrieval and processing
- `calculate_technical_indicators()`: Computes all indicators
- `get_quote()`: Gets real-time stock quotes

**Data Processing**:
- Converts Upstox candle format to Pandas DataFrame
- Calculates all technical indicators using NumPy
- Handles timestamp conversion (ISO 8601 format)
- Supports multiple timeframes and date ranges

### run_desktop.py
**Purpose**: Application launcher and dependency manager

**Key Functions**:
- `check_requirements()`: Verifies installed packages
- `install_missing_packages()`: Installs via pip
- `launch_desktop_app()`: Starts main application
- `main()`: Entry point with version checking

**Features**:
- Automatic dependency installation
- Python version validation (3.7+)
- User-friendly startup messages
- Error handling and troubleshooting tips

---

## API Integration

### Upstox API Overview

Upstox provides comprehensive market data APIs for Indian stock exchanges (NSE, BSE).

### Authentication Flow

```
1. User clicks "Authenticate"
    ↓
2. Generate OAuth2 login URL with client_id
    ↓
3. Open browser for user login
    ↓
4. User logs in with Upstox credentials
    ↓
5. Browser redirected with authorization code in URL
    ↓
6. User pastes authorization code in dialog
    ↓
7. Application exchanges code for access token
    ↓
8. Token stored and used for all API calls
    ↓
9. Token added to Authorization header: "Bearer {token}"
```

### API Endpoints Used

**1. Authorization Token Endpoint**
```
POST https://api.upstox.com/v2/login/authorization/token
Purpose: Exchange authorization code for access token
```

**2. Historical Candle Data Endpoint**
```
GET https://api.upstox.com/v2/historical-candle/{instrument_key}/{interval}/{to_date}/{from_date}
Purpose: Fetch historical candlestick data
Parameters:
  - instrument_key: Unique identifier (e.g., NSE_EQ|INE002A01018)
  - interval: "day" for daily data
  - to_date: End date (YYYY-MM-DD)
  - from_date: Start date (YYYY-MM-DD)
```

**3. Market Quote Endpoint**
```
GET https://api.upstox.com/v2/market-quote/quotes
Purpose: Get real-time quotes
Parameters:
  - instrument_key: Stock identifier
Returns: Price, change, volume, etc.
```

### Error Handling

Common Errors and Solutions:

1. **UDAPI100068**: Client ID or Redirect URI mismatch
   - Solution: Verify credentials match exactly in Upstox Developer Console

2. **401 Unauthorized**: Invalid or expired token
   - Solution: Re-authenticate or manually set a fresh token

3. **No data found for {symbol}**: Invalid instrument key
   - Solution: Verify stock symbol is correct and listed on NSE/BSE

4. **could not convert string to float**: Data type conversion error
   - Solution: Application handles this automatically - retry search

---

## Technical Indicators Explained

### Moving Averages (MA)

**Formula**:
```
MA_n = (Price₁ + Price₂ + ... + Priceₙ) / n
```

**Example (20-day MA)**:
- Sum last 20 closing prices
- Divide by 20
- Creates smooth trend line
- Filters out daily noise

**Trading Signals**:
- Price > MA = Uptrend (bullish)
- Price < MA = Downtrend (bearish)
- MA 20 > MA 50 = Golden Cross (strong buy)
- MA 20 < MA 50 = Death Cross (strong sell)

### Bollinger Bands

**Formula**:
```
Middle Band = 20-day SMA
Upper Band = Middle + (2 × 20-day Std Dev)
Lower Band = Middle - (2 × 20-day Std Dev)
```

**Interpretation**:
- **Width**: Measures volatility
  - Wide bands = High volatility
  - Narrow bands = Low volatility, expansion coming
  
- **Price Position**:
  - At upper band = Overbought (potential sell)
  - At lower band = Oversold (potential buy)
  - Mean reversion occurs ~95% of time

### RSI (Relative Strength Index)

**Formula**:
```
RS = Average Gain / Average Loss
RSI = 100 - (100 / (1 + RS))
Range: 0-100
```

**Thresholds**:
- RSI > 70: Overbought (potential reversal down)
- RSI > 50: Moderate uptrend
- RSI = 50: Neutral
- RSI < 30: Oversold (potential reversal up)
- RSI < 50: Moderate downtrend

**Divergence Trading**:
- Price makes new high but RSI doesn't → Reversal signal
- Price makes new low but RSI doesn't → Reversal signal

### MACD (Moving Average Convergence Divergence)

**Formula**:
```
MACD = 12-day EMA - 26-day EMA
Signal Line = 9-day EMA of MACD
Histogram = MACD - Signal Line
```

**Trading Signals**:
1. **Crossover**: MACD crosses above/below Signal Line
   - Above = Bullish momentum
   - Below = Bearish momentum

2. **Histogram**: Shows momentum strength
   - Expanding = Strengthening trend
   - Contracting = Weakening trend
   - Zero cross = Trend change

3. **Divergence**: Price moves but MACD doesn't
   - Indicates weakening trend (reversal likely)

### Volume Analysis

**Importance**:
- Confirms price movements
- Shows strength of conviction
- Identifies potential reversals

**Interpretation**:
- **High Volume + Price Up** = Strong buying
- **High Volume + Price Down** = Strong selling
- **Low Volume + Price Move** = Weak move (unreliable)
- **Volume Spike** = Significant event or reversal

---

## Troubleshooting

### Installation Issues

**Problem**: `ModuleNotFoundError: No module named 'PyQt5'`
**Solution**:
```bash
pip install --upgrade PyQt5
```

**Problem**: Version conflicts
**Solution**:
```bash
pip install --upgrade PyQt5 pyqtgraph matplotlib pandas numpy requests
```

### Authentication Issues

**Problem**: `UDAPI100068: Check your 'client_id' and 'redirect_uri'`
**Solution**:
1. Go to Upstox Developer Console
2. Copy exact Client ID from your app
3. Verify Redirect URI is: `http://localhost:5000/callback`
4. Update `upstox_provider.py` with exact values
5. Ensure no extra spaces or typos

**Problem**: Short authorization code (e.g., `6tapmE`)
**Solution**:
- This indicates invalid credentials
- Create new app in Upstox Developer Console
- Get fresh credentials
- Try again with correct credentials

**Problem**: `Failed to get access token`
**Solution**:
1. Verify authorization code is correct
2. Check code hasn't expired (codes expire ~5-10 minutes)
3. Get a fresh code by clicking "Open Login Page" again
4. Ensure internet connection is stable

### Data Fetching Issues

**Problem**: `No data found for {symbol}`
**Solution**:
- Verify stock symbol is correct
- Check symbol is listed on NSE/BSE
- Try alternative symbols (e.g., `SBIN` instead of `SBI`)
- Ensure authentication is successful

**Problem**: Data appears with gaps or missing values
**Solution**:
- This is normal for weekend/holiday/market closure days
- Historical data only includes trading days
- Charts handle missing data automatically

### Chart Display Issues

**Problem**: Charts appear blank or don't update
**Solution**:
1. Check data loaded successfully (status bar message)
2. Try zooming out or panning
3. Toggle indicators off/on to refresh
4. Search for stock again

**Problem**: Charts very slow or frozen
**Solution**:
- Select shorter time period (1 Month instead of 2 Years)
- This reduces data points
- Performance improves significantly

### Performance Issues

**Problem**: Application freezes during data loading
**Solution**:
- This is normal for large date ranges
- Application uses threading to prevent freezing
- Wait for progress bar to complete
- Try shorter time periods (1 Month, 3 Months)

**Problem**: Index stock list loads slowly
**Solution**:
- Nifty 50 requires fetching 50 stocks' data
- Click "Refresh Data" after initial load for faster updates
- First load may take 30-60 seconds
- Subsequent loads are faster due to API response caching

### Common Error Messages

| Error | Cause | Solution |
|-------|-------|----------|
| "Access token required" | Not authenticated | Click authenticate button and complete OAuth2 |
| "Error fetching data" | Network issue | Check internet connection |
| "Invalid instrument key" | Stock symbol not found | Verify correct NSE/BSE symbol |
| "Error parsing response" | API format changed | Update `upstox_provider.py` |
| "Connection timeout" | API server slow/down | Wait and retry after few minutes |

---

## Dependencies

### Core Libraries

| Package | Version | Purpose |
|---------|---------|---------|
| `PyQt5` | >=5.15.0 | Desktop GUI framework |
| `pyqtgraph` | >=0.13.0 | Advanced charting and plotting |
| `pandas` | >=1.5.0 | Data manipulation and analysis |
| `numpy` | >=1.21.0 | Numerical computations for indicators |
| `matplotlib` | >=3.5.0 | Additional visualization support |
| `requests` | >=2.28.0 | HTTP requests for API calls |
| `urllib3` | >=1.26.0 | URL handling and connection pooling |

### Installation Command (All-in-One)

```bash
pip install PyQt5>=5.15.0 pyqtgraph>=0.13.0 matplotlib>=3.5.0 pandas>=1.5.0 numpy>=1.21.0 requests>=2.28.0 urllib3>=1.26.0
```

### Version Compatibility

- **Python**: 3.7, 3.8, 3.9, 3.10, 3.11, 3.12+
- **Operating Systems**: Windows, macOS, Linux
- **Upstox API**: v2 (current as of 2024)

---

## Advanced Usage

### Custom Indicators

To add your own technical indicator:

1. **Edit `upstox_provider.py`**
2. **Add to `calculate_technical_indicators()` method**:

```python
# Example: Stochastic Oscillator
def stochastic_oscillator(self, df, period=14):
    lowest_low = df['Low'].rolling(window=period).min()
    highest_high = df['High'].rolling(window=period).max()
    df['K%'] = 100 * (df['Close'] - lowest_low) / (highest_high - lowest_low)
    df['D%'] = df['K%'].rolling(window=3).mean()
    return df
```

3. **Call in `calculate_technical_indicators()`**:
```python
df = self.stochastic_oscillator(df)
```

4. **Plot in `main_desktop.py`** in `AdvancedChartWidget`

### Backtesting

Use exported CSV data with Python for backtesting:

```python
import pandas as pd
df = pd.read_csv('RELIANCE_upstox_data_*.csv')
# Implement your trading strategy on this data
```

### Real-Time Price Alerts

To add price alerts, create a background thread that:
1. Periodically calls `get_quote()` method
2. Compares price to target
3. Triggers notification when target reached

---

## Contributing & Development

### Project Structure for Extensions

```
├── core/
│   ├── api/          # API wrappers
│   └── indicators/   # Technical indicators
├── ui/
│   ├── widgets/      # Custom UI widgets
│   └── dialogs/      # Dialog windows
├── utils/
│   ├── formatters/   # Data formatting
│   └── validators/   # Input validation
└── tests/            # Unit tests
```

### Adding New Indices

Edit `main_desktop.py` in `get_index_stocks()`:

```python
"NIFTY_IT": ["INFY", "TCS", "WIPRO", "TECHM", "HCLTECH", ...]
```

---

## Support & Resources

### Upstox API Documentation
- [Official Docs](https://upstox.com/developer/api-documentation)
- [GitHub Repository](https://github.com/upstox/upstox-python)

### Technical Analysis Resources
- Investopedia: Moving Averages, RSI, MACD, Bollinger Bands
- StockCharts: ChartSchool for technical analysis
- Trading View: Advanced charting and idea sharing

### Python Documentation
- [PyQt5 Docs](https://doc.qt.io/qt-5/)
- [Pandas Docs](https://pandas.pydata.org/docs/)
- [NumPy Docs](https://numpy.org/doc/)

---

## Disclaimer

This software is provided for educational and informational purposes only. It is not financial advice. Always conduct your own research and consult with a financial advisor before making investment decisions. Past performance does not guarantee future results.

---

## License

This project is open-source and available for educational use.

---

## Changelog

### Version 1.0 (Current)
- Initial release with Upstox API integration
- Nifty 50, Bank Nifty, Sensex index browsing
- Advanced technical indicators (MA, BB, RSI, MACD)
- OAuth2 authentication
- Multi-timeframe analysis
- Data export functionality

---

**Created**: 2024 | **Last Updated**: 2024
**Author**: Professional Stock Analysis Software Team
**Platform**: Python 3.7+, Cross-Platform (Windows, macOS, Linux)
