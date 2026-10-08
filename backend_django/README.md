# Hermes Trading Platform - Backend API

[![Python](https://img.shields.io/badge/python-3.8+-blue.svg)](https://www.python.org/downloads/)
[![Django](https://img.shields.io/badge/django-4.2-green.svg)](https://www.djangoproject.com/)
[![Django REST Framework](https://img.shields.io/badge/DRF-3.14+-orange.svg)](https://www.django-rest-framework.org/)

## 📊 Overview

Django REST API backend for the Hermes Trading Platform. Provides user management, portfolio tracking, trading functionality, AI-powered bot system, ML strategies, and real-time stock data integration via Yahoo Finance.

> **📚 For full project documentation, see the [Main README](../README.md)**

---

## 🚀 Quick Setup

### Prerequisites
- **Python 3.8+**
- **pip** (Python package installer)

### Installation

1. **Navigate to backend directory**
   ```bash
   cd backend_django
   ```

2. **Create and activate virtual environment**
   ```bash
   # Create virtual environment
   python -m venv venv
   
   # Activate (Windows Git Bash)
   source venv/Scripts/activate
   
   # Activate (Windows Command Prompt)
   venv\Scripts\activate
   
   # Activate (macOS/Linux)
   source venv/bin/activate
   ```

3. **Install dependencies**
   ```bash
   pip install -r requirements.txt
   ```

4. **Navigate to Django project**
   ```bash
   cd trading_back
   ```

5. **Run migrations**
   ```bash
   python manage.py makemigrations
   python manage.py migrate
   ```

6. **Create superuser (optional)**
   ```bash
   python manage.py createsuperuser
   ```

7. **Start development server**
   ```bash
   python manage.py runserver
   ```

The API will be available at: **http://127.0.0.1:8000/api/**

---

## 🏗️ Project Structure

```
backend_django/
├── requirements.txt              # Python dependencies
├── venv/                         # Virtual environment (ignored by git)
└── trading_back/                 # Django project
    ├── manage.py                 # Django management script
    ├── db.sqlite3                # SQLite database
    ├── trading_back/             # Project configuration
    │   ├── settings.py           # Django settings
    │   ├── urls.py               # Root URL routing
    │   ├── wsgi.py               # WSGI config
    │   └── asgi.py               # ASGI config
    └── trading_app/              # Main application
        ├── models.py             # Database models
        ├── views.py              # API endpoints
        ├── serializers.py        # Data serialization
        ├── urls.py               # App URL routing
        ├── herm_trades.py        # Hermes bot API
        ├── auto_trading_engine.py # Risk configuration
        ├── backtest_hermes_bot.py # Backtesting engine
        ├── ml_models/            # ML strategies
        │   ├── pivot.py
        │   ├── nextday_prediction.py
        │   ├── stock_screener.py
        │   └── index_rebalancing.py
        ├── management/
        │   └── commands/
        │       └── backtest_hermes.py
        ├── migrations/           # Database migrations
        └── tests.py              # Unit tests
```

---

## 📚 Database Models

### **User Model**
Custom user model with email authentication.

**Fields:**
- `email` - Primary authentication (EmailField, unique)
- `name` - Full name (CharField)
- `userid` - Unique user ID (CharField, unique)
- `balance` - Available cash balance (DecimalField)
- `realized_profit_loss` - Total realized P/L from closed positions (DecimalField)

**Authentication:** Token-based

---

### **Transaction Model**
Tracks all financial activities.

**Fields:**
- `user` - ForeignKey to User
- `transaction_type` - deposit, withdrawal, buy, sell, dividend, fee
- `stock` - Stock symbol (nullable for deposits/withdrawals)
- `quantity` - Number of shares (nullable)
- `price` - Price per share (nullable)
- `amount` - Total transaction amount
- `timestamp` - Auto-created timestamp
- `description` - Optional notes

---

### **Holding Model**
Current stock positions.

**Fields:**
- `user` - ForeignKey to User
- `stock` - Stock symbol (CharField)
- `quantity` - Number of shares owned
- `buying_price` - Average purchase price
- `current_price` - Latest market price
- `total_invested` - Total amount invested
- `current_value` - Current market value (computed)
- `profit_loss` - Unrealized P/L (computed)
- `profit_loss_percentage` - P/L percentage (computed)

**Unique Constraint:** One holding per user-stock pair

---

### **AutoTradingBot Model**
AI-powered trading bots.

**Fields:**
- `user` - ForeignKey to User
- `name` - Bot name (CharField)
- `risk_level` - LOW, MEDIUM, HIGH (CharField)
- `duration` - Trading duration (CharField)
- `initial_capital` - Starting capital (DecimalField)
- `current_capital` - Current capital (DecimalField)
- `expected_return` - Target monthly return % (DecimalField)
- `total_trades` - Number of trades executed (IntegerField)
- `winning_trades` - Number of profitable trades (IntegerField)
- `losing_trades` - Number of losing trades (IntegerField)
- `total_profit_loss` - Total P/L (DecimalField)
- `status` - ACTIVE, PAUSED, STOPPED, COMPLETED (CharField)
- `use_pivot` - Enable pivot analysis (BooleanField)
- `use_prediction` - Enable price prediction (BooleanField)
- `use_screener` - Enable stock screener (BooleanField)
- `use_index_rebalancing` - Enable index rebalancing (BooleanField)
- `created_at` - Creation timestamp
- `updated_at` - Last update timestamp

**Computed Properties:**
- `roi_percentage` - ROI as percentage
- `win_rate` - Winning trades percentage

---

### **Signal Model**
Trading signals and alerts.

**Fields:**
- `user` - ForeignKey to User
- `stock` - Stock symbol
- `signal_type` - index_addition, index_removal, price_target, volume_spike
- `action` - buy, sell, hold, watch
- `title` - Signal title (CharField)
- `description` - Detailed description (TextField)
- `index_name` - Index name for index_addition signals (nullable)
- `current_price` - Stock price at signal creation (DecimalField)
- `is_read` - Read status (BooleanField)
- `is_active` - Active status (BooleanField)
- `created_at` - Creation timestamp

---

### **PortfolioSnapshot Model**
Historical portfolio tracking.

**Fields:**
- `user` - ForeignKey to User
- `timestamp` - Snapshot time
- `total_value` - Total portfolio value
- `total_invested` - Total amount invested
- `total_profit_loss` - Total P/L

**Purpose:** Powers performance charts

---

### **StockSnapshot Model**
Individual stock performance history.

**Fields:**
- `portfolio_snapshot` - ForeignKey to PortfolioSnapshot
- `stock` - Stock symbol
- `quantity` - Number of shares
- `buying_price` - Purchase price
- `current_price` - Price at snapshot
- `current_value` - Value at snapshot
- `profit_loss` - P/L at snapshot
- `profit_loss_percentage` - P/L % at snapshot

**Purpose:** Stock-specific performance tracking

---

## 🔌 API Endpoints

### **Authentication**
All protected endpoints require token authentication:
```
Authorization: Token <your_token_here>
```

### **Base URL**
```
http://localhost:8000/api/
```

---

### **User Endpoints**

**Register User**
```http
POST /api/users/register/
Content-Type: application/json

{
  "username": "testuser",
  "email": "test@example.com",
  "password": "securepassword",
  "name": "Test User"
}
```

**Login**
```http
POST /api/users/login/
Content-Type: application/json

{
  "email": "test@example.com",
  "password": "securepassword"
}

Response: { "token": "abc123...", "user": {...} }
```

**Get Profile**
```http
GET /api/users/profile/
Authorization: Token <token>
```

**Update Profile**
```http
PUT /api/users/update_profile/
Authorization: Token <token>
Content-Type: application/json

{
  "name": "Updated Name"
}
```

**Logout**
```http
POST /api/users/logout/
Authorization: Token <token>
```

---

### **Trading Endpoints**

**Buy Stock**
```http
POST /api/trading/buy/
Authorization: Token <token>
Content-Type: application/json

{
  "stock": "AAPL",
  "quantity": 10,
  "price": 150.00
}
```

**Sell Stock**
```http
POST /api/trading/sell/
Authorization: Token <token>
Content-Type: application/json

{
  "stock": "AAPL",
  "quantity": 5,
  "price": 155.00
}
```

**Get Real-Time Stock Price**
```http
POST /api/trading/get_stock_price/
Authorization: Token <token>
Content-Type: application/json

{
  "stock": "AAPL",
  "period": "1M"  // Optional: 1D, 1W, 1M, 3M, 1Y, 5Y
}

Response: {
  "symbol": "AAPL",
  "current_price": 150.25,
  "price_change": 2.50,
  "price_change_percent": 1.69,
  "historical_data": [...]
}
```

---

### **Holdings Endpoints**

**List Holdings**
```http
GET /api/holdings/
Authorization: Token <token>
```

**Get Holdings Summary**
```http
GET /api/holdings/summary/
Authorization: Token <token>

Response: {
  "total_holdings": 5,
  "total_invested": 8000.00,
  "total_current_value": 8500.00,
  "total_profit_loss": 500.00
}
```

**Refresh All Prices**
```http
POST /api/holdings/refresh_prices/
Authorization: Token <token>

Response: {
  "message": "Prices refreshed for 5 holdings"
}
```

---

### **Portfolio Endpoints**

**Portfolio Summary**
```http
GET /api/portfolio/summary/
Authorization: Token <token>

Response: {
  "total_balance": 5000.00,
  "total_invested": 8000.00,
  "total_holdings_value": 8500.00,
  "total_portfolio_value": 13500.00,
  "unrealized_profit_loss": 500.00,
  "realized_profit_loss": 1200.00
}
```

**Performance Metrics**
```http
GET /api/portfolio/performance/
Authorization: Token <token>

Response: {
  "total_return": 1700.00,
  "best_performer": {...},
  "worst_performer": {...}
}
```

**Save Portfolio Snapshot**
```http
POST /api/portfolio-snapshots/save_snapshot/
Authorization: Token <token>

Response: {
  "message": "Snapshot saved",
  "snapshot_id": 123
}
```

**Get Portfolio History**
```http
GET /api/portfolio-snapshots/portfolio_history/?period=1M
Authorization: Token <token>

Response: {
  "period": "1M",
  "count": 50,
  "data": [
    {
      "date": "2025-12-01T00:00:00Z",
      "total_value": 10000.00,
      "cash_balance": 5000.00,
      "holdings_value": 5000.00
    },
    ...
  ]
}
```

**Get Stock History**
```http
GET /api/portfolio-snapshots/stock_history/?stock=AAPL&period=1M
Authorization: Token <token>
```

---

### **Hermes Bot Endpoints**

**Create Trading Bot**
```http
POST /api/herm/create/
Authorization: Token <token>
Content-Type: application/json

{
  "investment_amount": 1000,
  "risk_level": "MEDIUM",
  "duration_weeks": 4
}

Response: {
  "id": 1,
  "name": "Herm_MEDIUM_4w",
  "status": "ACTIVE",
  "initial_capital": 1000.00,
  "risk_level": "MEDIUM"
}
```

**List User's Bots**
```http
GET /api/herm/list/
Authorization: Token <token>

Response: [
  {
    "id": 1,
    "name": "Herm_MEDIUM_4w",
    "risk_level": "MEDIUM",
    "status": "ACTIVE",
    "total_trades": 15,
    "roi_percentage": 5.2,
    "win_rate": 73.3
  }
]
```

**Get Bot Status**
```http
GET /api/herm/1/status/
Authorization: Token <token>

Response: {
  "id": 1,
  "name": "Herm_MEDIUM_4w",
  "initial_capital": 1000.00,
  "current_capital": 1052.00,
  "total_profit_loss": 52.00,
  "roi_percentage": 5.2,
  "total_trades": 15,
  "winning_trades": 11,
  "losing_trades": 4,
  "win_rate": 73.3
}
```

---

### **Signal Endpoints**

**List All Signals**
```http
GET /api/signals/
Authorization: Token <token>
```

**Get Active Signals**
```http
GET /api/signals/active/
Authorization: Token <token>
```

**Get Unread Count**
```http
GET /api/signals/unread_count/
Authorization: Token <token>

Response: {
  "unread_count": 3
}
```

**Mark Signal as Read**
```http
POST /api/signals/1/mark_read/
Authorization: Token <token>

Response: {
  "message": "Signal marked as read"
}
```

**Dismiss Signal**
```http
POST /api/signals/1/dismiss/
Authorization: Token <token>

Response: {
  "message": "Signal dismissed"
}
```

---

### **ML Strategy Endpoints**

**Pivot Point Analysis**
```http
POST /api/ml/pivot/
Authorization: Token <token>
Content-Type: application/json

{
  "stock": "AAPL"
}

Response: {
  "stock": "AAPL",
  "pivot_point": 150.00,
  "support_1": 148.00,
  "resistance_1": 152.00,
  "recommendation": "buy"
}
```

**Next-Day Prediction**
```http
POST /api/ml/predict/
Authorization: Token <token>
Content-Type: application/json

{
  "stock": "AAPL"
}
```

**Stock Screener**
```http
POST /api/ml/screener/
Authorization: Token <token>
Content-Type: application/json

{
  "stock": "AAPL"
}
```

**Index Rebalancing**
```http
POST /api/ml/index-event/
Authorization: Token <token>
Content-Type: application/json

{
  "stock": "AAPL"
}
```

---

## 🤖 Hermes Bot Risk Configuration

### **LOW Risk (Conservative)**
- Expected Monthly Return: **2%**
- Stop Loss: **5%**
- Take Profit: **10%**
- Max Position Size: **20%** of capital
- **Watchlist:** AAPL, MSFT, GOOGL, AMZN, TSLA, META, NVDA, JPM, V, JNJ

### **MEDIUM Risk (Balanced)**
- Expected Monthly Return: **5%**
- Stop Loss: **8%**
- Take Profit: **15%**
- Max Position Size: **30%** of capital
- **Watchlist:** AAPL, MSFT, GOOGL, AMZN, TSLA, META, NVDA, AMD, NFLX, DIS

### **HIGH Risk (Aggressive)**
- Expected Monthly Return: **10%**
- Stop Loss: **15%**
- Take Profit: **25%**
- Max Position Size: **40%** of capital
- **Watchlist:** TSLA, NVDA, AMD, META, NFLX, PLTR, RIVN, LCID, SOFI, HOOD

---

## 🧪 Backtesting

### **Run Backtest**

```bash
cd trading_back
python manage.py backtest_hermes
```

### **Backtest Options**

```bash
# Specific risk level and investment
python manage.py backtest_hermes --risk-level MEDIUM --investment 1000

# Backtest existing bot
python manage.py backtest_hermes --bot-id 1

# Custom date range
python manage.py backtest_hermes --start-date 2025-01-01 --end-date 2025-01-31
```

### **Programmatic Usage**

```python
from trading_app.backtest_hermes_bot import run_backtest_for_bot

results = run_backtest_for_bot(
    bot_id=None,
    risk_level='MEDIUM',
    investment_amount=1000
)

print(f"Total Trades: {results['total_trades']}")
print(f"Win Rate: {results['win_rate']}")
print(f"ROI: {results['roi']}")
```

---

## 🧪 Testing

### **Run Tests**
```bash
cd trading_back
python manage.py test
```

### **Run Specific Test**
```bash
python manage.py test trading_app.tests.TestUserModel
```

### **Test Coverage**
```bash
pip install coverage
coverage run manage.py test
coverage report
```

---

## 🔧 Configuration

### **Environment Variables**

For production, create a `.env` file:

```env
DEBUG=False
SECRET_KEY=your-secret-key-here
ALLOWED_HOSTS=yourdomain.com,www.yourdomain.com
DATABASE_URL=postgresql://user:pass@localhost/dbname
```

### **CORS Settings**

**File:** `trading_back/settings.py`

```python
CORS_ALLOWED_ORIGINS = [
    "http://localhost:3000",
    "http://yourdomain.com",
]
```

### **Database Configuration**

**Development (SQLite):**
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}
```

**Production (PostgreSQL):**
```python
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'hermes_trading',
        'USER': 'postgres',
        'PASSWORD': 'password',
        'HOST': 'localhost',
        'PORT': '5432',
    }
}
```

---

## 📦 Dependencies

**Key Packages:**
- `Django==4.2.0` - Web framework
- `djangorestframework==3.14.0` - API framework
- `django-cors-headers==4.0.0` - CORS support
- `yfinance==0.2.66` - Stock data
- `pandas==2.0.0` - Data analysis
- `numpy==1.24.0` - Numerical computing
- `scikit-learn==1.2.2` - Machine learning
- `xgboost==1.7.5` - ML gradient boosting

**Full list:** See `requirements.txt`

---

## 🛠️ Admin Interface

Django admin is available at: **http://localhost:8000/admin/**

**Create superuser:**
```bash
python manage.py createsuperuser
```

**Accessible models:**
- Users
- Transactions
- Holdings
- AutoTradingBots
- Signals
- Portfolio Snapshots

---

## 🐛 Troubleshooting

### **Port already in use**
```bash
python manage.py runserver 8001
```

### **Migration issues**
```bash
# Delete migrations and database
rm db.sqlite3
rm -rf trading_app/migrations/

# Recreate
python manage.py makemigrations trading_app
python manage.py migrate
```

### **Package conflicts**
```bash
pip install --upgrade pip
pip install -r requirements.txt --force-reinstall
```

### **CORS errors**
- Verify `django-cors-headers` is installed
- Check `CORS_ALLOWED_ORIGINS` in `settings.py`
- Ensure middleware is properly configured

---

## 📚 Additional Resources

- **Main README:** [../README.md](../README.md)
- **Frontend README:** [../frontend_react/README.md](../frontend_react/README.md)
- **Django Docs:** https://docs.djangoproject.com/
- **DRF Docs:** https://www.django-rest-framework.org/

---

**For full project documentation and user guides, see the [Main README](../README.md).**
