# Lab 4

## How to run

1. Open `lab4.ipynb` in Google Colab.
2. Upload `classification_escalation.csv` and `resolution_time_regression.csv`.
3. Run all cells from top to bottom.

## 1. The problem

- **Experiment A:** predict if a support ticket will be escalated (yes or no). Only about 21% are escalated.
- **Experiment B:** predict how many hours a ticket takes to resolve.

## 2. Preprocessing

- I removed `ticket_id` because it is only an ID.
- Numbers: I filled missing values with the median, then scaled them.
- Categories: I filled missing values with the most common value, then turned them into 0/1 columns.
- All of this is inside a pipeline.

## 3. Validation

I split the data into 80% training and 20% testing. On the training data I used 5-fold cross-validation.

- A: stratified folds, scored with F1.
- B: normal folds, scored with MAE (average error in hours).

## 4. Models and tuning

- A: logistic regression, k-NN and decision tree. I tuned logistic regression (`C`: 0.01, 0.1, 1, 10; `class_weight`: None, balanced).
- B: linear regression, ridge and decision tree. I tuned ridge (`alpha`: 0.01, 0.1, 1, 10, 100).

## 5. Cross-validation results

**A (higher F1 is better)**

| Model | Mean F1 | Std |
|---|---|---|
| Logistic regression | 0.393 | 0.063 |
| k-NN | 0.361 | 0.050 |
| Decision tree | 0.331 | 0.058 |
| Tuned logistic regression (C=0.1, balanced) | 0.520 | not recorded |

**B (lower MAE is better)**

| Model | Mean MAE | Std |
|---|---|---|
| Linear regression | 2.035 | 0.172 |
| Ridge | 2.035 | 0.171 |
| Decision tree | 3.269 | 0.144 |
| Tuned ridge (alpha=0.1) | 2.035 | 0.172 |

## 6. Test results

**A:** precision 0.52, recall 0.71, F1 0.60 for escalated tickets. Accuracy 0.80.

| | Predicted no | Predicted yes |
|---|---|---|
| **Actual no** | 117 | 25 |
| **Actual yes** | 11 | 27 |

- 25 false positives: flagged but not escalated. This wastes time.
- 11 false negatives: escalated but missed. I think this is worse because the customer may not get help in time.

**B:** MAE 2.23 hours, RMSE 3.40 hours.

## 7. Outliers, MAE and RMSE

Most tickets take about 5.4 hours (the median), but the longest takes 27.5 hours. Also, 72 tickets are exactly 0.5 hours, which looks like a minimum limit. RMSE punishes big mistakes more than MAE does. My RMSE is higher, so some errors are much bigger than others, probably on long tickets. Because of this, the two metrics can pick different models.

## 8. Preventing data leakage

- I split off the test data first.
- The pipeline learns only from the training part of each fold.
- I removed the ID column.
- I used each test set only once.

## 9. Final models

- **A: tuned logistic regression.** It was best before tuning, improved after tuning, and is easy to explain.
- **B: tuned ridge.** It tied with linear regression, beat the decision tree, and is simple.

## 10. Limitations

- The F1 score is still low, so the model makes many mistakes.
- The test set had only 38 escalated tickets.
- The time prediction is off by about 2 hours on average.
- I tuned only one model in each experiment.

## 11. Reflection questions

**1. Why stratify the classification split?**
So the training and test sets have about the same share of escalated tickets (about 21%).

**2. Why must preprocessing be inside the pipeline?**
So it learns only from training data. Otherwise test or validation information leaks in and the scores can look too good.

**3. What does fold variability tell you?**
It shows how stable the score is. In A, the scores vary more than the gaps between close models, so the ranking is not certain. In B, the results are fairly stable.

**4. Why should the test set be evaluated only once?**
It acts as new data. If I keep using it to make changes, the final score will be too optimistic.

**5. What deployment constraints might change your choice?**
Speed, memory, how easy the model is to explain, the cost of each type of mistake, and how often it needs retraining.
