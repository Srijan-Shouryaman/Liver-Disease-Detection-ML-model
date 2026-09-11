# Liver Disease Classification Using SVM

A machine learning project for predicting liver disease from patient medical records using a Support Vector Machine (SVM) classifier.

The project focuses on data preprocessing, exploratory data analysis, feature engineering, model training, and evaluation of an SVM-based classification model.

## Dataset

The dataset used in this project is the **Indian Liver Patient Dataset (ILPD)**, obtained from the **UCI Machine Learning Repository**.

The dataset contains medical and demographic information about Indian liver patients, including:

- Age
- Gender
- Total Bilirubin
- Direct Bilirubin
- Alkaline Phosphotase
- Alamine Aminotransferase
- Aspartate Aminotransferase
- Total Proteins
- Albumin
- Albumin and Globulin Ratio
- Liver Disease (target)

The target variable indicates whether the patient has liver disease:

- `1` → Liver Disease
- `0` → No Liver Disease

## Project Workflow

The project follows these steps:

1. Load and inspect the dataset
2. Handle missing values
3. Perform exploratory data analysis (EDA)
4. Analyze feature correlations and distributions
5. Engineer relevant features
6. Apply transformations to highly skewed features
7. Encode categorical variables
8. Standardize numerical features
9. Train an SVM classifier
10. Tune SVM hyperparameters using GridSearchCV
11. Evaluate the model using multiple classification metrics

## Machine Learning Model

The primary model used in the notebook is:

**Support Vector Machine (SVM)**

Different kernels and hyperparameters were explored, including:

- Linear kernel
- RBF kernel
- C
- Gamma

Hyperparameter tuning was performed using cross-validation with `GridSearchCV`.

### Additional Experiments

Logistic Regression and XGBoost were also explored separately as alternative approaches during experimentation. However, the main implementation and notebook focus on the SVM model.

## Evaluation Metrics

The model is evaluated using:

- Accuracy
- Precision
- Recall
- F1 Score
- Specificity
- Confusion Matrix

Specificity is particularly considered because correctly identifying patients without liver disease is important for evaluating false-positive predictions.

## Synthetic Data Experiment

The original dataset contains considerably fewer non-disease samples than disease samples.

To investigate whether additional non-disease examples could improve model specificity, synthetic samples with `Liver_Disease = 0` were generated based on patterns observed in the original dataset.

The synthetic samples were designed to:

- Preserve relationships between laboratory measurements
- Avoid simple duplication of existing observations
- Maintain realistic feature ranges based on the original dataset
- Preserve relationships between Total Bilirubin and Direct Bilirubin
- Preserve relationships between AST and ALT
- Preserve relationships between Total Proteins and Albumin
- Maintain consistency of the Albumin/Globulin (A/G) ratio
- Introduce natural variation rather than simply copying existing samples

The synthetic samples were treated as **training data only** and were not considered real patient observations.

### Results

#### Original SVM

| Metric | Score |
|---|---:|
| Accuracy | 74.56% |
| Precision | 77.08% |
| Recall | 91.36% |
| F1 Score | 83.62% |
| Specificity | 33.33% |

The original SVM achieved high recall, meaning it identified most disease cases correctly. However, its specificity was relatively low, resulting in a high number of false-positive predictions for non-disease cases.

#### XGBoost with Synthetic Data

| Metric | Score |
|---|---:|
| Accuracy | 73.78% |
| Precision | 78.79% |
| Recall | 64.20% |
| F1 Score | 70.75% |
| Specificity | 83.13% |

After introducing synthetic non-disease samples and experimenting with XGBoost, specificity increased substantially.

The model correctly identified many more non-disease cases compared with the original SVM. However, this improvement came with a reduction in recall and overall accuracy.

This experiment demonstrates that increasing representation of the minority class can significantly affect the model's decision boundary and the balance between sensitivity and specificity.

## Classification Threshold Experiment

Probability thresholds were also tested with the XGBoost model to investigate the trade-off between detecting disease cases and correctly identifying non-disease cases.

| Threshold | Accuracy | Recall | Specificity |
|---:|---:|---:|---:|
| 0.30 | 62.2% | 85.2% | 39.8% |
| 0.35 | 67.1% | 77.8% | 56.6% |
| 0.40 | 71.3% | 75.3% | 67.5% |
| 0.45 | 71.3% | 67.9% | 74.7% |
| 0.50 | 73.8% | 64.2% | 83.1% |

As the classification threshold increased:

- Recall generally decreased.
- Specificity generally increased.
- The model became more conservative when predicting liver disease.

The highest accuracy in this experiment was obtained at a threshold of `0.50`, while lower thresholds favored higher recall at the cost of specificity.

This demonstrates the trade-off between **sensitivity (recall)** and **specificity** when changing the classification threshold.

> **Note:** Synthetic data was used only as an experimental technique and does not represent real patient observations. Model performance should not be interpreted as clinical diagnostic performance.

