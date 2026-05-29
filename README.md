# 🛒 E-commerce Price Tracker - Automated Price Monitoring System

> A sophisticated web scraping and automation pipeline that monitors product prices across multiple e-commerce platforms, sends real-time alerts on price drops, and generates comprehensive analytics reports.

[![Python 3.8+](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

## 📋 Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [System Architecture](#system-architecture)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Examples](#examples)
- [Project Structure](#project-structure)
- [Technologies Used](#technologies-used)
- [Automation & Scheduling](#automation--scheduling)
- [Data Outputs](#data-outputs)
- [Future Enhancements](#future-enhancements)

## 🎯 Overview

This price tracking system automates the process of monitoring product prices across Amazon and Best Buy, providing:

- **Real-time price monitoring** with configurable intervals
- **Intelligent price drop detection** with customizable thresholds
- **Email notifications** with beautiful HTML formatting
- **Historical price tracking** with SQLite database
- **Visual analytics** with interactive charts
- **Data export** in multiple formats (CSV, JSON)
- **Automated scheduling** for hands-free operation

Perfect for savvy shoppers, deal hunters, retailers monitoring competition, and anyone interested in e-commerce data analytics.

## ✨ Key Features

### 🔍 Multi-Platform Web Scraping
- Extensible scraper architecture supporting multiple e-commerce sites
- Currently supports Amazon and Best Buy with easy expansion
- Robust error handling and retry logic
- Respectful scraping with rate limiting and random delays
- Rotating user agents to avoid detection

### 📊 Advanced Price Analytics
- Historical price tracking with timestamp accuracy
- Price trend visualization with matplotlib/seaborn
- Comparative price analysis across retailers
- Target price monitoring
- Availability tracking

### 🔔 Smart Notifications
- Real-time email alerts for significant price drops
- Beautiful HTML-formatted emails
- Configurable alert thresholds (percentage and absolute amount)
- Daily/weekly summary reports
- Optional Telegram integration (extensible)

### 💾 Robust Data Management
- SQLite database for efficient storage
- Automated data exports (CSV, JSON)
- Price history queries with pandas
- Data integrity and validation

### ⚙️ Automation & Scheduling
- Configurable scraping intervals
- Background task scheduling
- Automated report generation
- Cron-compatible for server deployment

## 🏗️ System Architecture

```
┌─────────────────┐
│  Scheduler      │ ──> Runs tracking jobs at intervals
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│ Price Tracker   │ ──> Main orchestration layer
└────────┬────────┘
         │
    ┌────┴────┬────────────┬──────────────┐
    │         │            │              │
    ▼         ▼            ▼              ▼
┌────────┐ ┌──────┐ ┌──────────┐ ┌──────────────┐
│Scrapers│ │ DB   │ │Notifier  │ │Visualization │
└────────┘ └──────┘ └──────────┘ └──────────────┘
    │         │          │              │
    ▼         ▼          ▼              ▼
 [Websites] [SQLite] [Email]      [Charts/PNG]
```

## 🚀 Installation

### Prerequisites
- Python 3.8 or higher
- pip package manager
- Gmail account (for email notifications)

### Step 1: Clone the Repository
```bash
git clone https://github.com/yourusername/web-scraping-automation-pipeline.git
cd web-scraping-automation-pipeline
```

### Step 2: Install Dependencies
```bash
pip install -r requirements.txt
```

### Step 3: Configure Environment
```bash
cp .env.example .env
```

Edit `.env` file with your settings:
```env
EMAIL_SENDER=your_email@gmail.com
EMAIL_PASSWORD=your_app_password
EMAIL_RECEIVER=recipient@email.com
SCRAPE_INTERVAL_HOURS=12
PRICE_DROP_PERCENTAGE=5
PRICE_DROP_AMOUNT=10
```

**Important:** For Gmail, you need to use an [App Password](https://support.google.com/accounts/answer/185833), not your regular password.

## ⚙️ Configuration

### Adding Products to Track

Edit `config.py` to add products you want to monitor:

```python
PRODUCTS_TO_TRACK = [
    {
        'name': 'Product Name',
        'urls': {
            'amazon': 'https://www.amazon.com/dp/PRODUCT_ID',
            'bestbuy': 'https://www.bestbuy.com/site/product/SKU.p'
        },
        'target_price': 299.99  # Optional: get notified when price reaches this
    },
    # Add more products...
]
```

### Configurable Parameters

| Parameter | Description | Default |
|-----------|-------------|---------|
| `SCRAPE_INTERVAL_HOURS` | How often to check prices | 12 hours |
| `MAX_RETRIES` | Retry attempts for failed requests | 3 |
| `REQUEST_TIMEOUT` | Request timeout in seconds | 30 |
| `PRICE_DROP_PERCENTAGE` | Minimum % drop to trigger alert | 5% |
| `PRICE_DROP_AMOUNT` | Minimum $ drop to trigger alert | $10 |

## 📖 Usage

### Command Line Interface

The system provides a comprehensive CLI for all operations:

```bash
# Track all configured products
python price_tracker.py --track

# Generate price charts and reports
python price_tracker.py --report

# Export data to CSV
python price_tracker.py --export csv

# Export data to JSON
python price_tracker.py --export json

# View current price summary
python price_tracker.py --summary

# Combine multiple actions
python price_tracker.py --track --report --export csv
```

### Track Specific Product

```bash
python price_tracker.py --track --product "Sony WH-1000XM5 Headphones"
```

### Generate Reports for Specific Timeframe

```bash
python price_tracker.py --report --days 60
```

### Automated Scheduling

Run the scheduler for continuous monitoring:

```bash
python scheduler.py
```

This will:
- Run price checks every N hours (configured in `.env`)
- Send email alerts on price drops
- Generate weekly summary reports
- Log all activities

## 💡 Examples

### Example 1: One-Time Price Check

```bash
python price_tracker.py --track --summary
```

Output:
```
================================================================================
                          PRICE TRACKER SUMMARY
================================================================================

📦 Sony WH-1000XM5 Headphones
--------------------------------------------------------------------------------
  Amazon          $ 349.99  (updated: 2025-01-15 14:30:22)
  Best Buy        $ 369.99  (updated: 2025-01-15 14:32:15)
```

### Example 2: Full Analytics Pipeline

```bash
python price_tracker.py --track --report --export json
```

This will:
1. Scrape current prices from all sites
2. Store in database
3. Check for price drops
4. Send email alerts if drops detected
5. Generate trend charts
6. Export data to JSON

### Example 3: Generate Demo Data for Portfolio

```bash
python demo.py
```

Creates:
- 30 days of realistic price history
- Multiple product comparisons
- Sample charts and visualizations
- CSV/JSON exports

Perfect for showcasing your work!

## 📁 Project Structure

```
web-scraping-automation-pipeline/
│
├── config.py                      # Configuration settings
├── price_tracker.py               # Main application
├── scheduler.py                   # Automated scheduling
├── demo.py                        # Demo data generator
├── requirements.txt               # Python dependencies
├── .env.example                   # Environment template
├── .gitignore                     # Git ignore rules
│
├── src/
│   ├── scrapers/
│   │   ├── base_scraper.py       # Abstract scraper base class
│   │   ├── amazon_scraper.py     # Amazon-specific scraper
│   │   ├── bestbuy_scraper.py    # Best Buy scraper
│   │   └── scraper_factory.py    # Scraper factory pattern
│   │
│   ├── database/
│   │   └── db_manager.py         # SQLite database management
│   │
│   ├── notifications/
│   │   └── email_notifier.py     # Email notification system
│   │
│   └── visualization/
│       └── price_charts.py       # Chart generation
│
├── data/
│   ├── price_tracker.db          # SQLite database (generated)
│   └── outputs/                  # Exported data and charts
│
└── logs/
    ├── scraper.log               # Scraping activity logs
    ├── price_tracker.log         # Application logs
    └── scheduler.log             # Scheduler logs
```

## 🛠️ Technologies Used

### Core Technologies
- **Python 3.8+** - Main programming language
- **BeautifulSoup4** - HTML parsing and scraping
- **Requests** - HTTP requests
- **Selenium** - Dynamic content (if needed)

### Data & Storage
- **SQLite3** - Lightweight database
- **Pandas** - Data manipulation and analysis
- **JSON/CSV** - Data export formats

### Visualization
- **Matplotlib** - Chart generation
- **Seaborn** - Statistical visualization

### Automation
- **Schedule** - Task scheduling
- **python-dotenv** - Environment management

### Others
- **fake-useragent** - User agent rotation
- **smtplib** - Email sending
- **logging** - Activity logging

## 🤖 Automation & Scheduling

### Running as a Background Service

#### Linux/Mac (using cron)

```bash
# Edit crontab
crontab -e

# Add this line to run every 12 hours
0 */12 * * * cd /path/to/project && /usr/bin/python3 price_tracker.py --track
```

#### Using the Built-in Scheduler

```bash
# Run in background (Linux/Mac)
nohup python scheduler.py > /dev/null 2>&1 &

# Or use screen/tmux for persistent sessions
screen -S price_tracker
python scheduler.py
# Ctrl+A, D to detach
```

#### Docker Deployment (Optional)

Create a `Dockerfile`:
```dockerfile
FROM python:3.9-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install -r requirements.txt

COPY . .

CMD ["python", "scheduler.py"]
```

Run:
```bash
docker build -t price-tracker .
docker run -d --name price-tracker price-tracker
```

## 📊 Data Outputs

### Database Schema

**products** table:
- `id` - Product ID
- `name` - Product name
- `created_at` - First tracked timestamp

**price_history** table:
- `id` - Record ID
- `product_id` - Foreign key to products
- `site` - Retailer name
- `price` - Price at time of scraping
- `title` - Product title from site
- `url` - Product URL
- `available` - Availability status
- `timestamp` - Scrape timestamp

### CSV Export Example

```csv
product_name,site,price,title,url,available,timestamp
Sony WH-1000XM5,Amazon,349.99,Sony WH-1000XM5 Wireless...,https://...,True,2025-01-15 14:30:22
Sony WH-1000XM5,Best Buy,369.99,Sony - WH1000XM5 Wireless...,https://...,True,2025-01-15 14:32:15
```

### Chart Types Generated

1. **Price History Chart** - Line chart showing price trends over time
2. **Price Comparison Chart** - Bar chart comparing current prices across sites
3. **Savings Tracker** - Area chart highlighting best prices and target prices

## 🎨 Email Alert Preview

Price drop alerts are sent as beautiful HTML emails featuring:
- Eye-catching gradient header
- Old vs New price comparison
- Savings amount and percentage
- Professional formatting
- Mobile-responsive design

## 🚧 Future Enhancements

### Planned Features
- [ ] Web dashboard with Flask/Django
- [ ] More e-commerce sites (Walmart, Target, eBay)
- [ ] Browser extension integration
- [ ] Machine learning price prediction
- [ ] Telegram bot notifications
- [ ] Price history API
- [ ] Docker compose setup
- [ ] Cloud deployment guides (AWS, Heroku)
- [ ] React frontend dashboard
- [ ] Mobile app integration

### Extensibility

This project is designed to be easily extended:

**Add a new scraper:**
1. Create new class inheriting from `BaseScraper`
2. Implement `extract_price()` and `extract_title()`
3. Add to `ScraperFactory`

**Add new notification channel:**
1. Create new notifier class in `src/notifications/`
2. Implement send methods
3. Integrate in `PriceTracker`

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👨‍💻 Author

**Md Shamir Raza Quraishi**
- GitHub: https://github.com/Md-Shamir-raza


## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## ⭐ Show Your Support

If this project helped you land a client or inspired your work, please give it a star! ⭐

## 📧 Contact

For questions or collaboration opportunities:
- Email: shamirraza559@gmail.com
- LinkedIn: www.linkedin.com/in/md-shamir-raza-quraishi

---


