# Wellness Behavior Analytics API

A behavioral analytics engine built on 90 days of real longitudinal habit-tracking data, designed to mirror the insights layer that powers consumer wellness apps.

---

## Business Context

Apps like HealthifyMe, Cult.fit, and Habitica collect millions of habit completions daily. But raw completion data alone is not what retains users. Behavioral insight is. Users stay when an app tells them why they are struggling, what is working, and what to do next.

This project builds exactly that: a FastAPI-based analytics engine that sits on top of habit-tracking data and serves computed KPIs, trend detection, and human-readable insights on demand.

The dataset is 90 days of real personal tracking data, 10 habits across 6 behavioral categories, approximately 900 rows. Using real data means the system handles the messiness actual wellness apps deal with: inconsistency, uneven effort, and streaks that break.

**Target use case:** Any wellness or behavior-change platform that needs to move from data storage to actionable insight delivery.

---

## Key Findings

- **Outdoor Activities** had the lowest completion (43%) despite being the lowest-effort habit, a classic intention-action gap common in wellness app user cohorts
- **Spiritual habits** outperformed **Emotional ones** by 10 percentage points, suggesting category-level friction worth surfacing to users
- Effort-weighted completion score exposed a behavioral bias: users complete easy tasks more than hard ones, an honest signal that most apps do not compute
- Momentum metric (14-day rolling delta) caught a plateau before it became a drop-off, exactly the kind of early signal retention systems need

---

## Project Structure
```
wellness_behavior_api/
├── app/
│   ├── main.py               # FastAPI app entry point
│   ├── data_loader.py        # Data cleaning and feature engineering
│   ├── metrics.py            # KPI computation logic
│   ├── insights_engine.py    # Narrative insight generation
│   └── routers/
│       ├── ingest.py         # Dataset profile and CSV upload
│       ├── metrics.py        # Metrics endpoints
│       └── insights.py       # Insight endpoints
├── data/
│   └── habit_tracking_sample.csv
├── notebooks/
│   └── eda.ipynb             # Exploratory data analysis
├── docs/
│   └── METRICS.md            # Full metric definitions
├── requirements.txt
└── README.md
```

---

## API Endpoints

| Method | Endpoint | Description |
|---|---|---|
| GET | `/health` | API status check |
| GET | `/ingest/profile` | Dataset summary (rows, date range, completeness) |
| POST | `/ingest/upload` | Upload and validate a new user CSV |
| GET | `/metrics/overview` | Completion rate, effort-weighted score, momentum |
| GET | `/metrics/category` | Breakdown by behavioral category and balance index |
| GET | `/metrics/tasks` | Per-habit completion rates and streak data |
| GET | `/metrics/trends` | Weekly trend and day-of-week pattern |
| GET | `/insights/top-wins` | Auto-generated positive behavioral signals |
| GET | `/insights/bottlenecks` | Detected friction points in habit performance |
| GET | `/insights/recommendations` | Rules-based nudges for the user |
| GET | `/insights/today` | Daily summary card with completions, streaks, and message |

---

## Getting Started
```bash
git clone https://github.com/YOUR_USERNAME/wellness-behavior-api.git
cd wellness-behavior-api
pip install -r requirements.txt
uvicorn app.main:app --reload
# Interactive docs available at http://127.0.0.1:8000/docs
```

---

## Sample Responses

**GET /metrics/overview**
```json
{
  "overall_completion_rate_pct": 53.0,
  "effort_weighted_completion_pct": 51.2,
  "momentum": {
    "recent_14d_pct": 56.4,
    "previous_14d_pct": 51.8,
    "delta": 4.6,
    "direction": "improving"
  },
  "total_days_tracked": 90,
  "total_tasks_logged": 900
}
```

**GET /insights/bottlenecks**
```json
{
  "insights": [
    "Outdoor Activities has the lowest completion rate (43.3%) despite being a low-effort habit. Possible intention-action gap detected.",
    "Emotional category is underperforming at 47.8%, which is 10 points below Spiritual.",
    "Day-of-week analysis shows Sunday completions drop significantly, a common behavioral pattern."
  ]
}
```

**GET /insights/today**
```json
{
  "date": "2026-02-28",
  "completed": ["Affirmation", "Prayer", "Art", "Household Chores", "Journaling", "Dance"],
  "missed": ["Outdoor Activities", "Gratitude", "Errands", "Day Visualization"],
  "completion_rate_today": 60.0,
  "active_streaks": { "Prayer": 5, "Affirmation": 3 },
  "message": "Strong day. 6 out of 10 habits completed."
}
```

---

## Metrics Reference

All 7 KPIs have explicit definitions with grain, denominator rules, edge case handling, and interpretation notes. See [docs/METRICS.md](docs/METRICS.md).

**Metrics included:** Completion Rate, Effort-Weighted Completion, Streaks, Momentum (14-day delta), Category Balance Index, Day-of-Week Pattern, Weekly Trend

---

## Tech Stack

| Tool | Purpose |
|---|---|
| Python and Pandas | Data cleaning, feature engineering, metric computation |
| FastAPI | API framework |
| Uvicorn | ASGI server |
| Pydantic | Response validation |
| Power BI (optional) | Dashboard fed by exported metrics |

---

## What This Demonstrates

- Designing a behavioral data model with metric grain, keys, and denominator rules, consistent with wellness app analytics pipelines
- Building a production-style REST API that serves computed insights rather than raw data
- Working with real longitudinal behavioral data that is messy, inconsistent, and human
- Translating behavioral patterns into actionable insight outputs
- Domain fluency in habit science concepts including intention-action gaps, effort bias, momentum, and category balance
