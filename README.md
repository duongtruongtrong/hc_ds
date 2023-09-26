# hc_ds Short Report

## Overview
Data includes loan applications from 2014-06-20 to 2015-05-31.
Target of prediction: Ability to repay the loan back of the applications (values: 0 and 1).
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

• Explanatory analysis of features and their relation to target
• Brief description of the used algorithms and their pros and cons
• Performance evaluation of the model on the last month (May 2015), which would
be used as holdout testing data. Please include AUC-ROC among your evaluation
metrics
• Short conclusion and recommendatio
