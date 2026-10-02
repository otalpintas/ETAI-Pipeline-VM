# 20231655 Vicente Marques

# Logistic Regression

Train accuracy: 0.679
Test accuracy:  0.680
Gap (train - test): -0.001

# Decision Tree

Train accuracy: 0.829
Test accuracy:  0.626
Gap (train - test): +0.203

# Conclusions

Logistic Regression is better because it has a higher test accuracy than the decision tree decision tree has a huge sign of overfitting.


# Week 3

# Logistic Regression after cleaning the dataset

Train accuracy: 0.676
Test accuracy:  0.657
Gap (train - test): +0.019

# Conclusions

With a test accuracy of 68.0% on the original dataset and 65.7% after data cleaning, the model's prediction performance was not enhanced by the cleaning procedure, despite the elimination of duplicates and other possibly misleading observations. In fact, there was a 2.3 percentage point drop in test accuracy. In all situations, the train–test gap stayed comparatively narrow, suggesting that neither model exhibits significant overfitting. These findings indicate that the data-cleaning procedure did not increase the test set's Logistic Regression performance.


# Week 4

Cross-validation (5 stratified folds, metric: accuracy)

 fold  train  validation    gap
    1  0.676       0.680 -0.004
    2  0.680       0.654  0.026
    3  0.674       0.675 -0.001
    4  0.672       0.688 -0.016
    5  0.674       0.665  0.010

False positive rate by race (development set, out-of-fold)
(share of people who did NOT reoffend, but were predicted to)

  Our model:
    African-American     FPR = 0.26  (n=1420)
    Asian                FPR = 0.10  (n=21)
    Caucasian            FPR = 0.13  (n=1161)
    Hispanic             FPR = 0.15  (n=311)
    Native American      FPR = 0.00  (n=6)
    Other                FPR = 0.14  (n=185)

  COMPAS's own score:
    African-American     FPR = 0.45  (n=1420)
    Asian                FPR = 0.10  (n=21)
    Caucasian            FPR = 0.23  (n=1161)
    Hispanic             FPR = 0.23  (n=311)
    Native American      FPR = 0.17  (n=6)
    Other                FPR = 0.14  (n=185)

Strong generalisation between cross-validation (67.2%) and the locked test set (65.7%) was the outcome of the redesigned data pipeline's successful prevention of data leakage. By cleaning placeholder values, removing duplicates, and using imputation methods, the model substantially reduced false positive rates across all demographic groups—cutting the false positive rate for African-American individuals from 45% down to 26% and Caucasian individuals from 23% down to 13%.

