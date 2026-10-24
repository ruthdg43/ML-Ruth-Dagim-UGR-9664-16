# Machine Learning Lab 3

## About the Project
This project is about predicting how many hours it takes to resolve a support ticket using machine learning.

I tried different regression models to see which one could make the best predictions.

## Dataset
The dataset contains information about support tickets.

For this project, I used:
- complexity_score: How complex the ticket is.
- team_experience_years: How many years of experience the team has.
- resolution_hours: The number of hours needed to resolve the ticket. This is what the models predict.

I did not use ticket_id because it is only an identifier. I also left out lines_changed and reopened because they might contain information that would not be available when making a prediction.

## What I Did
1. Loaded and explored the dataset.
2. Selected the input features and the value to predict.
3. Split the data into training and testing sets.
4. Created a simple baseline model for comparison.
5. Trained Linear, Polynomial, Ridge, and Lasso Regression models.
6. Compared the models using MAE, RMSE, and R².
7. Created graphs to compare actual values with predictions and examine prediction errors.

## Results

| Model | MAE | RMSE | R² |
|---|---:|---:|---:|
| Baseline | 9.11 | 10.60 | -0.021 |
| Linear Regression | 2.91 | 3.57 | 0.884 |
| Polynomial Regression | 3.14 | 3.95 | 0.858 |
| Ridge Regression | 2.91 | 3.57 | 0.884 |
| Lasso Regression | 2.91 | 3.57 | 0.884 |

Linear Regression, Ridge Regression, and Lasso Regression gave the same results on the test data. Linear Regression is a good choice for this project because it is simple and performed well.

## Tools Used
- Python
- pandas
- NumPy
- Matplotlib
- scikit-learn
- Jupyter Notebook

## Files
- ML_Lab3.ipynb: The notebook containing the code, results, graphs, and explanations.
- lab_ticket_resolution.csv: The dataset used for the project, if included in the repository.

## What I Learned
This lab helped me practice preparing data, training different regression models, comparing their results, and understanding why it is important to check prediction errors before choosing a model.

## Limitations
The dataset may not represent real support tickets, and I only used two input features. The model should be tested on real, future ticket data before being used in practice.
