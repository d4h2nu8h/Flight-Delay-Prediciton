# Flight Delay Prediction using Machine Learning and IBM Cloud

> A binary classification system that predicts whether a scheduled flight will arrive on time or be delayed, trained on US domestic flight data and deployed as a live web application via IBM Cloud.

---

## Overview

Flight delays are one of the most persistent operational challenges in commercial aviation, costing airlines, passengers, and airports billions of dollars annually. Beyond financial impact, delays cascade across networks — a single late departure can propagate through an entire day's schedule across multiple routes.

This project builds a supervised machine learning pipeline that predicts flight delay likelihood based on scheduling and routing features, enabling passengers and operations teams to anticipate disruptions ahead of time. The trained model is served through a Flask web application where users input flight details and receive an immediate on-time or delayed prediction.

---

## Dataset

**Source:** US Domestic Flight Data (`flightdata.csv`)

The dataset captures scheduled and operational flight records across five major US airports. Features include temporal scheduling attributes, origin-destination routing, and departure time differentials.

| Feature | Description |
|---|---|
| Flight Number | Unique identifier for the flight |
| Month | Month of travel (1–12) |
| Day of Month | Calendar day |
| Day of Week | Day index (1 = Monday, 7 = Sunday) |
| Origin | Departure airport (ATL, DTW, JFK, MSP, SEA) |
| Destination | Arrival airport (ATL, DTW, JFK, MSP, SEA) |
| Scheduled Departure Time | Planned departure (HHMM format) |
| Scheduled Arrival Time | Planned arrival (HHMM format) |
| Actual Departure Time | Recorded actual departure |
| Departure Delta | Difference between scheduled and actual departure |

**Target variable:** Binary — on time (0) or delayed (1)

---

## Methodology

### Data Preprocessing (`flightdelay.ipynb`)

- Computed a derived feature, departure delta, representing the gap between scheduled and actual departure time — a strong predictor of downstream arrival delay
- Applied one-hot encoding to categorical origin and destination airport fields, producing binary indicator columns for each of the five airports
- Handled missing values and normalised numerical features prior to training

### Model Training

The classification model was trained using scikit-learn and serialised as `flight.pkl` for use in production inference. The training pipeline explored multiple algorithms with cross-validated evaluation before selecting the final model.

### Web Application (`app.py`)

The prediction interface is built with Flask. User inputs are collected via a web form, preprocessed to match the training feature schema (including one-hot encoding of airports and computation of the departure delta), and passed to the loaded model for inference. The result is rendered back to the user on the same page.

### Deployment

The application is containerised and deployed on **IBM Cloud**, making predictions accessible via a public URL without any local setup.

---

## Results

The model successfully distinguishes between on-time and delayed flights based on scheduling features alone, without requiring real-time weather or ATC data. The departure delta feature — the difference between scheduled and actual departure time — proved to be the strongest individual predictor of arrival delay status.

> Full training metrics and feature importance analysis are available in `flightdelay.ipynb`.

---

## Limitations & Future Work

**Current Limitations:**

- Coverage is limited to five US airports; the model has not been validated on other routes or carriers
- Weather, air traffic control, and mechanical delay causes are not captured in the feature set, limiting predictive ceiling
- The departure delta feature requires knowledge of the actual departure time, which is not available at the time of booking — reducing utility for pre-flight prediction scenarios
- Binary classification does not quantify the expected magnitude of the delay

**Future Directions:**

- Incorporate real-time weather data and NOTAM feeds as additional features to improve accuracy
- Reframe as a regression problem to predict expected delay duration in minutes
- Expand airport and carrier coverage using the full Bureau of Transportation Statistics dataset
- Replace the departure delta with pre-departure proxy features (e.g., aircraft rotation history, route congestion scores) to enable genuine advance prediction

---

## How to Run This Project

### Prerequisites

```bash
Python 3.8+
```

### 1. Clone the Repository

```bash
git clone https://github.com/d4h2nu8h/flight-delay-prediction.git
cd flight-delay-prediction
```

### 2. Install Dependencies

```bash
pip install -r requirements.txt
```

### 3. Run the Flask Application Locally

```bash
cd flask
python app.py
```

Then open `http://localhost:5000` in your browser. Enter the flight details in the form and click Submit to receive a prediction.

### 4. IBM Cloud Deployment

The application is configured for deployment on IBM Cloud Foundry. To deploy your own instance, follow the IBM Cloud CLI deployment steps and push the contents of the `flask/` directory as the application root.

---

## Tech Stack

| Category | Tools |
|---|---|
| Language | Python 3.8+ |
| Machine Learning | Scikit-learn |
| Data Processing | Pandas, NumPy |
| Model Serialisation | Pickle |
| Web Framework | Flask |
| Frontend | HTML, CSS |
| Deployment | IBM Cloud |
| Notebook Environment | Jupyter Notebook |

---

## Authors

**Dhanush Sambasivam & Madhumitha**

[![GitHub](https://img.shields.io/badge/GitHub-d4h2nu8h-181717?style=flat&logo=github)](https://github.com/d4h2nu8h)

---

## License

This project is intended for academic and research purposes.
