# Hermes Trading Platform - Frontend

[![React](https://img.shields.io/badge/react-18-blue.svg)](https://reactjs.org/)
[![Material-UI](https://img.shields.io/badge/MUI-5-blue.svg)](https://mui.com/)
[![Recharts](https://img.shields.io/badge/recharts-2.5-blue.svg)](https://recharts.org/)

## 📊 Overview

React-based frontend for the Hermes Trading Platform. Features a modern dark theme with gold accents, real-time stock prices, interactive performance charts, trading signals with badge notifications, AI bot management dashboard, and comprehensive portfolio tracking.

> **📚 For full project documentation, see the [Main README](../README.md)**

---

## 🚀 Quick Setup

### Prerequisites
- **Node.js 16+**
- **npm or yarn**

### Installation

1. **Navigate to frontend directory**
   ```bash
   cd frontend_react
   ```

2. **Install dependencies**
   ```bash
   npm install
   ```

3. **Start development server**
   ```bash
   npm start
   ```

The app will automatically open at: **http://localhost:3000/**

---

## 🏗️ Project Structure

```
frontend_react/
├── public/
│   ├── index.html               # HTML template
│   ├── hermes_logo.png          # Hermes logo (gold silhouette)
│   └── favicon.ico
├── src/
│   ├── components/
│   │   └── Layout.jsx           # Main layout with navigation & sidebar
│   ├── pages/
│   │   ├── Dashboard.jsx        # Portfolio overview with 4 key metrics
│   │   ├── Portfolio.jsx        # Portfolio summary page
│   │   ├── Holdings.jsx         # Current stock positions (auto-refresh)
│   │   ├── Performance.jsx      # Performance charts with time periods
│   │   ├── Trading.jsx          # Buy/Sell interface with price charts
│   │   ├── Transactions.jsx     # Transaction history
│   │   ├── Signals.jsx          # Trading signals with filtering
│   │   ├── HermesAgent.jsx      # Bot management dashboard
│   │   ├── MLStrategies.jsx     # ML strategy analysis tools
│   │   ├── Profile.jsx          # User profile management
│   │   ├── Login.jsx            # User authentication
│   │   └── Register.jsx         # User registration
│   ├── contexts/
│   │   └── AuthContext.jsx      # Authentication state management
│   ├── services/
│   │   └── api.js               # API client (Axios)
│   ├── utils/
│   │   └── format.js            # Formatting utilities (currency, %)
│   ├── config.js                # App configuration
│   ├── App.jsx                  # Main app with routing
│   └── index.js                 # Entry point
├── package.json                 # Dependencies and scripts
├── package-lock.json            # Locked dependency versions
└── README.md                    # This file
```

---

## 🎨 Key Components

### **Layout.jsx** (Main Layout)

**Features:**
- Responsive sidebar with navigation
- Hermes logo (centered, 60px height)
- Top app bar with user profile
- Signal badge notifications (red circle with count)
- Auto-updates signal count every 60 seconds
- Responsive mobile drawer

**Navigation Items:**
1. Dashboard
2. Portfolio
3. Holdings (BusinessCenter icon)
4. Performance
5. Trading
6. Transactions
7. **Signals** (with badge)
8. **Hermes Agent** (SmartToy icon)
9. ML Strategies

**Theme Colors:**
- Background: `#0c0f27` (dark navy)
- Accent: `#febc4c` (gold)
- Selected item: Gold text + bold
- Unselected item: White text

---

### **Dashboard.jsx** (Portfolio Overview)

**Features:**
- **4 Key Metrics Cards:**
  1. Total Balance (cash available)
  2. Current Value (holdings value)
  3. Unrealized P/L (open positions)
  4. Realized P/L (closed positions profit)
- Portfolio summary section
- Recent transactions list
- Quick navigation to other pages

**Color Coding:**
- Positive P/L: Green (`success.main`)
- Negative P/L: Red (`error.main`)

---

### **Holdings.jsx** (Current Positions)

**Features:**
- **Auto-refresh every 30 seconds**
- Displays all current stock positions
- Real-time price updates
- Profit/Loss calculations
- Summary cards (Total Invested, Current Value, Total P/L)
- Automatically saves portfolio snapshots on refresh

**Table Columns:**
- Stock symbol
- Quantity
- Buying price
- Current price
- Invested amount
- Current value
- P/L (dollar amount)
- P/L % (percentage)
- Status badge (Gain/Loss)

---

### **Performance.jsx** (Performance Charts)

**Features:**
- Interactive line charts (Recharts)
- **Time period selector:** 1D, 1W, 1M, 3M, 1Y, 5Y
- **View mode toggle:** Complete Portfolio vs Individual Stock
- Stock selector dropdown
- Best/Worst performers display
- Holdings performance ranking table

**Chart Configuration:**
- Y-axis: Dynamic domain (`dataMin - 200` to `dataMax + 200`)
- X-axis: Formatted dates
- Tooltip: Shows value on hover
- Line: Blue (`#1976d2`), 2px width
- Grid: Dashed lines

---

### **Trading.jsx** (Buy/Sell Interface)

**Features:**
- **Buy Tab:**
  - Stock symbol input
  - "Get Current Price" button
  - Real-time price display
  - Historical price chart (1M default)
  - Quantity input
  - Total cost calculation
  - Available balance display

- **Sell Tab:**
  - Stock selector (from holdings)
  - Current price display
  - Quantity input (max = owned shares)
  - Total proceeds calculation
  - Realized P/L tracking

**Charts:**
- Uses Recharts LineChart
- Shows historical prices
- Multiple time periods
- Blue line with tooltips

---

### **Signals.jsx** (Trading Signals)

**Features:**
- Signal cards with color-coded actions
- **Filter tabs:** All, Unread, Buy, Sell, Watch
- **Signal actions:**
  - "Buy Now" button (quick trade)
  - "Mark as Read" button
  - "Dismiss" button
- Displays:
  - Stock symbol
  - Signal title
  - Description
  - Current price
  - Index name (for index additions)
  - Created timestamp

**Badge Colors:**
- Buy: Green
- Sell: Red
- Watch: Blue
- Hold: Grey

---

### **HermesAgent.jsx** (Bot Management)

**Features:**
- **"Create Bot" tab:**
  - Investment amount input
  - Duration selector (weeks)
  - Risk level selector (LOW/MEDIUM/HIGH)
  - Create button

- **"My Bots" tab:**
  - Bot list table
  - Columns:
    - Bot name
    - Risk level (chip)
    - Status (chip: Active/Paused/Stopped)
    - Investment
    - Current value
    - P/L
    - ROI %
    - Total trades
    - Win rate

**Risk Level Chips:**
- LOW: Grey
- MEDIUM: Orange
- HIGH: Red

---

### **MLStrategies.jsx** (ML Analysis Tools)

**Features:**
- **4 Strategy Tabs:**
  1. Pivot Point Analysis
  2. Next-Day Prediction
  3. Stock Screener
  4. Index Rebalancing

- Stock symbol input
- "Analyze" button
- Results display with recommendations
- Support/resistance levels
- Buy/sell signals

---

## 🔌 API Integration

### **API Client** (`services/api.js`)

Centralized API client using Axios with token authentication.

```javascript
import api from './services/api'

// Example usage
const holdings = await api.get('/holdings/')
```

**API Objects:**

```javascript
// Authentication
export const authAPI = {
  login: (credentials) => api.post('/users/login/', credentials),
  register: (userData) => api.post('/users/register/', userData),
  logout: () => api.post('/users/logout/'),
}

// User
export const userAPI = {
  profile: () => api.get('/users/profile/'),
  updateProfile: (data) => api.put('/users/update_profile/', data),
}

// Trading
export const tradingAPI = {
  buy: (data) => api.post('/trading/buy/', data),
  sell: (data) => api.post('/trading/sell/', data),
  getStockPrice: (data) => api.post('/trading/get_stock_price/', data),
}

// Holdings
export const holdingAPI = {
  list: () => api.get('/holdings/'),
  summary: () => api.get('/holdings/summary/'),
  refreshPrices: () => api.post('/holdings/refresh_prices/'),
}

// Portfolio
export const portfolioAPI = {
  summary: () => api.get('/portfolio/summary/'),
  performance: () => api.get('/portfolio/performance/'),
  saveSnapshot: () => api.post('/portfolio-snapshots/save_snapshot/'),
  getPortfolioHistory: (period) => api.get(`/portfolio-snapshots/portfolio_history/?period=${period}`),
  getStockHistory: (stock, period) => api.get(`/portfolio-snapshots/stock_history/?stock=${stock}&period=${period}`),
}

// Signals
export const signalAPI = {
  list: () => api.get('/signals/'),
  active: () => api.get('/signals/active/'),
  unreadCount: () => api.get('/signals/unread_count/'),
  markRead: (id) => api.post(`/signals/${id}/mark_read/`),
  dismiss: (id) => api.post(`/signals/${id}/dismiss/`),
}

// Hermes Bots
export const hermAPI = {
  create: (data) => api.post('/herm/create/', data),
  list: () => api.get('/herm/list/'),
  status: (botId) => api.get(`/herm/${botId}/status/`),
}

// ML Strategies
export const mlAPI = {
  pivotAnalysis: (data) => api.post('/ml/pivot/', data),
  nextDayPrediction: (data) => api.post('/ml/predict/', data),
  stockScreener: (data) => api.post('/ml/screener/', data),
  indexRebalancing: (data) => api.post('/ml/index-event/', data),
}

// Transactions
export const transactionAPI = {
  list: () => api.get('/transactions/'),
  create: (data) => api.post('/transactions/', data),
}
```

**Authentication:**
- Token stored in `localStorage` as `token`
- Automatically added to all requests via interceptor
- Removed on logout

---

## 🎨 Styling & Theme

### **Material-UI Custom Theme**

**Colors:**
- **Primary:** `#0c0f27` (Dark navy background)
- **Secondary:** `#febc4c` (Gold accent)
- **Success:** `#4caf50` (Green for profits)
- **Error:** `#f44336` (Red for losses)
- **Background:** `#0c0f27`
- **Paper:** `#15182e`
- **Text Primary:** `#ffffff` (White)
- **Text Secondary:** `#b0b0b0` (Grey)

**Typography:**
- Font Family: Roboto (Material-UI default)
- Headers: Bold weight
- Body: Regular weight

**Component Overrides:**
- Cards: Dark background with subtle border
- Tables: Dark theme with hover effects
- Buttons: Gold primary, outlined secondary
- Chips: Color-coded by status/risk

---

### **Chart Styling (Recharts)**

**Performance Charts:**
```javascript
<LineChart data={chartData}>
  <CartesianGrid strokeDasharray="3 3" stroke="#333" />
  <XAxis 
    dataKey="date" 
    stroke="#888" 
    tick={{ fill: '#888', fontSize: 10 }}
  />
  <YAxis 
    stroke="#888"
    tick={{ fill: '#888', fontSize: 12 }}
    domain={['dataMin - 200', 'dataMax + 200']}
    tickFormatter={(value) => `$${value.toLocaleString()}`}
  />
  <Tooltip 
    contentStyle={{ 
      backgroundColor: '#15182e', 
      border: '1px solid #333' 
    }}
  />
  <Line 
    type="monotone" 
    dataKey="value" 
    stroke="#1976d2" 
    strokeWidth={2}
    dot={false}
  />
</LineChart>
```

**Trading Price Charts:**
- Blue line (`#1976d2`)
- Tooltips with dark background
- Responsive container
- Date formatting on X-axis

---

## 🔔 Features & Functionality

### **Auto-Refresh Mechanism**

**Holdings Page:**
```javascript
useEffect(() => {
  const interval = setInterval(async () => {
    await holdingAPI.refreshPrices()
    await portfolioAPI.saveSnapshot()  // Saves for performance chart
    await loadHoldings()
  }, 30000) // Every 30 seconds

  return () => clearInterval(interval)
}, [])
```

**Signal Badge:**
```javascript
useEffect(() => {
  const fetchUnreadCount = async () => {
    const response = await signalAPI.unreadCount()
    setUnreadSignals(response.data.unread_count)
  }
  
  fetchUnreadCount()
  const interval = setInterval(fetchUnreadCount, 60000) // Every 60 seconds
  
  return () => clearInterval(interval)
}, [])
```

---

### **Real-Time Price Fetching**

**Trading Page:**
```javascript
const fetchStockPrice = async () => {
  setLoadingPrice(true)
  try {
    const response = await tradingAPI.getStockPrice({
      stock: symbol,
      period: timePeriod
    })
    
    setCurrentPrice(response.data.current_price)
    setHistoricalData(response.data.historical_data)
    setPriceChange(response.data.price_change)
  } catch (error) {
    toast.error('Failed to fetch stock price')
  } finally {
    setLoadingPrice(false)
  }
}
```

---

### **Chart Data Formatting**

**Performance Chart:**
```javascript
const loadPortfolioChart = async () => {
  const response = await portfolioAPI.getPortfolioHistory(timePeriod)
  
  const data = response.data.data.map(item => ({
    date: new Date(item.date).toLocaleString('en-US', { 
      month: 'short', 
      day: 'numeric',
      hour: timePeriod === '1D' ? '2-digit' : undefined,
      minute: timePeriod === '1D' ? '2-digit' : undefined
    }),
    value: parseFloat(item.total_value)  // IMPORTANT: Convert to number
  }))
  
  setChartData(data)
}
```

---

## 📦 Dependencies

### **Core Libraries**

```json
{
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0",
    "react-router-dom": "^6.14.0",
    "axios": "^1.4.0",
    "@mui/material": "^5.14.0",
    "@mui/icons-material": "^5.14.0",
    "@emotion/react": "^11.11.0",
    "@emotion/styled": "^11.11.0",
    "recharts": "^2.5.0",
    "react-toastify": "^9.1.3"
  }
}
```

### **Development Dependencies**

```json
{
  "devDependencies": {
    "react-scripts": "5.0.1"
  }
}
```

---

## 🔧 Configuration

### **API Base URL**

**File:** `src/config.js`

```javascript
const config = {
  API_URL: process.env.REACT_APP_API_URL || 'http://localhost:8000/api',
}

export default config
```

**Environment Variables (.env):**
```env
REACT_APP_API_URL=http://localhost:8000/api
```

---

### **Authentication Context**

**File:** `src/contexts/AuthContext.jsx`

Provides authentication state globally:
```javascript
const { user, token, login, logout, isAuthenticated } = useAuth()
```

**Features:**
- Persists token in `localStorage`
- Auto-loads user on app start
- Provides login/logout functions
- `PrivateRoute` wrapper for protected pages

---

## 🎯 Routing

**File:** `src/App.jsx`

```javascript
<Routes>
  {/* Public Routes */}
  <Route path="/login" element={<Login />} />
  <Route path="/register" element={<Register />} />
  
  {/* Protected Routes */}
  <Route path="/dashboard" element={
    <PrivateRoute><Layout><Dashboard /></Layout></PrivateRoute>
  } />
  <Route path="/portfolio" element={
    <PrivateRoute><Layout><Portfolio /></Layout></PrivateRoute>
  } />
  <Route path="/holdings" element={
    <PrivateRoute><Layout><Holdings /></Layout></PrivateRoute>
  } />
  <Route path="/performance" element={
    <PrivateRoute><Layout><Performance /></Layout></PrivateRoute>
  } />
  <Route path="/trading" element={
    <PrivateRoute><Layout><Trading /></Layout></PrivateRoute>
  } />
  <Route path="/transactions" element={
    <PrivateRoute><Layout><Transactions /></Layout></PrivateRoute>
  } />
  <Route path="/signals" element={
    <PrivateRoute><Layout><Signals /></Layout></PrivateRoute>
  } />
  <Route path="/hermes-agent" element={
    <PrivateRoute><Layout><HermesAgent /></Layout></PrivateRoute>
  } />
  <Route path="/ml-strategies" element={
    <PrivateRoute><Layout><MLStrategies /></Layout></PrivateRoute>
  } />
  <Route path="/profile" element={
    <PrivateRoute><Layout><Profile /></Layout></PrivateRoute>
  } />
  
  {/* Default Redirect */}
  <Route path="/" element={<Navigate to="/login" />} />
</Routes>
```

---

## 🧪 Development

### **Start Development Server**
```bash
npm start
```
- Runs on `http://localhost:3000`
- Hot-reloads on file changes
- Opens browser automatically

### **Build for Production**
```bash
npm run build
```
- Creates optimized production build
- Output in `build/` folder
- Minified and optimized

### **Run Tests**
```bash
npm test
```
- Runs Jest test suite
- Interactive watch mode

### **Eject (Not Recommended)**
```bash
npm run eject
```
- Exposes Create React App configuration
- **Warning:** This is one-way and irreversible

---

## 🐛 Troubleshooting

### **Module Not Found**
```bash
# Clear and reinstall
rm -rf node_modules package-lock.json
npm install
```

### **Port Already in Use**
```bash
# Use different port
PORT=3001 npm start
```

### **API Connection Errors**
1. Verify backend is running on `http://localhost:8000`
2. Check `src/config.js` for correct API_URL
3. Check browser console for CORS errors
4. Verify token is stored in `localStorage`

### **Charts Not Displaying**
1. Check if data is being fetched (Network tab)
2. Verify `parseFloat()` is used on values
3. Check Y-axis domain configuration
4. Install Recharts: `npm install recharts`

### **Signal Badge Not Updating**
1. Check if `signalAPI.unreadCount()` is being called
2. Verify 60-second interval is running
3. Check browser console for errors

### **Auto-Refresh Not Working**
1. Verify intervals are being set in `useEffect`
2. Check if cleanup function returns `clearInterval()`
3. Test API endpoints manually

---

## 🎨 Customization

### **Change Theme Colors**

**File:** `src/App.jsx` (or create `theme.js`)

```javascript
const theme = createTheme({
  palette: {
    mode: 'dark',
    primary: {
      main: '#0c0f27',  // Change this
    },
    secondary: {
      main: '#febc4c',  // Change this
    },
    background: {
      default: '#0c0f27',
      paper: '#15182e',
    },
  },
})
```

### **Change Logo**

Replace `public/hermes_logo.png` with your logo (60px height recommended)

### **Modify Navigation**

**File:** `src/components/Layout.jsx`

Edit the `menuItems` array:
```javascript
const menuItems = [
  { text: 'Dashboard', icon: <Dashboard />, path: '/dashboard' },
  // Add or remove items here
]
```

---

## 📚 Additional Resources

- **Main README:** [../README.md](../README.md)
- **Backend README:** [../backend_django/README.md](../backend_django/README.md)
- **React Documentation:** https://reactjs.org/
- **Material-UI Documentation:** https://mui.com/
- **Recharts Documentation:** https://recharts.org/
- **React Router Documentation:** https://reactrouter.com/

---

## 🤝 Development Guidelines

When contributing to the frontend:

1. **Component Structure:**
   - Use functional components with hooks
   - Keep components under 300 lines
   - Extract reusable logic into custom hooks

2. **State Management:**
   - Use `useState` for local state
   - Use Context API for global state (auth)
   - Consider Redux for complex state (future)

3. **API Calls:**
   - Always use try-catch blocks
   - Show loading indicators
   - Display error messages with `react-toastify`
   - Handle edge cases (empty data, network errors)

4. **Styling:**
   - Use Material-UI components when possible
   - Follow the existing color scheme
   - Keep responsive design in mind
   - Test on mobile devices

5. **Performance:**
   - Use `useCallback` for memoized functions
   - Use `useMemo` for expensive computations
   - Clean up intervals/timers in `useEffect` cleanup

---

**For complete project setup and architecture, see the [Main README](../README.md).**
