# Predicting the 2026 MLB All-Star Team

## Overview

This project develops a decision-aware machine learning framework to predict which MLB hitters are most likely to be selected to the 2026 MLB All-Star Game using historical All-Star outcomes and Steamer 2026 projections.

Rather than treating All-Star selection as a purely performance-based outcome, this work models recognition and perception. These factors directly impact payroll planning, arbitration risk, roster valuation, and marketing strategy in a front-office context.

The final output is a probabilistic forecast of All-Star likelihood for each player designed to support baseball operations and R and D decision-making rather than produce a single deterministic answer.

## Why This Matters for Baseball Operations

All-Star selections influence:

* Arbitration and salary negotiations
* Future payroll forecasting
* Player market perception
* Marketing and fan engagement strategy

By identifying likely All-Star candidates before the season begins teams can anticipate downstream financial and roster implications rather than reacting after recognition occurs.

## Data Sources

| Source | Description |
|-------|-------------|
| Historical MLB batting data 2015-2024 | Player season offensive statistics used to learn All-Star selection patterns |
| All-Star selection indicator | Binary target variable one equals All-Star zero equals Non-All-Star |
| Steamer 2026 projections | Forward looking projections used to generate 2026 predictions |

To ensure relevance to the modern game model training was restricted to the 2021-2024 post pandemic period after statistical testing showed greater stability in All-Star performance standards during this window.

## Modeling Approach

Problem type Binary classification All-Star versus Non-All-Star

Each player season is treated as an independent observation and model outputs are interpreted as probabilities not hard labels.

### Models Evaluated

| Model | Rationale |
|------|-----------|
| Logistic Regression | Interpretable baseline with probabilistic output |
| Decision Tree | Captures nonlinear performance thresholds |
| Random Forest | Reduces variance and overfitting through ensembling |
| Gaussian Naive Bayes | Fast probabilistic baseline with class priors |

All models were implemented using scikit-learn with:

* Pipeline based preprocessing
* Stratified cross validation
* Hyperparameter tuning via GridSearchCV
* Decision threshold optimization

## Custom Evaluation Metric

Standard F1 score enforces an equal tradeoff between precision and recall which is not ideal for All-Star forecasting.

In a front-office context:

* Missing a true All-Star candidate false negative is more costly than
* Flagging a fringe candidate false positive

To reflect this asymmetry we implemented a custom F beta metric that weights recall more heavily than precision approximately 60 percent recall and 40 percent precision.

This approach better reflects how front offices evaluate candidate identification problems where missing elite talent is more costly than shortlisting fringe cases.

## Model Performance Comparison

| Model | F beta | Recall | Precision |
|------|--------|--------|-----------|
| Logistic Regression class weight | 0.59 | 0.61 | 0.74 |
| Logistic Regression SMOTE | 0.65 | 0.81 | 0.52 |
| Decision Tree pre pruned | 0.49 | 0.61 | 0.62 |
| Decision Tree post pruned | 0.54 | 0.77 | 0.49 |
| Random Forest | 0.55 | 0.74 | 0.55 |
| Gaussian Naive Bayes | 0.54 | 0.88 | 0.42 |

Final model selected Logistic Regression with SMOTE and optimized decision threshold.

## 2026 Predictions

The final model was refit on all available 2021-2024 data and applied to Steamer 2026 projections.

| Output | Description |
|--------|-------------|
| All-Star probability | Probability of being selected as a 2026 All-Star |
| Binary prediction | Based on optimized decision threshold |
| Ranked candidate list | Players ordered by All-Star likelihood |

These outputs are intended to support payroll planning arbitration planning roster valuation and player marketing strategy.

## Tech Stack

| Category | Tools |
|---------|-------|
| Language | Python |
| Data analysis | pandas NumPy |
| Machine learning | scikit-learn |
| Imbalanced learning | imbalanced-learn SMOTE |
| Model selection | GridSearchCV Stratified K Fold |
| Visualization | matplotlib seaborn |
| Environment | Jupyter Notebook |

## Final Note

This project was built with MLB R and D and Baseball Operations use cases in mind emphasizing interpretability decision relevance and practical tradeoffs over leaderboard driven metrics.
