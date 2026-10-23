# Mental Health Score

A small full-stack ML project that predicts a student's **mental health score (0–10)** from their social media habits, sleep, study, physical activity and stress level.

- **Model:** scikit-learn pipeline (preprocessing + Random Forest regressor) trained on the *Student Social Media and Mental Health Impact* dataset (~5,000 rows).
- **Backend:** FastAPI app that loads the trained model and exposes a `/predict` endpoint.
- **Frontend:** a static HTML/CSS/JS page with a form and an animated gauge for the result.

> **Disclaimer:** This is a learning project, not a clinical tool. The score is a statistical estimate and is not a diagnosis.

## Project structure

index.html | style.css | script.js | main.py | Mental_Health_Model.pkl | ML_Project.ipynb | Student Social Media And Mental Health Impact.csv | requirements.txt | .gitignore

## Input features

| Field | Type / allowed values |
|---|---|
| `age` | int, 10–100 |
| `gender` | `Male`, `Female` |
| `country` | any string (countries outside the top list are grouped as `Other`) |
| `academic_level` | `High School`, `Undergraduate`, `Graduate` |
| `most_used_platform` | Facebook, Instagram, Snapchat, Twitter, YouTube, TikTok, LinkedIn, LINE, KakaoTalk, VKontakte, WhatsApp, WeChat |
| `purpose_of_use` | `Networking`, `Education`, `Entertainment`, `News` |
| `avg_daily_usage_hours` | float, 0–24 |
| `daily_unlocks` | int, ≥ 0 |
| `study_hours` | float, 0–24 |
| `physical_activity_hours` | float, 0–24 |
| `sleep_hours_per_night` | float, 0–24 |
| `stress_level` | `Low`, `Medium`, `High`, `Very High` |

## Getting started

python -m venv venv
# Windows
venv\Scripts\activate
# macOS / Linux
source venv/bin/activate

pip install -r requirements.txt

> `scikit-learn` is pinned to **1.6.1** because the `.pkl` file was saved with that version.

### 2. Run the API

uvicorn main:app --reload --port 2200

- API: http://127.0.0.1:2200
- Interactive docs (Swagger): http://127.0.0.1:2200/docs

### 3. Point the frontend at your API

const API_BASE = "http://127.0.0.1:2200";   // local

### 4. Open the frontend

python -m http.server 5500
(then visit http://127.0.0.1:5500)

## API reference

### `POST /predict`

**Request body**

{
  "age": 21,
  "gender": "Male",
  "country": "India",
  "academic_level": "Undergraduate",
  "most_used_platform": "Instagram",
  "purpose_of_use": "Entertainment",
  "avg_daily_usage_hours": 4.5,
  "daily_unlocks": 80,
  "study_hours": 5,
  "physical_activity_hours": 1,
  "sleep_hours_per_night": 7,
  "stress_level": "Medium"
}

**Response**

{ "prediction_mental_health_score": 6.8 }

Invalid input returns a `422` response with field-level details, which the frontend maps onto the form.

## Retraining the model

Open `ML_Project.ipynb`, make sure the CSV path in the first `pd.read_csv(...)` cell is correct, and run all cells. Export the fitted pipeline with `joblib.dump(model, "Mental_Health_Model.pkl")` and keep the `scikit-learn` version in `requirements.txt` in sync with the one you trained with.

## Tech stack

Python · pandas · scikit-learn · FastAPI · Uvicorn · HTML · CSS · JavaScript