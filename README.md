# Premier League Score Predictor

Predict the outcome (win vs. non-win) of Premier League matches using historical data and machine learning.

---

## Table of Contents

1. [Project Overview](#project-overview)  
2. [Features](#features)  
3. [Data Collection & Processing](#data-collection--processing)  
4. [Modeling](#modeling)  
5. [Results](#results)  
6. [Usage](#usage)  
7. [Repository Structure](#repository-structure)  
8. [Future Work](#future-work)  
9. [Environment & Installation](#environment--installation)  
10. [License](#license)  

---

## Project Overview

This project scrapes match-level statistics for all Premier League seasons, builds rolling statistics per team, and trains a Random Forest classifier to predict whether a given match will result in a win for the “home” team.

---

## Features

- **Web scraping** of match tables from FBref.  
- **Feature engineering**:  
  - Categorical encoding: venue, opponent.  
  - Temporal features: match kick-off hour, day of week.  
  - Rolling averages (last 3 games) of attacking/defensive stats:  
    - Goals for (`gf`), goals against (`ga`)  
    - Shots (`sh`), shots on target (`sot`)  
    - Distance covered (`dist`), fouls conceded (`fk`)  
    - Penalty kicks (`pk`), penalty kick attempts (`pkatt`)  
- **Model**: RandomForestClassifier.  
- **Evaluation** on hold-out seasons (post Jan 1, 2022) with accuracy and precision.

---

## Data Collection & Processing

1. **Scraping**  
   Run `scraping.ipynb` to pull match URLs and detailed stats tables from FBref.  
2. **Raw data**  
   Output stored in `matches.csv`.  
3. **Preprocessing**  
   - Drop unused columns (`comp`, `notes`).  
   - Convert dates and times to `datetime` and numeric features.  
   - Encode categorical variables.  
   - Generate rolling averages with a 3-match look-back.

---

## Modeling

- **Train/Test Split**:  
  - **Train**: matches before 2022-01-01  
  - **Test**: matches after 2022-01-01  
- **Algorithm**:  
  ```python
  from sklearn.ensemble import RandomForestClassifier
  rf = RandomForestClassifier(
      n_estimators=50,
      min_samples_split=10,
      random_state=1
  )
  rf.fit(X_train, y_train)

