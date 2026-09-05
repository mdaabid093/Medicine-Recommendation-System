# Medicine Recommendation System

A Flask web application that predicts a likely disease from a set of symptoms using a trained ML model, then returns the disease description, precautions, medications, recommended diet, and workout — combined with a full clinic-style appointment platform for patients, doctors, and admins.

> ⚠️ **Disclaimer:** This project is for educational purposes only. Predictions come from a machine learning model trained on a sample dataset and are **not a substitute for professional medical advice, diagnosis, or treatment.**

## Features

**Disease Prediction**
- Symptom-based disease prediction using a trained SVM classifier
- Returns disease description, precautions, medications, recommended diet, and workout suggestions
- Severity classification (low / medium / high) based on the predicted disease

**Patient Portal**
- Registration and login
- Book appointments with doctors by specialization
- Submit symptoms/complications and upload medical reports
- View appointment history and prescriptions

**Doctor Portal**
- Login and availability toggle
- View and approve booked appointments
- View patient-submitted descriptions and reports
- Issue prescriptions (disease, precautions, medication, diet, workout)

**Admin Portal**
- Add/remove doctors
- View all patients, doctors, appointments, and prescriptions

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Python, Flask |
| ML Model | scikit-learn (SVM), pickle |
| Data handling | pandas, numpy |
| Database | MySQL (via `flask-mysqldb`) |
| Frontend | HTML, CSS, Jinja2 templates |

## Project Structure

```
Medicine-Recommendation-System/
├── assets/           # Static assets (images, etc.)
├── datasets/         # CSVs used for lookups (symptoms, precautions, medications, diets, workouts, descriptions)
├── models/           # Trained model (svc.pkl)
├── static/           # CSS/JS/static files for templates
├── templates/         # HTML templates (homepage, login, dashboards, result page, etc.)
├── uploads/          # Uploaded patient report files
└── main.py           # Flask application (routes, ML inference, DB logic)
```

## How the Prediction Works

1. User selects symptoms on the frontend.
2. `main.py` converts the selected symptoms into a binary feature vector (132 known symptoms).
3. The vector is passed to a pre-trained SVM model (`models/svc.pkl`) to predict the disease.
4. The predicted disease is used to look up:
   - Description (`description.csv`)
   - Precautions (`precautions_df.csv`)
   - Medications (`medications.csv`)
   - Recommended diet (`diets.csv`)
   - Suggested workout (`workout_df.csv`)
5. A severity level (low/medium/high) is assigned from a hardcoded disease-severity mapping.
6. Results are rendered on the result page.

## Getting Started

### Prerequisites
- Python 3.8+
- MySQL Server

### 1. Clone the repository
```bash
git clone https://github.com/mdaabid093/Medicine-Recommendation-System.git
cd Medicine-Recommendation-System
```

### 2. Install dependencies
No `requirements.txt` is currently committed to the repo — install the packages used by `main.py` manually, or generate and commit a `requirements.txt` for future setups:
```bash
pip install flask flask-mysqldb numpy pandas scikit-learn mysqlclient
```

### 3. Set up the database
The app expects a MySQL database (default name in code: `medicine_recommendationss`) with these tables, inferred from the queries in `main.py`:
- `user_register`
- `patient_register`
- `add_doctor`
- `book_appointment`
- `patient_description`
- `prescription`
- `admin_register`

> No `.sql` schema file is currently included in the repo. You'll need to create these tables manually (matching the columns referenced in `main.py`) or add and commit a schema/migration script — this is the main blocker to a clean setup from a fresh clone.

### 4. Configure credentials
`main.py` currently hardcodes the secret key and DB credentials:
```python
app.secret_key = 'thisisthesecretkey'
app.config['MYSQL_HOST'] = 'localhost'
app.config['MYSQL_USER'] = 'root'
app.config['MYSQL_PASSWORD'] = '7861'
app.config['MYSQL_DB'] = 'medicine_recommendationss'
```
Before deploying or sharing this publicly, move these into environment variables (e.g. via `python-dotenv`) rather than committing them to source.

### 5. Run the app
```bash
python main.py
```
The app will be available at `http://localhost:5000/`.

## Known Issues / TODO
- [ ] Add a `requirements.txt`
- [ ] Add a database schema/migration script (`.sql` file)
- [ ] Move secrets/credentials to environment variables
- [ ] Turn off `debug=True` for production use
- [ ] Add a `.gitignore` (avoid committing user-uploaded files in `uploads/`)
- [ ] Add a LICENSE file
