# Student Performance Factors

CS 334 final project (Group 17). We analyze which academic, behavioral, socio-economic and environmental factors affect secondary school students' exam scores, and build models to predict those scores.

- **View the notebook online:** [umerasghar-bit.github.io/student-performance-factors](https://umerasghar-bit.github.io/student-performance-factors/CS334_Project_Jupyter_Notebook.html)
- **Blog post:** [Analyzing Factors Influencing Student Performance: A Data-Driven Approach](https://medium.com/@umerasghar6754/analyzing-factors-influencing-student-performance-a-data-driven-approach-360fc2495fc4)
- **Notebook:** [CS334_Project_Jupyter_Notebook.ipynb](CS334_Project_Jupyter_Notebook.ipynb) (a static [HTML export](CS334_Project_Jupyter_Notebook.html) is also included)

## Research Questions

### 1. Impact of Parental and Socio-Economic Factors on Exam Performance

- **Research Question:** What is the effect of parental involvement, access to resources, family income level, and parental education level on student exam performance (and, by proxy, overall student performance)?
- **Objective:** This question aims to explore how family support and socio-economic factors influence academic success. By analyzing these variables, we seek to understand the role of a student's home environment and resources in shaping educational outcomes. Insights could inform policies and practices to provide support where needed, addressing educational disparities.

### 2. Combined Influence of Academic, Behavioral, and Environmental Factors on Exam Scores

- **Research Question:** How do various factors, including hours studied, attendance, parental involvement, access to resources, extracurricular activities, sleep hours, previous academic performance, motivation level, internet access, tutoring sessions, family income, teacher quality, school type, peer influence, physical activity, learning disabilities, parental education level, distance from home, and gender, collectively impact students' exam scores (and by proxy, overall student performance)?
- **Objective:** This question broadens the scope to examine a holistic view of student performance, assessing how a combination of academic, behavioral, and environmental factors interact to affect outcomes. Understanding these interactions can help build predictive models for academic success and identify key areas for targeted interventions that support balanced student growth.

## Data

[secondary_dataset.csv](secondary_dataset.csv) contains 6,607 students with 19 features and their exam score. After cleaning, the analysis uses **6,377 students**:

- 229 rows with missing values (in Teacher_Quality, Parental_Education_Level and Distance_from_Home) were dropped (3.5% of the data).
- 1 row with an invalid exam score of 101 (on a 100-point scale) was removed.

## Methods

- Exploratory analysis: distributions, count plots, correlations, scatter plots, box plots and IQR outlier detection.
- OLS regression (statsmodels) for each research question, with Variance Inflation Factors to check for multicollinearity.
- Random Forest and linear regression models (scikit-learn), compared on the same 70/30 train/test split and with 5-fold cross-validation.
- Interactive ipywidgets forms that predict an exam score from user inputs (these need a running Jupyter kernel).

## Key Results

### Research Question 1: socio-economic factors

All four factors have statistically significant effects (p ≤ 0.001) in the OLS model, but the effects are small:

| Factor (compared with baseline) | Effect on exam score |
|---|---|
| Low parental involvement (vs. High) | −1.82 points |
| Low access to resources (vs. High) | −1.95 points |
| Low family income (vs. High) | −0.95 points |
| Postgraduate parental education (vs. College) | +0.64 points |
| High school parental education (vs. College) | −0.47 points |

| Model | Test MAE | Test R² | 5-fold CV R² |
|---|---|---|---|
| Linear Regression | 2.68 | 0.075 | 0.073 |
| Random Forest | 2.69 | 0.061 | 0.062 |

Together these four factors explain only about 7% of the variation in exam scores.

### Research Question 2: all factors combined

- The OLS model explains **72.6%** of the variation in exam scores (in-sample R² = 0.726).
- **Attendance** (+0.20 points per percentage point) and **hours studied** (+0.29 points per weekly hour) are the strongest predictors by correlation (r = 0.58 and 0.45), t-statistic and Random Forest feature importance.
- Other significant effects include tutoring sessions (+0.49 per monthly session), low access to resources (−2.06), low parental involvement (−2.00), low motivation (−1.08), low family income (−1.06), low teacher quality (−1.04), positive peer influence (+1.02) and internet access (+0.98).
- Sleep hours, school type and gender have **no significant effect**.
- All Variance Inflation Factors are below 3, so multicollinearity is not a concern.

| Model | Test MAE | Test R² | 5-fold CV R² |
|---|---|---|---|
| **Linear Regression** | **0.46** | **0.773** | **0.725** |
| Random Forest | 1.21 | 0.645 | 0.610 |

Linear regression outperforms the Random Forest: 98.4% of its test predictions are within 1 point of the actual score. Most of the remaining error comes from 11 of the 1,914 test students, all of whom scored much higher than predicted; excluding them, R² is 0.985.

### Limitations

The data is observational, so these results show associations rather than causal effects. Several factors (motivation, teacher quality, peer influence) are self-reported or perceived ratings, and the regression residuals are non-normal because of a small number of unexplained high scores.

## Running the Notebook

```bash
pip install -r requirements.txt
jupyter notebook CS334_Project_Jupyter_Notebook.ipynb
```

The notebook expects `secondary_dataset.csv` in the same folder.

## Repository Contents

| File | Description |
|---|---|
| `CS334_Project_Jupyter_Notebook.ipynb` | Full analysis notebook |
| `CS334_Project_Jupyter_Notebook.html` | Static HTML export of the notebook |
| `secondary_dataset.csv` | Student performance dataset |
| `Research Qs & Blog Post.pdf` | Research questions and blog post link |
| `requirements.txt` | Python dependencies |
