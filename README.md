# Fitbit Sleep Quality

This repository contains a small data science project that uses wearable (Fitbit-style) daily data to estimate whether a user is likely to have a **good sleep** on a given night.

The idea:  
> take today's daytime behavior (steps, activity minutes, sedentary time, calories, etc.) → predict whether tonight's sleep will meet a “good sleep” standard.

---

## 1. Data

The notebook expects the Fitbit-style CSV files you downloaded locally.  
Place them in the **same folder** as the notebook:

- `dailyActivity_merged.csv`
- `dailyCalories_merged.csv`
- `sleepDay_merged.csv`
- `heartrate_seconds_merged.csv` or `heartrate_seconds_merged.csv.zip`

These files are **not** committed to the repo to keep it small.  
If someone else wants to run the notebook, they need to download the same CSVs and put them in the project root.

---

## 2. What the notebook does

File: **`fitbit_sleep.ipynb`**

Main steps inside the notebook:

1. **Load & clean data**  
   - unify date columns to one column called `Date`
   - aggregate heart-rate seconds into **daily average heart rate** per user (`AvgHeartRate`)
2. **Merge** activity, calories, sleep, and heart rate into one table (`full_df`)
3. **Create a sleep-quality label**  
   - `GoodSleep = 1` if  
     - `TotalMinutesAsleep >= 420` (7 hours) **and**  
     - `SleepEfficiency >= 85%`  
   - else `GoodSleep = 0`
4. **Explore** differences between good-sleep vs not-good-sleep days
5. **Modeling**: logistic regression  
   - baseline features: steps, different activity minutes, sedentary minutes, calories  
   - compare model **with** vs **without** daily average heart rate
   - evaluate in a **leave-one-user-out** fashion (train on all other users, test on this user)
6. **Sensitivity / feature importance**  
   - standardize features
   - plot logistic regression coefficients to see which daytime behaviors matter more

---

## 3. Project structure

```text
fitbit_project/
├── fitbit_sleep.ipynb   # main notebook with data prep, modeling, plots
├── .gitignore           # ignore large data files (csv, zip, checkpoints)
└── (CSV files go here locally, but are not tracked)
