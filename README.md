# Classification Lab 3 – Model Evaluation and Recommendation

In this lab, I worked on predicting whether a customer case would be escalated using different classification models. The dataset had 900 records, with 186 escalated cases and 714 non-escalated cases. Since most cases were not escalated, I learned that accuracy alone is not enough to judge how well a model performs.

I tested four models: the majority-class baseline, Logistic Regression, Linear Discriminant Analysis (LDA), and K-Nearest Neighbors (KNN). Logistic Regression and LDA performed similarly, both achieving an accuracy of 0.822, precision of 0.750, recall of 0.196, and F1-score of 0.310. Logistic Regression had a slightly better ROC-AUC of 0.811. However, both models missed many actual escalations when using the default threshold of 0.50. KNN with k=15 performed worse than the other models.

I also tested different thresholds for Logistic Regression to see how changing the threshold affects the predictions. The threshold of 0.20 gave the lowest cost of 98 using the formula 5FN + FP. At this threshold, the model caught 36 out of 46 escalated cases, giving a recall of 0.783. However, it also produced 48 false positives. I think this is a reasonable trade-off because missing an escalation is considered five times more costly than a false alarm in this lab.

For KNN, I used stratified five-fold cross-validation to test k values of 5, 9, 15, 21, and 31. The best result was k=5, with a mean F1-score of 0.243. Even after tuning, its test performance was not very strong, especially its recall of 0.130 and ROC-AUC of 0.631.

Based on these results, I would choose Logistic Regression with a threshold of 0.20 as the model to consider first. It catches more escalated cases and has the lowest cost among the tested thresholds. Before using it in a real situation, I would test it on new data, monitor false alarms and missed escalations, and keep human review for important decisions.
