# BizPulse

Business intelligence dashboard for small service businesses (salons, spas, booking-based). Turns raw appointment data into customer analytics, segmentation, retention insights, and actionable outreach.

**Tagline:** "Know your business before it surprises you"
**Target users:** Service-based small business owners

## Tech Stack

- **Framework:** Streamlit (>=1.28.0)
- **Data:** Pandas, NumPy, SciPy
- **Visualization:** Plotly
- **Models:** lifetimes (CLV), custom NBD/Gamma-Gamma/Churn models

## Setup

```bash
cd ~/Projects/bizpulse
python -m venv venv
source venv/bin/activate
pip install -r requirements.txt
streamlit run app.py              # http://localhost:8501
```

No environment variables required. Optionally configure `.streamlit/secrets.toml` for API keys.

## Project Structure

```
bizpulse/
├── app.py                        # Main app: login, upload, dashboard with 6 tabs
├── requirements.txt              # Python dependencies
├── utils/
│   ├── data_loader.py            # CSV loading, validation, price estimation
│   ├── segmentation.py           # Customer tier assignment by visit frequency
│   ├── models.py                 # NBD, Gamma-Gamma CLV, churn probability
│   └── outreach.py               # Message templates, coupon codes, email/WhatsApp links
├── pages/                        # Streamlit multi-page files (legacy, now integrated in app.py)
│   ├── 0_Upload.py
│   ├── 1_Segmentation.py
│   ├── 2_Latent_Demand.py
│   ├── 3_Retention.py
│   ├── 4_Upgrades.py
│   ├── 5_CLV.py
│   └── 6_Churn.py
├── data/uploads/                 # User-uploaded CSVs
└── sample_data/
    └── appointments_sample.csv   # Example nails salon data (BarefootCare, Sept-Dec 2023)
```

## User Flow

1. **Login** - Enter email and business name (split-screen layout)
2. **Upload** - CSV from Square/Toast/booking system
3. **Dashboard** - Health score (0-100) + top 3 actions + 6 analytics tabs

## Data Format

CSV with columns:
- `client_name` (required) - Customer identifier
- `start` (required) - Appointment datetime (MM-DD-YYYY HH:MM:SS AM/PM)
- `status` (required) - Filters to "accepted" only
- `service` (optional) - Used for price estimation
- `estimated_total_price` (optional) - Transaction amount
- `email`, `phone` (optional) - For outreach links

## Dashboard Tabs

| Tab | What it does |
|---|---|
| **Overview** | KPI cards (customers, revenue, avg visits, active rate), segment charts |
| **Segments** | Customer tiers: Explorer (1-2), Casual (3-8), Regular (9-12), Superuser (13+) |
| **Latent Demand** | Zero-Truncated NBD model estimates unobserved market size |
| **Retention** | At-risk customers (30/60/90 days), value at risk, overdue table |
| **Upgrades** | Upgrade candidates, coupon codes, editable message templates |
| **CLV** | Customer lifetime value, top 10 customers, hidden gems |
| **Churn** | Churn probability scoring, risk scatter plot, win-back messages |

## Health Score Formula

- 30% retention (active in last 60 days)
- 25% loyalty (regulars + superusers share)
- 25% growth (new customers in last 90 days)
- 20% monetization (avg customer value)

## Key Utilities

- **data_loader.py** - Validates CSV, parses dates, estimates missing prices via fuzzy service name matching, aggregates to customer-level RFM metrics
- **segmentation.py** - Assigns tiers by visit count
- **models.py** - NBD for market estimation, Gamma-Gamma for CLV (AvgSpend x Frequency x 1.2), heuristic churn scoring
- **outreach.py** - Generates coupon codes, mailto/WhatsApp links, personalized message templates

## Design

Editorial modern style. Colors: dark green (#003D29), cream (#FAF1E5), white. Fonts: Playfair Display (headlines), Inter (body).
