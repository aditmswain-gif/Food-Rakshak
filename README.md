# Food Rakshak

Food Rakshak is a local, full-stack MVP for tracking institutional meal preparation, food surplus, safety eligibility, demand forecasts, redistribution workflows and environmental impact.

## Quick start on Windows

```powershell
cd C:\Users\hp\Food-Rakshak
py -3.13 -m venv venv
.\venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
pip install -r requirements.txt
python backend/app.py
```

Open http://127.0.0.1:5000

## Demo workflow

1. Open **Kitchen Management** and add a meal record.
2. Open **Food Inventory** and add surplus food with a safe-use deadline.
3. Open **Surplus Management** and view ranked demo recipients.
4. Create a request, then change it to Collected and Delivered.
5. Open **AI Predictions** to generate a forecast.
6. View updated results in Dashboard and Impact Analytics.
7. Export kitchen or redistribution CSV files from Reports.

## Architecture

- Vanilla JavaScript frontend with responsive HTML/CSS and Chart.js
- Flask REST API
- SQLite relational database
- Scikit-learn Random Forest when enough history exists
- Rolling-average fallback for limited data
- Local distance and category matching for demo NGOs

## Main API endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| GET | /api/dashboard | Dashboard totals and charts |
| GET/POST | /api/kitchens | List or create kitchen records |
| PUT/DELETE | /api/kitchens/<id> | Edit or delete kitchen records |
| GET/POST | /api/food | List or create inventory |
| PUT/DELETE | /api/food/<id> | Update or delete inventory |
| GET/POST | /api/ngos | List or create demo recipients |
| GET | /api/food/<id>/matches | Ranked recipient matches |
| GET/POST | /api/redistribution | List or create requests |
| PUT | /api/redistribution/<id> | Update workflow status |
| GET/POST | /api/predictions | View or create forecasts |
| GET | /api/analytics | Date-filtered analytics |
| GET | /api/reports | Period and institution reports |
| GET | /api/reports/export | CSV download |

## Prediction and analytics assumptions

- The model uses actual consumption, weekday, meal type and expected consumers.
- Random Forest is used when at least 12 compatible samples are available and scikit-learn is installed.
- Otherwise the model transparently reports a historical rolling-average fallback.
- Meal conversion assumes 0.36 kg per estimated meal.
- Recommended preparation adds a 5% operational buffer.
- `surplus_quantity = quantity_prepared - quantity_consumed` in kitchen records.
- Waste is tracked separately and remaining surplus is not automatically treated as waste.

## Safety limitations

The inventory eligibility check is only a prototype. It does not replace FSSAI rules, local regulations, time-temperature controls or inspection by qualified food safety personnel. Expired, contaminated or unsafe records are excluded from recipient matching.

## Redistributed organizations

All NGO records are clearly marked as demonstration data. The application does not contact real recipients or send messages. Human collection confirmation is simulated locally.

## Database

The database is `food_rakshak.db` in the project root. It is initialized and seeded automatically when the Flask application starts. Delete the database file to regenerate demonstration data.

## Tests

```powershell
pytest -q
```

## Limitations and future work

- No authentication or role management is enabled.
- NGO and map coordinates are simulated.
- Demand forecasting improves only with real historical records.
- PDF reports and real SMS/email notifications require external integrations.
