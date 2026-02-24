# ☁️ 100% Cloudflare Deployment Guide

Deploy the entire Nuclear Plant Simulation on Cloudflare infrastructure - NO local server needed!

## 🚀 Quick Deploy (3 Steps)

### Step 1: Create D1 Database

```bash
wrangler d1 create nuclear_plant_db
```

Copy the database ID from output, then update `wrangler.toml`:
```toml
database_id = "your-database-id-here"
```

### Step 2: Initialize Database Schema

```bash
wrangler d1 execute nuclear_plant_db --file=schema.sql
```

### Step 3: Deploy to Cloudflare

```bash
wrangler deploy
```

✅ Done! Your app is live at: `https://nuclear-plant-sim.YOUR-SUBDOMAIN.workers.dev`

## 📋 Complete Setup

### 1. Install Wrangler
```bash
npm install -g wrangler
```

### 2. Login to Cloudflare
```bash
wrangler login
```

### 3. Create D1 Database
```bash
cd d:\project_power\nuclear-plant-sim
wrangler d1 create nuclear_plant_db
```

Output will show:
```
✅ Successfully created DB 'nuclear_plant_db'
database_id = "abc123-def456-ghi789"
```

### 4. Update wrangler.toml
Replace `__D1_ID__` with your actual database ID:
```toml
[[d1_databases]]
binding = "DB"
database_name = "nuclear_plant_db"
database_id = "abc123-def456-ghi789"
```

### 5. Create Database Tables
```bash
wrangler d1 execute nuclear_plant_db --file=schema.sql
```

### 6. Deploy Worker
```bash
wrangler deploy
```

## 🌐 Architecture

```
User Browser
    ↓
Cloudflare Workers (Edge)
    ↓
Durable Objects (State)
    ↓
D1 Database (History)
```

### Components:

1. **Cloudflare Workers**: Serverless compute at the edge
2. **Durable Objects**: Real-time state management + WebSocket
3. **D1 Database**: SQLite-based persistent storage

## 📊 Database Schema

### plant_history table
- Stores simulation data every 10 seconds
- Keeps last 1000 records
- Fields: timestamp, reactor_temp, reactor_pressure, power_output, turbine_speed

### plant_events table
- Logs critical events (alarms, shutdowns)
- Fields: timestamp, event_type, event_data

## 🔧 API Endpoints

All endpoints run on Cloudflare edge:

```
GET  /                    - Dashboard UI
GET  /api/state          - Current plant state
POST /api/control        - Control plant parameters
GET  /api/history        - Historical data from D1
WS   /ws                 - WebSocket real-time updates
```

## 💾 Data Storage

- **Durable Objects**: Real-time state (last 100 data points)
- **D1 Database**: Long-term history (unlimited)

## 🧪 Test Locally (Optional)

```bash
wrangler dev --remote
```

Note: Use `--remote` flag to test with actual D1 database.

## 📈 Monitoring

### View Logs
```bash
wrangler tail
```

### Check D1 Data
```bash
wrangler d1 execute nuclear_plant_db --command="SELECT COUNT(*) FROM plant_history"
```

### Query Historical Data
```bash
wrangler d1 execute nuclear_plant_db --command="SELECT * FROM plant_history ORDER BY timestamp DESC LIMIT 10"
```

## 💰 Cloudflare Pricing

### Free Tier Includes:
- Workers: 100,000 requests/day
- Durable Objects: 1 GB storage
- D1: 5 GB storage, 5M reads/day

### This app uses:
- ~86,400 requests/day (1 per second simulation)
- ~10 KB Durable Objects storage
- ~8,640 D1 writes/day (every 10 seconds)

✅ **Fits perfectly in FREE tier!**

## 🔄 Update Deployment

Make code changes, then:
```bash
wrangler deploy
```

Changes are live instantly worldwide!

## 🌍 Global Edge Network

Your app runs on 300+ Cloudflare data centers worldwide:
- Ultra-low latency
- Automatic scaling
- DDoS protection
- Free SSL/TLS

## 🐛 Troubleshooting

### D1 Database not found
```bash
wrangler d1 list
```
Verify your database exists and ID matches wrangler.toml

### WebSocket connection failed
- Ensure you're using `wss://` for HTTPS
- Check browser console for errors

### Durable Objects error
```bash
wrangler deploy --compatibility-date=2024-01-01
```

## 📱 Access Your Live App

After deployment:
```
https://nuclear-plant-sim.YOUR-SUBDOMAIN.workers.dev
```

Open in any browser - works on desktop, tablet, mobile!

## 🎯 What You Get

✅ Real-time nuclear plant simulation
✅ WebSocket live updates
✅ Interactive control panel
✅ Live graphs and charts
✅ Historical data storage
✅ Global edge deployment
✅ Auto-scaling
✅ 99.99% uptime
✅ Free SSL certificate
✅ No server maintenance

## 🔒 Security

- Automatic HTTPS
- DDoS protection
- Rate limiting (built-in)
- No exposed database credentials
- Edge-side execution

## 📞 Support

- Cloudflare Docs: https://developers.cloudflare.com
- Workers: https://developers.cloudflare.com/workers
- D1: https://developers.cloudflare.com/d1
- Durable Objects: https://developers.cloudflare.com/durable-objects
