# Residential Properties: Imputation, Classification and Price Prediction in R

Statistical modelling of 2,919 residential property sales recorded between 2006 and 2010, with 51 variables covering lot, building, quality, condition and sale attributes. The neighbourhood codes match the Ames, Iowa housing data. The project asks two questions:

1. Can a house's **overall condition** (grouped into poor / average / good) be predicted from its other attributes?
2. How well can **sale price** be predicted, and which model does better?

University of Essex, MA321 Applied Statistics, group project (supervisors Dr Stella Hadjiantoni and Dr Fanlin Meng), March 2020. Authors: Danisha A. Rajoo, Divya Arora, Rohini Subramaniam, Nur F. F. B. Rosli, Srinidhi Karthikeyan. My parts were the numerical and graphical summaries, the conclusion, and LaTeX formatting (title and contents pages).

## Methods

| Step | Method |
|---|---|
| Missing data | Missingness patterns (`mice::md.pattern`, `VIM::aggr`). Columns with more than 60% missing were dropped. Multiple imputation by chained equations with predictive mean matching (`mice`, m = 5, 50 iterations) |
| Feature selection | Correlation filter: one of each pair with \|r\| > 0.8 removed (`caret::findCorrelation`) |
| Overall condition | Multinomial logistic regression (`nnet::multinom`) and linear discriminant analysis (`MASS::lda`) on centred and scaled features |
| Sale price | Random forest (`randomForest`) and gradient boosting (`gbm`), each tuned over 100 / 500 / 1,000 trees |
| Validation | Validation-set approach and leave-one-out cross-validation, scored by mean absolute error |
| Further questions | MANOVA and follow-up ANOVAs relating overall condition to overall quality and year built |

## Results

As reported in the project report:

| Task | Model | Result |
|---|---|---|
| Overall condition (3 classes) | Multinomial logistic regression | 81.6% accuracy |
| | LDA | 81% accuracy |
| Sale price, validation set | Random forest | MAE 20,013 |
| | Gradient boosting | MAE 23,687 |
| Sale price, LOOCV | Random forest | MAE 33,475 |
| | Gradient boosting | MAE 36,843 |

Random forest gave lower error than boosting under both validation schemes. Most houses fall in the "average" condition class, which should be borne in mind when reading the ~81% classification accuracy.

## Repository contents

| File | Description |
|---|---|
| `Residential_Data-Applied_Statistics.pdf` | Report (34 pages). The full R code is in Appendix 6.1 and contributions in 6.2 |
| `house_data.csv` | Dataset: 2,919 rows × 51 columns |

## Reproducing

The R code is in Appendix 6.1 of the report. It uses `mice`, `VIM`, `caret`, `nnet`, `MASS`, `randomForest`, `gbm`, `caTools`, `dplyr` and `ggplot2`, and reads `house_data.csv` from the working directory. One script in the appendix writes the imputed data to an absolute path on a team member's machine (`/Users/shankar/Desktop/...`) and reads it back as `completedData.csv`. Change that path to `completedData.csv` before running it.
