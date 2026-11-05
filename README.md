# Diabetes Risk Screening Model

Can everyday health information flag who should get a diabetes blood test?

## Data
[Diabetes Health Indicators Dataset](https://www.kaggle.com/datasets/alexteboul/diabetes-health-indicators-dataset) on Kaggle, built from the CDC's BRFSS 2015 survey. About 250,000 respondents and 21 health and lifestyle indicators, like blood pressure, BMI, and age. About 14% were diabetic.

## Goal
A screening model: something that could flag people who should get a blood test. That meant catching as many diabetics as possible, not just getting a high accuracy score.

## What happened
**The first model looked fine on paper but caught under 9% of diabetics.** Since 86% of the data was non-diabetic, it was mostly predicting "no."

**So I rebuilt the evaluation.** I split the data first, balanced only the training set, and kept the test set at real-world proportions. That gave me honest numbers: 75% of diabetics caught, but only 29% precision.

**Then I investigated the errors instead of just reporting them.** The false positives looked like diabetics on the main risk factors; 86% had high blood pressure, versus 75% of actual diabetics. They were also about four times as likely to be prediabetic. The missed diabetics were the opposite: diabetics with low-risk profiles. That told me the limit: the features measure risk, not the disease itself.

**Based on that, I added health-status features** like self-rated general health, which became the strongest predictor.

**Finally, I tested decision thresholds** on the training data and chose 0.4, weighing missed diabetics against unnecessary blood tests.

## Results (unseen test data)
- Catches **85%** of diabetics
- When it says someone isn't diabetic, it's right **96%** of the time
- The cost: about 44% of people get flagged for a blood test, and 27% of those flagged are diabetic

## Limitations
- Diabetes here is self-reported diagnosis, so undiagnosed diabetics are labeled non-diabetic.
- No blood sugar measurement, so the model sees risk factors, not the disease.
- Some features overlap (general health, physical health days, difficulty walking), so their individual coefficients shouldn't be read alone.

## Files
- `analysis.ipynb`: full analysis, start to finish
- `diabetes_012_health_indicators_BRFSS2015.csv`: dataset
