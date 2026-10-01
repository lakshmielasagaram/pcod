# AI-Based Smart Menstrual & PCOS Risk System

Hackathon prototype built with Flask, SQLite, Bootstrap and ReportLab.
It is a rule-based, explainable **screening** tool. It does not diagnose PCOS or any other condition.

## Run locally

```bash
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate
pip install -r requirements.txt
python app.py
```

Open http://127.0.0.1:5000 and register an account. `database.db` is created on first run.
Bootstrap and the font load from a CDN, so the browser needs internet access.

Optional: set `SECRET_KEY` in the environment; otherwise a key is generated into `.secret_key`.

## Demo data
Add periods starting 1 Jan, 4 Feb, 10 Mar and 20 May (each about 5 days), answer the questionnaire,
then open "Check risk" and "Doctor report".

## How the score works (out of 100)
| Factor | Max points |
|---|---|
| Irregular cycle pattern (from saved dates) | 25 |
| Long cycle pattern (average / most cycles over 35 days) | 15 |
| Excess facial/body hair | 14 |
| Irregular periods, acne, hair thinning, weight changes | 8 each |
| Long cycles (self-reported) | 4 |
| Family history | 4 |
| Pelvic discomfort, fatigue | 3 each |

"Sometimes" earns half the points, "Often"/"Yes" all of them. Under 30 = low, 30-59 = moderate, 60+ = higher screening risk.
Cycle irregularity is flagged when a cycle is outside 21-35 days, cycle lengths differ by 9+ days,
or the latest cycle differs from the earlier average by more than 7 days.

## Notes
- Passwords are hashed (Werkzeug); all forms carry a CSRF token; sessions expire after 2 hours.
- PDFs are built in `reports/`, sent to the browser and deleted straight away.
- Deleting periods or symptoms also removes saved screening results, since they depend on that data.
