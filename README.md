# hc_ds Short Report

## Overview
Data includes loan applications from 2014-06-20 to 2015-05-31.
Target of prediction: Ability to repay the loan back of the applications (values: 0 and 1).

Number of application is quite stable over time.
![image](https://github.com/duongtruongtrong/hc_ds/assets/71629218/ef4fbf1f-454e-4d36-a2f6-8e81fd7bb71c)

## Data Exploration
### Data Distribution
Categorical features are distributed quite evenly.
![image](https://github.com/duongtruongtrong/hc_ds/assets/71629218/7849a470-789c-4782-9bbe-60234eab6e1e)

Most of numerical features have normal distribution.
![image](https://github.com/duongtruongtrong/hc_ds/assets/71629218/29314451-46b0-47e4-93b7-99d825065401)

However, NUMERICAL_10 and NUMERICAL_40 are bias on 0 values (after fill null with 0).
![numerical_10_distribution](https://github.com/duongtruongtrong/hc_ds/assets/71629218/268813d3-8e47-4fad-9045-11684b2833fe)![numerical_40_distribution](https://github.com/duongtruongtrong/hc_ds/assets/71629218/09e6f04c-7c7e-4466-beb9-7b77672fe69c)

### Missing Data
June 2014 data is not completed.

![image](https://github.com/duongtruongtrong/hc_ds/assets/71629218/00c11732-ae3b-4103-abcc-bb75c7e6c782)

NUMERICAL_10 has null value in a long period of time. If filling null with any constaent, it might lead to big bias for this period of time in prediction.

![image](https://github.com/duongtruongtrong/hc_ds/assets/71629218/23654226-192f-4bff-a802-1943bd9a3b1c)

### Data Exploration Conclusion
- Remove NUMERICAL_10 from feature list.
- NUMERICAL_40 fill null with mean value of the last 30 days (could be another time range, but with limited time for testing).
- June 2014 is not too importance, can be use for mean and standard deviation aggregation.

## Feature Engineering
### Filling null
Numerical features: Fill null with mean value of the last 30 days since application date.

Categorical features: Fill null with "unknown".

### Date Features
Extract these features from TIME column:
- Months Columns: month_1, month_2...
- Dates Columns: date_1, date_2...
- DOW Columns: dow_0, dow_1...
- Hour: hour_1, hour_2...

### Standard Deviation Features
Calculate standard deviation of numerical features using mean of the last 30 days since application date.

## Machine Learning Model

Chosen 2 algorithms: LogisticRegression and RandomForestClassifier
### LogisticRegression
LogisticRegression is a data analysis technique that uses mathematics to find the relationships between two data factors. It then uses this relationship to predict the value of one of those factors based on the other. The prediction usually has a finite number of outcomes, like yes or no.

This algorithm is fast in training and prediction because its complexity is low.
However, accuracy rate of LogisticRegression is not normally high comparing to other algorithms for complex input data (bit amount of features).

### RandomForestClassifier
RandomForestClassifier is a set of binary trees taking their majority vote for classification.

Training and prediction speed is quite slow, depending on the complexity of algorithm settings. The more trees or the more depth, the slower the algorithm runs.
Exchange of its speed, the accuracy rate is noemally higher than LogisticRegression for high complexity data.

### Comparison
|                 | LogisticRegression                       | RandomForestClassifier                    |
|-----------------|------------------------------------------|-------------------------------------------|
| Speed           | Fast                                     | Slow                                      |
| Size on Disk    | Small                                    | Big                                       |
| Accuracy Rate   | Normally low for high complexity data    | Normally high for high complexity data    |
| Complexity Data | Normally handle well low complexity data | Normally handle well high complexity data |

## Model Performance
### ROC_AUC Score
ROC_AUC score of 2 models are quite high in training.
![image](https://github.com/duongtruongtrong/hc_ds/assets/71629218/048a0338-68a0-4697-8e4c-42cf0546f9ce)

ROC_AUC score of 2 models are low in test -> 2 models are heavily overfitted.
![image](https://github.com/duongtruongtrong/hc_ds/assets/71629218/8026c976-3d38-4a2f-994f-13bf20a8403f)

Tunning hyper-parameters of the models will not reduce considerably overfit.

Reducing data complexity might be benificial more.

### Features vs TARGET Relationship
Since both models have the similar ROC_AUC score, considering feature importance of both models are ok for reducing overfit in the future.

LogisticRegression depends more on numberical than categorical features.
Highlights:
- NUMERICAL_4, NUMERICAL_7, NUMERICAL_20_std_dev_last_30_days and NUMERICAL_18_std_dev_last_30_days affect TARGET highly negatively.
- NUMERICAL_4_std_dev_last_30_days, NUMERICAL_7_std_dev_last_30_days, NUMERICAL_20 and NUMERICAL_18 affect TARGET highly positively.
- Original numberical features and their standard deviation features affect TARGET oppositely.

![image](https://github.com/duongtruongtrong/hc_ds/assets/71629218/7ac24d90-d5cd-4be6-91e0-19c1ad24ff0e)

RandomForestClassifier depends more on categorical than numberical features.
Highlights:
- CATEGORICAL_9, CATEGORICAL_7 and CATEGORICAL_1 highly affect TARGET. Around 60% of the predictions were based on these features.
- If original numberical features highly affect TARGET, their standard deviation features will affect TARGET as well.

![image](https://github.com/duongtruongtrong/hc_ds/assets/71629218/7b906781-3762-42b9-a658-f7ecc01b1cea)

Those highly affected features might be considered in reduce data complexity, basing on their real meaning, those features can be excluded in training.

## Conclusion
- Input data is quite clean.
- New features added did not show significant difference comparing to original features.
- Even though training ROC_AUC scores are high, but both models are very overfitted.
- Reducing data complexity might be the best way to reduce overfit.
- The following features can be considered for feature reduction:
  - NUMERICAL_4
  - NUMERICAL_7
  - NUMERICAL_20
  - NUMERICAL_18
  - CATEGORICAL_9
  - CATEGORICAL_7
  - CATEGORICAL_1
