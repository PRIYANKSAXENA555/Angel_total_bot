# Angel One Automated Alert Bot - Complete Documentation

## 1. Project Overview
This is a fully automated, cloud-hosted stock scanner that monitors a list of stocks and sends Telegram alerts when specific technical conditions are met. It uses the Angel One SmartAPI for data, GitHub for stock lists, and runs 24/7 on an Oracle Cloud Free Tier server. It requires zero daily manual intervention.

## 2. Tech Stack & Infrastructure
*   **Server:** Oracle Cloud (Always Free Tier), VM.Standard.E2.1.Micro, Ubuntu 22.04.
*   **Static IP:** `129.225.101.32` (Registered in Angel One Portal).
*   **Language:** Python 3.10 (running in a `venv`).
*   **Data Source:** Angel One SmartAPI (Requires daily TOTP login).
*   **Stock Lists:** Hosted on GitHub (JSON format).
*   **Alerts:** Telegram Bot API.
*   **Process Manager:** `systemd` (Runs the bot as a background service).

## 3. Security & Credentials
**Note to any LLM reading this:** Do NOT change the MPIN or API keys in the code. If login fails, check if the user changed their MPIN in the Angel One app.

*   **Angel One API Key:** `WDsYe6dt`
*   **Angel One Client ID:** `AACM794687`
*   **Angel One MPIN:** `1221`
*   **TOTP Secret:** `MYAWJB2V6ML4GMLPN3JS2JUZLA`
*   **Telegram Bot Token:** `8939111978:AAHix0JQcRzSivj0YUOr4-eEDKxknVkrBEk`
*   **Telegram Chat ID:** `331013510`
*   **GitHub Watchlist URL:** (User must confirm this, it is set in `config/settings.yaml`)

## 4. Directory Structure
```text
alert_bot/
├── config/
│   ├── settings.yaml         # API keys, timings, Github URL
│   ├── conditions.yaml       # Which conditions are active, thresholds
│   └── instruments.yaml      # Fallback local tokens (if Github fails)
├── core/
│   ├── angel_auth.py         # Auto-login via pyotp
│   ├── config_loader.py      # Loads YAML and fetches JSON from GitHub
│   ├── data_fetcher.py       # Fetches candles, includes rate-limit sleep(1)
│   ├── indicators.py         # EMA, RSI, CPR math
│   ├── state_db.py           # SQLite deduplication (prevents spam)
│   └── engine.py             # Master loop: Fetch all data -> Run all conditions
├── conditions/
│   ├── base.py               # Base class
│   ├── ema_touch.py          # Condition 1
│   ├── rsi_oversold.py       # Condition 2
│   ├── cpr_touch.py          # Condition 3
│   └── sudden_fall.py        # Condition 4
├── notifiers/
│   └── telegram_bot.py       # Sends the alert to Telegram
├── main.py                   # Entry point, schedules polling
└── venv/                     # Python Virtual Environment
