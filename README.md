# F1 Lap Time Prediction Using LSTM Neural Networks

Predicting next-lap performance from raw Formula 1 lap timing data.

This project uses a Long Short-Term Memory (LSTM) model to predict the next lap time for a Formula 1 driver based on their previous 10 laps. It learns racing patterns such as pace evolution, tire degradation, and fuel load effects — using only historical lap times.

---

## Table of Contents
- Project Overview
- Dataset
- Preprocessing
- Model Architecture
- Training Configuration
- Results
- Visualizations
- How to Run
- Repository Structure
- Future Work
- Author

---

## Project Overview

Lap times form a sequential time series — each lap depends on the previous ones.  
This project reframes lap-time prediction as:

**"Given the last 10 laps, predict the next lap."**

LSTMs are ideal because they:
- Learn patterns over time  
- Handle sequences  
- Capture lap-to-lap relationships  

This provides a baseline predictive model using only raw lap-time data.

---

## Dataset

Data includes:
- raceId  
- driverId  
- lap number  
- lap time in milliseconds  

Only modern-era (2014–2024) races are used.

---

## Preprocessing

### Steps:

**1. Filtering**  
Only 2014–2024 races included.

**2. Sorting**  
Sorted by race → driver → lap.

**3. Sliding Windows**  
Each training sample:

- Input: 10 previous lap times  
- Target: next lap time  

**4. Scaling**  
Lap times standardized using `StandardScaler`  
(fit only on training set to avoid leakage)

---

## Model Architecture

Input: (10 timesteps, 1 feature)
↓
LSTM(64)
↓
Dropout(0.2)
↓
Dense(32, relu)
↓
Dense(1)

Purpose:
- LSTM learns temporal structure  
- Dropout reduces overfitting  
- Dense layers compress sequence representation  
- Output is a single predicted lap time  

---

## Training Configuration

| Setting | Value |
|--------|--------|
| Loss | MSE |
| Metric | MAE |
| Optimizer | Adam (lr=0.001) |
| Window Size | 10 |
| Epochs | 20 |
| Batch Size | 64 |
| Split | 70/15/15 train/val/test |

---

## Results

### Unscaled (real lap times)

| Metric | Value |
|--------|-------|
| Test MAE | ~4972 ms (~5 sec) |
| Test RMSE | ~43541 ms |
| Mean Error | ~256 ms |
| Std Dev | ~43025 ms |

Interpretation:
- Average error ≈ 5 seconds  
- Mean error near zero → predictions not biased  
- Large outliers caused by pit laps, SC/VSC laps, or very slow laps  

---

## Visualizations

Create a folder:

plots/

scss
Copy code

Place your images inside it. GitHub will display them automatically.

### 1. Training Curves

shell
Copy code

### 2. Actual vs Predicted Scatter

shell
Copy code

### 3. Residual Distribution

shell
Copy code

### 4. Actual vs Predicted Sequence (first 300 samples)

yaml
Copy code

---

## How to Run

### Clone
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>

shell
Copy code

### Install Dependencies
pip install -r requirements.txt

yaml
Copy code

### Run Notebook
Open the notebook inside `notebooks/` and run all cells.

Plots will save automatically to `plots/`.

---

## Repository Structure

.
├── data/
├── notebooks/
├── models/
├── plots/
├── requirements.txt
└── README.md

yaml
Copy code

---
