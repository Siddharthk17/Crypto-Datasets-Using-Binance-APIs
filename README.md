# 🚀 Crypto Datasets Generator (Binance APIs)

![Python](https://img.shields.io/badge/Python-3.8%2B-blue.svg)
![Data](https://img.shields.io/badge/Data-Binance_Futures-yellow.svg)
![Performance](https://img.shields.io/badge/Performance-Parallel_Downloads-green.svg)
![License](https://img.shields.io/badge/License-MIT-lightgrey.svg)

**Generate professional-grade, high-fidelity 1-minute cryptocurrency datasets for algorithmic trading, backtesting, and machine learning.**

This repository contains highly optimized Python scripts to download, validate, and merge historical data from Binance Futures. Unlike simple OHLCV downloaders, this tool constructs a **comprehensive market view** by aggregating multiple data points into a single, clean Parquet or CSV file.

---

## ⚡ Features

-   **🏎️ High Performance**: Uses `ThreadPoolExecutor` to download years of data in parallel, drastically reducing wait times.
-   **🛡️ Robust Validation**: Automatically detects and fixes data issues:
    -   Removes duplicate timestamps.
    -   Checks for OHLC logical inconsistencies (e.g., High < Low).
    -   Identifies and reports time gaps.
-   **📦 Comprehensive Data**: Merges multiple data streams into one master dataset:
    -   **OHLCV**: Open, High, Low, Close, Volume (Spot & Taker).
    -   **Mark Price**: Critical for calculating liquidation levels.
    -   **Index Price**: The underlying asset price.
    -   **Funding Rates**: Historical funding rates (forward-filled).
    -   **Open Interest**: Recent open interest data.
-   **💾 Efficient Storage**: Saves data in **Parquet** format (highly compressed and fast to read) with scripts to convert to **CSV**.

---

## 🛠️ Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/Siddharthk17/Crypto-Datasets-Using-Binance-APIs.git
    cd Crypto-Datasets-Using-Binance-APIs
    ```

2.  **Install dependencies:**
    ```bash
    pip install pandas numpy requests tqdm pyarrow fastparquet
    ```

---

## 🚀 Usage

### 1. Generate Datasets
Run the generator script for your desired pair. The scripts are pre-configured for BTC and ETH but can be easily adapted for any pair.

**For Bitcoin (BTCUSDT):**
```bash
python btcusdt_1min.py
```

**For Ethereum (ETHUSDT):**
```bash
python ethusdt_1min.py
```

*This will create a `btc_perp_1m_dataset` (or similar) folder containing the `.parquet` file and a `.json` summary.*

### 2. Convert to CSV (Optional)
If you prefer CSV format over Parquet, use the included helper scripts:

```bash
python parquet_to_csv_BTCUSDT.py
```

---

## 📊 Data Structure

The generated dataset is incredibly rich. A single row represents one minute of trading data with the following columns:

| Column | Description |
| :--- | :--- |
| `timestamp_utc` | UTC Timestamp of the candle open |
| `open`, `high`, `low`, `close` | Standard OHLC price data |
| `volume`, `quote_volume` | Total volume in base asset and quote asset (USDT) |
| `num_trades` | Total number of trades in that minute |
| `taker_buy_base_vol` | Buying volume from market takers |
| `taker_sell_base_vol` | Selling volume from market takers (Derived) |
| `mark_open`, `mark_high`, ... | Mark Price OHLC (for liquidation logic) |
| `index_open`, `index_high`, ... | Index Price OHLC |
| `fundingRate` | The funding rate at that timestamp (8-hour intervals, ffilled) |
| `open_interest` | Total open interest (available for recent data) |

---

## ⚙️ Customization

Want to download data for **SOLUSDT**, **BNBUSDT**, or any other coin?

1.  Open `btcusdt_1min.py`.
2.  Change the `SYMBOL` variable at the top of the file:
    ```python
    # CONFIG
    SYMBOL = "SOLUSDT"  # Change this to your desired pair
    OUT_DIR = "sol_perp_1m_dataset"
    ```
3.  Run the script!

---

## 📈 Example Output

```text
✅ DOWNLOAD COMPLETE!
📁 File: btc_perp_1m_dataset/BTCUSDT_perp_1m_master.parquet
📊 Total rows: 2,145,000
📅 Date range: 2019-09-01 00:00:00+00:00 to 2024-01-01 00:00:00+00:00
💾 File size: 145.20 MB
```

---

## 🤝 Contributing

Contributions are welcome! Feel free to open issues or pull requests if you have ideas for new features or improvements.

## 📝 License

This project is open source and available for use in your trading and data analysis projects.
