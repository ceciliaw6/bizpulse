# BizPulse

A business intelligence dashboard for small service businesses — salons, spas, and any booking-based operation. Turns raw appointment exports into actionable customer analytics: segmentation, CLV modeling, churn prediction, and AI-generated outreach.

> "Know your business before it surprises you."

Built with Python, Streamlit, and probabilistic ML models (BG/NBD, Gamma-Gamma).

## Live Demo

Try it live at [bizpulseapp.streamlit.app](https://bizpulseapp.streamlit.app). There is an option to skip account or log-in.


Alternatively, use the direct demo link [bizpulseapp.streamlit.app/?demo=true](https://bizpulseapp.streamlit.app/?demo=true) to skip login and jump straight to the dashboard with sample data — no account or upload required.

---

## The Problem

Small service business owners sit on a gold mine of appointment data from Square, Vagaro, Toast, or other booking systems — but almost none of them analyze it. Most don't know which customers are about to churn, which ones are ready to upgrade, or how much revenue they're leaving on the table. BizPulse turns a raw CSV export into a full customer intelligence suite in under a minute.

---

## Features

### Dashboard Overview
A health score (0–100) computed from retention, loyalty, growth, and monetization metrics — plus the top 3 recommended actions to take this week.

### Customer Segmentation
Automatically tiers customers by visit frequency:

| Tier | Visits | Description |
|---|---|---|
| Explorer | 1–2 | First-timers worth converting |
| Casual | 3–8 | Regulars in the making |
| Regular | 9–12 | Core customer base |
| Superuser | 13+ | Your most loyal advocates |

### Latent Demand Modeling
Uses a Zero-Truncated NBD (Negative Binomial Distribution) model to estimate the unobserved potential customer pool — how many people *could* be your customers but haven't booked yet.

### Retention Alerts
Surfaces customers at risk of lapsing at 30, 60, and 90-day intervals. Calculates value at risk for each cohort so you know exactly what you stand to lose.

### CLV (Customer Lifetime Value)
Fits a Gamma-Gamma spend model to predict each customer's lifetime value. Identifies your highest-value customers and hidden gems who spend above average but visit infrequently.

### Churn Prediction
Scores every customer's churn probability using a heuristic recency-frequency model. Generates a risk scatter plot and personalized win-back message templates.

### Upgrade Opportunities
Identifies customers who are one step below the next tier and generates coupon codes + one-click email/WhatsApp outreach links to nudge them.

---

## Tech Stack

| Layer | Tech |
|---|---|
| Framework | Streamlit |
| Data processing | Pandas, NumPy |
| Probabilistic models | `lifetimes` (BG/NBD, Gamma-Gamma), SciPy |
| Visualization | Plotly |
| Outreach | mailto links, WhatsApp deep links |

---

## Getting Started

### Prerequisites

- Python 3.9+
- pip

### Installation

```bash
git clone https://github.com/ceciliaw6/bizpulse.git
cd bizpulse
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate
pip install -r requirements.txt
streamlit run app.py
```

Open [http://localhost:8501](http://localhost:8501).

No environment variables required for core functionality.

### Try It With Sample Data

A sample dataset from a fictional nail salon is included at `sample_data/appointments_sample.csv`. Upload it on the Upload page to explore all features immediately.

---

## Data Format

BizPulse accepts CSV exports from Square, Vagaro, Toast, or any booking system. Required columns:

| Column | Required | Notes |
|---|---|---|
| `client_name` | ✅ | Customer identifier |
| `start` | ✅ | Appointment datetime (`MM-DD-YYYY HH:MM:SS AM/PM`) |
| `status` | ✅ | App filters to `accepted` appointments only |
| `service` | Optional | Used for price estimation if totals are missing |
| `estimated_total_price` | Optional | Transaction amount |
| `email`, `phone` | Optional | Powers one-click outreach links |

---

## Project Structure

```
bizpulse/
├── app.py                        # Main app: login, upload, 6-tab dashboard
├── requirements.txt
├── utils/
│   ├── data_loader.py            # CSV parsing, validation, price estimation
│   ├── segmentation.py           # Customer tier assignment
│   ├── models.py                 # NBD, Gamma-Gamma CLV, churn scoring
│   └── outreach.py               # Message templates, coupon codes, mailto/WhatsApp
├── sample_data/
│   └── appointments_sample.csv   # Demo dataset (fictional nail salon)
└── data/uploads/                 # Runtime upload directory (gitignored)
```

---

## Health Score Formula

The dashboard health score (0–100) weights four dimensions:

```
30% — Retention    (% active in last 60 days)
25% — Loyalty      (share of regulars + superusers)
25% — Growth       (new customers in last 90 days)
20% — Monetization (average customer value)
```

---

## Roadmap

- [ ] Google Sheets / Square API direct integration
- [ ] Email campaign scheduling via SendGrid
- [ ] Multi-location support
- [ ] LLM-generated weekly business narrative ("Your top risk this week is...")
- [ ] Benchmarking against industry averages

---

## License

MIT
