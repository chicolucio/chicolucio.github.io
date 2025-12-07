---
name: Predicting Power Grid Failures with Weather Data
title: Predicting Power Grid Failures with Weather Data
image: /assets/images/portfolio_climatempo.webp
position: 0
period: 2025
toc: false
mathjax: true
header:
  image: /assets/images/portfolio_climatempo_header.png
  og_image: /assets/images/portfolio_climatempo.webp
description: |
  As a Data Scientist at NECTO Systems, I worked on a project for **Climatempo**,
  the leading meteorology company in Brazil. The main goal was to build a
  predictive model to estimate the number of occurrences (failures) in the
  electrical power grid caused by severe weather events, such as heavy rain, wind
  gusts, and lightning.
---

As a Data Scientist at NECTO Systems, I worked on a project for **Climatempo**,
the leading meteorology company in Brazil. The main goal was to build a
predictive model to estimate the number of occurrences (failures) in the
electrical power grid caused by severe weather events, such as heavy rain, wind
gusts, and lightning.

### The Challenge

The relationship between weather and power grid infrastructure is critical for
energy companies. Accurately predicting the volume of failures allows for better
resource allocation and faster response times.

However, this presented a significant modeling challenge: the target variable
was **count data** (non-negative integers) with a highly skewed distribution.
Most days have zero or very few incidents, while severe storms can cause a
sudden spike in failures (extreme values). Standard regression approaches
(optimizing for MSE) often failed to capture this behavior, predicting negative
values or underestimating the peaks.

### The Solution

To address this, I developed a complete Machine Learning pipeline using
**Python**. The process involved:

- **Data Engineering:** merging historical meteorological data with occurrence logs, 
performing extensive cleaning, and handling time-series constraints (ensuring no data 
leakage from future events).
- **Model Selection:** After an extensive screening phase comparing Linear Models, 
Random Forests, and Gradient Boosting methods, the final solution relied on a 
**LightGBM Regressor** configured with a **Poisson objective function**.
- **Optimization:** The Poisson loss function was crucial as it is specifically 
designed for count data, allowing the model to naturally handle the non-negative 
nature of the target and its variance.

The final model was evaluated using Time Series Cross-Validation, ensuring robustness across different seasons and weather patterns.

<figure style="width:80%">
<img src="/assets/images/climatempo_methodology.png" alt="climatempo"/>
<figcaption>Workflow of the regression methodology adopted for the final solution, from data processing to model validation</figcaption>
</figure>

### Technologies Used

- **Language:** Python
- **Libraries:** Pandas, Scikit-learn, LightGBM, XGBoost
- **Techniques:** Feature Engineering, Time Series Cross-Validation, Poisson Regression, Hyperparameter Tuning.
