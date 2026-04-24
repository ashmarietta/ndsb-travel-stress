# Travel Fatigue and Performance in Collegiate Softball
## Overview
This project analyzes how travel fatigue impacts physical performance in NCAA Division I softball student-athletes at the University of Notre Dame. Using force plate jump data and constructed travel stress metrics, the goal is to identify patterns of fatigue and classify athletes based on their sensitivity to travel.

The analysis focuses on two key objectives:
- Clustering athletes into fatigue-response groups based on force plate performance variation
- Predicting cluster membership using travel-related variables

---
## Data Sources
### 1. Jump Performance Data
- Variables:
  - Athlete ID
  - Peak Power
  - Z-Score Peak Power
  - Date
- Timeframe:
  - Pre-season: August 1, 2025 – February 4, 2026 (623 jumps)
  - In-season: February 5, 2026 – April 18, 2026 (328 jumps)

Z-score peak power is used as the primary outcome metric to normalize performance across athletes.

---
### 2. Travel Data
Travel metrics were engineered into several stress-based features:
- Circadian Score  
  Based on time zones crossed + travel direction (eastward weighted more heavily)
- Travel Load 
  Total distance + travel time (flight + drive)
- Logistics Score 
  Transit type (commercial vs. charter) + trip length
- Density Score 
  Return time to campus + turnaround time before next trip
- Trip Score
  Composite of all travel-related metrics
- RTL (Residual Travel Load)
  Daily fatigue score after travel
- CRTL (Cumulative RTL)
  Running total for fatigue across the season

---
## Methodology
### Baseline Construction
- Pre-season jump data was used to establish each athlete’s baseline peak power
- In-season performance was evaluated as deviation from this baseline (z-score)

---
### Feature Engineering
Derived variables included:
- Days since travel (restricted to 0–7 days post-travel)
- Flags for:
  - Negative z-scores
  - Z-score < -0.5
  - High RTL / CRTL / Trip Score
- Buckets for days post-travel

---
### Clustering
Athletes were grouped into five fatigue-response clusters based on lowest z-score and percentage of low-performance days

Clusters:
1. Highly-Sensitive  
2. Sensitive  
3. Moderate-Response  
4. Resilient  
5. Highly-Resilient  

Clustering was performed using athlete-level features only, excluding team-level variables.

---
### Modeling Approach
#### Data Split
- Athlete-level split due to limited season length
- Leave-One-Out Cross-Validation (LOOCV) used to prevent overfitting

---
### Models Used
#### 1. Random Forest (Baseline)
- 300 estimators
- Maximum depth: 5
- Minimum samples per leaf: 2
- Two versions:
  - With RTL + CRTL
  - Without RTL + CRTL

Purpose: Capture nonlinear relationships with minimal tuning

---
#### 2. XGBoost (Enhanced Model)
- 200 estimators
- Maximum depth: 3
- Learning rate: 0.05
- Subsample: 0.8
- Two versions:
  - With RTL + CRTL
  - Without RTL + CRTL

Purpose:
- Improve predictions by focusing on poorly-predicted athletes
- Incorporate regularization to reduce overfitting

---
## Key Findings

- Commercial return flights were the strongest predictor of performance decline
- Immediate travel conditions mattered more than cumulative metrics
- RTL and CRTL did not significantly improve model performance
- Only five out of 21 athletes were classified as highly-resilient

These results suggest that short-term recovery disruption plays a larger role than total travel accumulation.

---
## Practical Implications
### For Coaches
- Use highly-resilient athletes as prototypes for training and recovery habits
- Adjust training loads for fatigue-sensitive players
- Monitor post-travel performance more closely

### For Administrators
- Consider prioritizing charter flights for return travel
- Reduce emphasis on:
  - Time zones crossed
  - Charter flights before competition
  - Travel direction
  - Total distance
- Focus instead on recovery time after travel

---
## Limitations
- Small sample size (21 athletes)
- Jump performance influenced by external factors:
  - Academic stress
  - Training load
  - Effort variability
- Model with directional insights, not precise predictions

---
## Future Work

- Develop athlete-specific models
- Focus more on conference-specific travel
- Incorporate additional variables:
  - Academic workload
  - Opponent strength
- Add injury flags to control for confounding factors
- Extend framework to other sports (ex: baseball)

---
## Repository Structure
- Quarto file containing full code (`sb_analysis.qmd`)
- HTML file containing code and output (`sb_analysis.html`)
- Dataset with travel information (`project_data.xlsx`)
- Report with findings (`04.30_final_report.docx`)
- Presentation with findings (`04.30_final_report.docx`)

---
## Conclusion
This project establishes a framework for analyzing travel fatigue in collegiate athletics. While predictive performance is limited, the results highlight the importance of recovery-focused travel decisions and provide a foundation for more individualized performance modeling.
