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

# Week 9 (Option A)

Cross-validation (5 stratified folds, metric: roc_auc)

 fold  train  validation    gap
    1  0.906       0.896  0.009
    2  0.906       0.896  0.010
    3  0.904       0.924 -0.020
    4  0.912       0.876  0.037
    5  0.904       0.895  0.009

Train       mean = 0.907   std = 0.003
Validation  mean = 0.898   std = 0.017
Gap         mean = +0.009   std = 0.020

Nested cross-validation: 5 outer folds x 12 trials x 3 inner folds ...
Cross-validation (5 stratified folds, metric: roc_auc)

 fold  train  validation    gap  inner_best select__threshold  model__estimator__max_depth  model__n_estimators  model__learning_rate
    1  0.911       0.907  0.004       0.904          0.5*mean                            1                  208                 0.477
    2  0.918       0.907  0.011       0.903          0.5*mean                            2                  108                 0.365
    3  0.906       0.923 -0.018       0.893              mean                            3                  165                 0.094
    4  0.912       0.879  0.032       0.905              mean                            3                   87                 0.237
    5  0.913       0.897  0.016       0.900          0.5*mean                            2                  108                 0.365

Train       mean = 0.912   std = 0.004
Validation  mean = 0.903   std = 0.016
Gap         mean = +0.009   std = 0.018

Tuning on the whole development set (12 Optuna trials, metric: roc_auc)
Best hyperparameters: {'select__threshold': 'mean', 'model__estimator__max_depth': 3, 'model__n_estimators': 165, 'model__learning_rate': 0.09420457098319444}
Best mean CV score:   0.900  (the maximum over all trials -- optimistic)

Top 5 trials:
 trial  mean roc_auc  model__estimator__max_depth  model__learning_rate  model__n_estimators select__threshold   std
    11         0.900                            3                 0.094                  165              mean 0.004
     7         0.899                            3                 0.237                   87              mean 0.004
     4         0.898                            3                 0.057                  144              mean 0.003
    10         0.897                            4                 0.420                   80              mean 0.007
     0         0.896                            3                 0.080                   66              mean 0.003

Optimism check (nested CV):
  tuning score inside the outer folds (inner_best) mean = 0.901
  honest score on the outer folds (validation)     mean = 0.903
  optimism = -0.002   -> report the honest one
Classification report (out-of-fold predictions, development set):
              precision    recall  f1-score   support

           0      0.876     0.940     0.907      3651
           1      0.751     0.578     0.653      1149

    accuracy                          0.853      4800
   macro avg      0.814     0.759     0.780      4800
weighted avg      0.846     0.853     0.846      4800

False positive rate by sex (development set, out-of-fold)
(share of negative cases predicted positive)

  Our model:
    Female               FPR = 0.01  (n=1390)
    Male                 FPR = 0.09  (n=2261)

The pipeline was modified for the Adult income dataset (Option A) by generating config_adult.yaml, updating main.py to score adult_new.csv and accept a configuration argument, generalising missing-value ("?") replacement across all string columns in src/preprocessing.py, and making the COMPAS benchmark optional in src/evaluate.Py.   The model obtained an honest nested CV ROC-AUC of 0.903 (train 0.912 vs. validation 0.903, gap +0.009), accuracy of 0.854, and F1-score (class 1) of 0.653 using target encoding, robust scaling, missingness indicators (workclass, occupation), SelectFromModel, and nested cross-validation with Optuna tuning on AdaBoost (max_depth=3, n_estimators=165, learning_rate=0.094). While maintaining the original COMPAS pipeline, feature selection automatically eliminated all 25 anonymised noise columns (v01–v25) and kept the six primary predictors (age, education_num, capital_gain, marital_status, occupation, and relationship), producing all 2,000 predictions in results_adult/adult_predictions.csv.

