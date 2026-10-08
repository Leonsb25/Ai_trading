# Hermes Trading Platform

[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Django](https://img.shields.io/badge/django-4.2-green.svg)](https://www.djangoproject.com/)
[![React](https://img.shields.io/badge/react-18-blue.svg)](https://reactjs.org/)
[![Status](https://img.shields.io/badge/status-active-success.svg)]()

## Overview

**Hermes Trading** is a full-stack trading simulation platform with **real-time market data integration**, portfolio management, **trading signals**, **AI-powered automated trading bots**, and ML-powered trading strategies. Built with Django REST Framework (backend) and React (frontend), Hermes provides a realistic trading experience with automatic price updates, comprehensive performance tracking, intelligent buy/sell signals, and fully automated trading bots powered by machine learning.

Named after the Greek god of commerce and trade, Hermes combines speed, intelligence, and strategic thinking to help you master the art of trading.

---

**Documentation:**
- [Backend Documentation](backend_django/README.md) - API setup & endpoints
- [Frontend Documentation](frontend_react/README.md) - React app & components

---

##  Key Features

###  Core Trading Functionality
- **Real-Time Stock Prices** - Integration with Yahoo Finance API for live market data
- **Automatic Price Updates** - Holdings refresh every 30 seconds with current market prices
- **Interactive Price Charts** - Historical price visualization with multiple time periods (1D, 1W, 1M, 3M, 1Y, 5Y)
- **Smart Buy/Sell Interface** - Auto-fetch current prices before trading
- **Portfolio Management** - Track holdings, calculate P/L, monitor performance
- **Realized P/L Tracking** - Track actual profits/losses from closed positions
- **Hermes Branding** - Professional gold logo and interface design

### 🤖 Hermes AI Trading Bot
- **Automated Trading Bots** - Create AI-powered trading bots with customizable risk profiles
- **Multi-Strategy Integration** - Combines 4 ML strategies:
  - Pivot Point Analysis
  - Next-Day Price Prediction
  - Stock Screener
  - Index Rebalancing Strategy
- **Risk-Based Configuration** - THREE risk profiles:
  - **LOW**: 2%/month return, conservative (blue chips)
  - **MEDIUM**: 5%/month return, balanced (growth stocks)
  - **HIGH**: 10%/month return, aggressive (high volatility)
- **Automatic Position Management** - Stop loss, take profit, and position sizing based on risk level
- **Performance Tracking** - Real-time ROI, win rate, and trade statistics
- **Backtesting System** - Test bot strategies on historical data before live trading
- **Bot Management Dashboard** - Create, monitor, pause, and manage multiple trading bots

###  Trading Signals
- **Index Addition Alerts** - Get notified when stocks enter NASDAQ 100 or S&P 500
- **Action Recommendations** - Buy, Sell, Watch, or Hold signals
- **Unread Badge Notifications** - Visual alerts in navigation menu with count
- **Signal Filtering** - Filter by All, Unread, Buy, Sell, Watch
- **Quick Trade Actions** - Buy directly from signal cards
- **Signal Management** - Mark as read or dismiss signals
- **Auto-Refresh** - Signal count updates every 60 seconds

###  Performance Tracking
- **Historical Performance Charts** - Track portfolio value from purchase date forward
- **Individual Stock Analysis** - View performance of specific stocks
- **Automatic Snapshots** - System saves portfolio state every 30 seconds
- **Multiple Time Periods** - Analyze performance across different timeframes (1D to 5Y)
- **Best/Worst Performers** - Identify top and bottom performing stocks
- **Unrealized vs Realized P/L** - Separate tracking for open and closed positions

###  ML Trading Strategies
- **Pivot Point Analysis** - Technical analysis for support/resistance levels
- **Next-Day Price Prediction** - ML-based price movement forecasting
- **Stock Screener** - Analyze stocks for index addition eligibility
- **Index Rebalancing Analysis** - Track index reconstitution events

###  User Management
- **Custom User Authentication** - Token-based secure authentication
- **Account Balance Tracking** - Real-time balance updates
- **Transaction History** - Complete audit trail of all activities
- **Profile Management** - Update user information and preferences

---

## 🚀 Quick Start

### Prerequisites
- **Python 3.8+**
- **Node.js 16+**
- **npm or yarn**
- **Git**

### Installation

#### 1️⃣ Clone the Repository
```bash
git clone https://github.com/Leonsb25/Ai_trading.git
cd Ai_trading
```

#### 2️⃣ Backend Setup (Django)

```bash
# Create and activate virtual environment
python -m venv venv

# On Windows (Git Bash):
source venv/Scripts/activate

# On Windows (Command Prompt):
venv\Scripts\activate

# On macOS/Linux:
source venv/bin/activate

# Navigate to Django project
cd backend_django/trading_back

# Install dependencies
pip install -r ../requirements.txt

# Run migrations
python manage.py makemigrations
python manage.py migrate

# Create superuser (optional)
python manage.py createsuperuser

# Start development server
python manage.py runserver
```

Backend will run at: **http://127.0.0.1:8000/**

#### 3️⃣ Frontend Setup (React)

Open a **new terminal**:

```bash
cd frontend_react

# Install dependencies
npm install

# Start development server
npm start
```

Frontend will open at: **http://localhost:3000/**

---

##  Testing Features

### Create Test User and Signals

```bash
cd backend_django/trading_back
source ../../venv/Scripts/activate  # Windows Git Bash
python manage.py shell
```

Then:
```python
from trading_app.models import User, Signal
from decimal import Decimal

# Create test user
user = User.objects.create_user(
    username='testuser',
    email='test@example.com',
    password='testpass123',
    name='Test User'
)
user.balance = Decimal('5000.00')
user.save()

# Create test signals
Signal.objects.create(
    user=user,
    stock='TSLA',
    signal_type='index_addition',
    action='buy',
    title='TSLA Added to NASDAQ 100',
    description='Tesla has been added to the NASDAQ 100 index.',
    index_name='NASDAQ 100',
    current_price=Decimal('250.00'),
    is_read=False,
    is_active=True
)

Signal.objects.create(
    user=user,
    stock='NVDA',
    signal_type='index_addition',
    action='watch',
    title='NVDA Added to S&P 500',
    description='NVIDIA has been added to the S&P 500 index.',
    index_name='S&P 500',
    current_price=Decimal('495.00'),
    is_read=False,
    is_active=True
)

print(" Test user and signals created!")
print(f"Login: test@example.com / testpass123")
quit()
```

### Run Hermes Bot Backtest

```bash
cd backend_django/trading_back
source ../../venv/Scripts/activate
python manage.py backtest_hermes --risk-level MEDIUM --investment 1000
```

**Backtest Options:**
- `--bot-id <id>` - Backtest existing bot
- `--risk-level <LOW|MEDIUM|HIGH>` - Risk profile
- `--investment <amount>` - Investment amount in dollars
- `--start-date <YYYY-MM-DD>` - Backtest start date
- `--end-date <YYYY-MM-DD>` - Backtest end date

---

##  API Endpoints

### Base URL
```
http://localhost:8000/api/
```

### Authentication
All protected endpoints require token authentication:
```
Authorization: Token <your_token_here>
```

### User Endpoints
- `POST /api/users/register/` - User registration
- `POST /api/users/login/` - User login (returns token)
- `POST /api/users/logout/` - User logout
- `GET /api/users/profile/` - Get user profile
- `PUT/PATCH /api/users/update_profile/` - Update profile

### Hermes Bot Endpoints
- `POST /api/herm/create/` - Create new trading bot
- `GET /api/herm/list/` - List all user's bots
- `GET /api/herm/<bot_id>/status/` - Get bot performance and status

### Trading Endpoints
- `POST /api/trading/buy/` - Buy stock
- `POST /api/trading/sell/` - Sell stock (tracks realized P/L)
- `POST /api/trading/get_stock_price/` - Fetch real-time stock price with historical data

### Holdings Endpoints
- `GET /api/holdings/` - List all holdings
- `POST /api/holdings/refresh_prices/` - Refresh current prices for all holdings
- `GET /api/holdings/summary/` - Holdings summary

### Portfolio Endpoints
- `GET /api/portfolio/summary/` - Complete portfolio overview (includes realized P/L)
- `GET /api/portfolio/performance/` - Performance metrics

### Signal Endpoints
- `GET /api/signals/` - List all signals
- `GET /api/signals/active/` - Get only active signals
- `GET /api/signals/unread_count/` - Get count of unread signals
- `POST /api/signals/{id}/mark_read/` - Mark signal as read
- `POST /api/signals/{id}/dismiss/` - Dismiss/deactivate signal

### Performance Tracking Endpoints
- `POST /api/portfolio-snapshots/save_snapshot/` - Save current portfolio snapshot
- `GET /api/portfolio-snapshots/portfolio_history/?period=1M` - Get portfolio performance history
- `GET /api/portfolio-snapshots/stock_history/?stock=AAPL&period=1M` - Get individual stock history

### ML Strategy Endpoints
- `POST /api/ml/pivot/` - Pivot point analysis
- `POST /api/ml/predict/` - Next-day price prediction
- `POST /api/ml/screener/` - Stock screener analysis
- `POST /api/ml/index-event/` - Index rebalancing analysis

---

##  How to Use

### 1. Register & Login
- Navigate to **http://localhost:3000**
- Click "Register" to create an account
- Login with your credentials
- You'll receive a starting balance for trading

### 2. Make a Deposit
- Go to **Transactions** page
- Click "Add Transaction"
- Select "Deposit" and enter amount
- Your balance will be updated

### 3. Buy Stocks (Manual Trading)
- Go to **Trading** page
- Enter stock symbol (e.g., "AAPL", "GOOGL", "TSLA")
- Click **"Get Current Price"** to fetch real-time market price
- View historical price chart
- Enter quantity of shares
- Review total cost
- Click "Buy Stock"

### 4. Create AI Trading Bot
- Go to **Hermes Agent** page
- Enter investment amount (minimum $100)
- Select duration (weeks)
- Choose risk level:
  - **LOW**: Conservative, 2%/month target
  - **MEDIUM**: Balanced, 5%/month target
  - **HIGH**: Aggressive, 10%/month target
- Click "Create Herm_trades Bot"
- Bot will execute trades using ML strategies

### 5. Monitor Performance
- **Dashboard** - See 4 key metrics:
  - Total Balance (cash available)
  - Current Value (holdings value)
  - Unrealized P/L (open positions)
  - Realized P/L (profits from closed trades)
- **Holdings** page - View all your stocks (auto-refreshes every 30s)
- **Performance** page - Interactive charts showing portfolio history
- **Hermes Agent** page - Monitor bot performance, ROI, win rate

### 6. Trading Signals
- Check **Signals** menu for red badge (unread count)
- View signals: Index additions, buy/sell recommendations
- Filter by: All, Unread, Buy, Sell, Watch
- Click **"Buy Now"** to trade directly from signals
- Mark as read or dismiss signals

### 7. Sell Stocks
- Go to **Trading** → **Sell** tab
- Select stock from your holdings
- System shows current price
- Enter quantity to sell
- Realized P/L is calculated and added to your total

---

##  Hermes Bot Risk Profiles

### LOW Risk (Conservative)
- **Expected Return**: 2% per month
- **Stop Loss**: 5%
- **Take Profit**: 10%
- **Max Position**: 20% of capital
- **Stocks**: Large-cap blue chips (AAPL, MSFT, GOOGL, AMZN, TSLA, META, NVDA, JPM, V, JNJ)
- **Best For**: Conservative investors seeking steady growth

### MEDIUM Risk (Balanced)
- **Expected Return**: 5% per month
- **Stop Loss**: 8%
- **Take Profit**: 15%
- **Max Position**: 30% of capital
- **Stocks**: Growth stocks (AAPL, MSFT, GOOGL, AMZN, TSLA, META, NVDA, AMD, NFLX, DIS)
- **Best For**: Balanced investors seeking moderate returns

### HIGH Risk (Aggressive)
- **Expected Return**: 10% per month
- **Stop Loss**: 15%
- **Take Profit**: 25%
- **Max Position**: 40% of capital
- **Stocks**: High volatility stocks (TSLA, NVDA, AMD, META, NFLX, PLTR, RIVN, LCID, SOFI, HOOD)
- **Best For**: Aggressive traders comfortable with higher risk

---

## 🗂️ Project Structure

```
Hermes_Trading/
├── backend_django/
│   └── trading_back/
│       ├── trading_app/
│       │   ├── models.py              # Database models
│       │   ├── views.py               # API endpoints
│       │   ├── serializers.py         # Data serialization
│       │   ├── urls.py                # API routing
│       │   ├── herm_trades.py         # Hermes bot API
│       │   ├── auto_trading_engine.py # Trading engine
│       │   ├── backtest_hermes_bot.py # Backtesting system
│       │   ├── ml_models/             # ML strategies
│       │   │   ├── pivot.py
│       │   │   ├── nextday_prediction.py
│       │   │   ├── stock_screener.py
│       │   │   └── index_rebalancing.py
│       │   └── management/commands/
│       │       └── backtest_hermes.py # Backtest CLI
│       ├── manage.py
│       └── db.sqlite3
├── frontend_react/
│   ├── public/
│   │   └── hermes_logo.png           # Hermes branding
│   ├── src/
│   │   ├── components/
│   │   │   └── Layout.jsx            # Main layout with logo & badges
│   │   ├── pages/
│   │   │   ├── Dashboard.jsx         # Portfolio overview
│   │   │   ├── Trading.jsx           # Manual trading
│   │   │   ├── HermesAgent.jsx       # Bot management
│   │   │   ├── Signals.jsx           # Trading signals
│   │   │   ├── Holdings.jsx          # Current positions
│   │   │   └── Performance.jsx       # Performance charts
│   │   └── services/
│   │       └── api.js                # API client
│   └── package.json
├── venv/                              # Python virtual environment
├── .gitignore
└── README.md
```

---

##  Database Models

### User Model
- Custom user with email authentication
- Fields: balance, realized_profit_loss, name, email, userid

### Transaction Model
- Types: deposit, withdrawal, buy, sell, dividend, fee
- Tracks all financial activities with timestamps

### Holding Model
- Tracks current stock positions
- Auto-calculates P/L, percentages, current value

### Signal Model
- Trading signals with action recommendations
- Types: index_addition, index_removal, price_target, volume_spike
- Status: is_read, is_active

### AutoTradingBot Model
- Tracks automated trading bots
- Fields:
  - Configuration: risk_level, duration, initial_capital
  - Performance: current_capital, total_profit_loss, roi_percentage
  - Statistics: total_trades, winning_trades, losing_trades, win_rate
  - Status: ACTIVE, PAUSED, STOPPED, COMPLETED
  - ML flags: use_pivot, use_prediction, use_screener, use_index_rebalancing

### PortfolioSnapshot & StockSnapshot Models
- Historical performance tracking
- Automatic snapshots every 30 seconds

---

##  Configuration

### Environment Variables
For production deployment, configure:
- `DEBUG=False`
- `SECRET_KEY` (generate a new secure key)
- `ALLOWED_HOSTS` (add your domain)
- Database configuration (PostgreSQL recommended)

### CORS Settings
Backend accepts requests from `http://localhost:3000` by default.

To modify, edit `backend_django/trading_back/trading_back/settings.py`:
```python
CORS_ALLOWED_ORIGINS = [
    "http://localhost:3000",
    "http://your-domain.com",
]
```

---

## 🧪 Testing

### Backend Tests
```bash
cd backend_django/trading_back
python manage.py test
```

### Run Backtest
```bash
python manage.py backtest_hermes --risk-level MEDIUM --investment 1000
```

### Create Test Data
Use Django shell to create test signals, bots, or transactions for testing.

---

## 🛠️ Technologies

### Backend:
- **Django 4.2** - Web framework
- **Django REST Framework 3.14** - API framework
- **yfinance** - Real-time stock data
- **pandas** - Data analysis
- **numpy** - Numerical computing
- **scikit-learn** - Machine learning
- **xgboost** - ML gradient boosting
- **SQLite** - Database (development)

### Frontend:
- **React 18** - UI library
- **Material-UI (MUI)** - Component library
- **Recharts** - Charting library
- **React Router** - Routing
- **Axios** - HTTP client
- **React Toastify** - Notifications

---

## 🤝 Contributing

We welcome contributions! Here's how:

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Commit with clear messages (`git commit -m 'Add amazing feature'`)
5. Push to your branch (`git push origin feature/amazing-feature`)
6. Open a Pull Request

### Development Guidelines:
- Follow PEP 8 style guide for Python code
- Use ESLint configuration for JavaScript/React code
- Write tests for new features
- Update documentation for API changes
- Keep commits atomic and well-described

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🆘 Support & Documentation

### Resources:
- **GitHub Repository**: https://github.com/Leonsb25/Ai_trading/
- **API Documentation**: Check `/api/` endpoint when server is running
- **Django Documentation**: https://docs.djangoproject.com/
- **React Documentation**: https://reactjs.org/

### Common Issues:

**Port already in use?**
```bash
# Backend
python manage.py runserver 8001

# Frontend
PORT=3001 npm start
```

**CORS errors?**
- Verify `django-cors-headers` is installed
- Check `CORS_ALLOWED_ORIGINS` in settings.py

**Database errors?**
```bash
# Reset database
rm db.sqlite3
python manage.py migrate
```


---

## 🙏 Acknowledgments

- Yahoo Finance for providing free stock market data via yfinance
- Material-UI for the beautiful React components
- Recharts for the interactive charting library
- Django and React communities for excellent documentation
- Greek mythology for the inspiration (Hermes, god of commerce and trade)

---

## 📞 Contact

For questions or issues:
- **GitHub Issues:** https://github.com/Leonsb25/Ai_trading/issues

---

**Built with ❤️ - Hermes: Swift, Intelligent, Strategic Trading**

*Disclaimer: This is a simulation platform for educational purposes. Not financial advice. Always consult with a licensed financial advisor before making real investment decisions.*
