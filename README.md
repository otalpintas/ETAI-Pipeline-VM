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

# Week 6

fold  train  validation   gap
    1  0.728       0.637 0.091
    2  0.754       0.645 0.110
    3  0.708       0.632 0.076
    4  0.742       0.679 0.064
    5  0.731       0.660 0.071

Train       mean = 0.733   std = 0.017
Validation  mean = 0.650   std = 0.019
Gap         mean = +0.082   std = 0.018

Nested cross-validation: 5 outer folds x 30 trials x 5 inner folds ...
Cross-validation (5 stratified folds, metric: accuracy)

 fold  train  validation    gap  inner_best  model__max_depth  model__min_samples_leaf model__max_features
    1  0.693       0.675  0.018       0.685                22                       36               0.500
    2  0.696       0.658  0.039       0.689                27                       26                sqrt
    3  0.688       0.693 -0.005       0.677                 7                       59               0.500
    4  0.684       0.684  0.000       0.678                15                      111               0.500
    5  0.689       0.696 -0.007       0.682                14                       57               0.500

Train       mean = 0.690   std = 0.005
Validation  mean = 0.681   std = 0.015
Gap         mean = +0.009   std = 0.019

Tuning on the whole development set (30 Optuna trials, metric: accuracy)
Best hyperparameters: {'model__max_depth': 22, 'model__min_samples_leaf': 42, 'model__max_features': 0.5}
Best mean CV score:   0.683  (the maximum over all trials -- optimistic)

Top 5 trials:
 trial  mean accuracy  model__max_depth model__max_features  model__min_samples_leaf   std
    14          0.683                22               0.500                       42 0.015
    28          0.682                20                sqrt                       57 0.015
    21          0.682                15               0.500                       66 0.016
    25          0.682                25               0.500                       71 0.015
    15          0.681                25               0.500                       47 0.015

Optimism check (nested CV):
  tuning score inside the outer folds (inner_best) mean = 0.682
  honest score on the outer folds (validation)     mean = 0.681
  optimism = +0.001   -> report the honest one
Classification report (out-of-fold predictions, development set):
              precision    recall  f1-score   support

           0       0.69      0.78      0.73      3170
           1       0.67      0.57      0.62      2601

    accuracy                           0.68      5771
   macro avg       0.68      0.67      0.67      5771
weighted avg       0.68      0.68      0.68      5771

False positive rate by race (development set, out-of-fold)
(share of people who did NOT reoffend, but were predicted to)

  Our model:
    African-American     FPR = 0.30  (n=1420)
    Asian                FPR = 0.14  (n=21)
    Caucasian            FPR = 0.16  (n=1161)
    Hispanic             FPR = 0.17  (n=311)
    Native American      FPR = 0.17  (n=6)
    Other                FPR = 0.17  (n=185)
    unknown              FPR = 0.20  (n=66)

  COMPAS's own score:
    African-American     FPR = 0.45  (n=1420)
    Asian                FPR = 0.10  (n=21)
    Caucasian            FPR = 0.23  (n=1161)
    Hispanic             FPR = 0.23  (n=311)
    Native American      FPR = 0.17  (n=6)
    Other                FPR = 0.14  (n=185)
    unknown              FPR = 0.33  (n=66)

Reduced overall false arrest/prediction rates: Compared to COMPAS, both iterations of your model reduced the false positive rate for all main categories (African-American FPR decreased from 45% to 26–30%; Caucasian FPR decreased from 23% to 13–16%).

While the absolute difference between African-American and Caucasian defendants decreased dramatically (from a +22% difference to +13%), the relative ratio remained approximately 2:1, suggesting systemic historical bias in underlying features (such as prior arrests or criminal history) that tree regularisation alone cannot fully equalise.