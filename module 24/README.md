# Predicting Crop Yields Using Machine Learning

## Links
- [Jupyter Notebook](./notebook.ipynb)  
- [Dataset on Kaggle](https://www.kaggle.com/datasets/patelris/crop-yield-prediction-dataset?select=yield_df.csv) (dataset also uploaded to repo in the 'data' folder [here](./data/yield_df.csv))

## Introduction
### Problem Statement
The goal of this project is to leverage machine learning techniques to accurately predict crop yields based on various environmental factors and agricultural practices. By answering this question, we can enable farmers to properly estimate their yields to maximize profits and positively influence the world's food security.

### Objectives
- Identify the most significant factors affecting crop yields.
- Build a predictive model to estimate crop yields based on the various input factors.

### Data Source
The dataset was sourced from [Kaggle](https://www.kaggle.com/datasets/patelris/crop-yield-prediction-dataset?select=yield_df.csv), which includes information on environmental factors and crop yields from various countries around the world, from 1990 to 2013.

## Data Exploration and Cleaning
### Initial Data Exploration
The dataset contained the following information:
- Categorical:
    - Country
    - Crop
- Numerical:
    - Year
    - Average Temperature (in degrees Celsius)
    - Average Rainfall (in mm)
    - Amount of Pesticides (in tonnes of active ingredient)
    - Yield of the crop (in Hg/Ha)

### Data Cleaning Steps
- Dropped an unnecessary column called 'Unnamed: 0', which was a duplicate of the index
- Renamed column names to simplify/represent the data better
- The Pesticides and Yield features had a right-skew to their distributions
    - In order to improve the accuracy of the models I transformed these features on a logarithmic scale. This resulted in a more normal distribution. Please find more information and visuals in the Notebook.

### Data Preprocessing
- Using the 'Year' data, I generated n-1 lag features for Rainfall, Temperature, and Pesticides
    - This enabled the models to leverage data from the previous year in their computations
- Applied one-hot encoding for categorical variables - Crop and Country
    - This enables the models to utilize categorical data by converting them into a numerical format
- Generated higher-order Polynomial terms for models applying Sequential Feature Selection
- Applied scaling for numerical features - Rainfall, Temperature, and Pesticides
    - This enabled models that relied on regularization to apply their weights in the algorithm consistently across data/features that would otherwise vary widely in magnitude
    - Also scaled higher-order features where applicable


## Modeling and Analysis
### Chosen Models
- Linear Regression
- Ridge Regression
- Lasso Regression
- Polynomial Regression with Sequential Feature Selector (SFS)
- Ridge Regression with SFS
- Lasso Regression with SFS
- Ridge SFS with Linear Regression
- Lasso SFS with Linear Regression

### Cross-Validation and Grid Search
- Used cross-validation to validate model performance.
    - This ensured the models performance was generalized for the data, and not a consequence of randomness or sampling.
- Used grid search to tune and optimize hyperparameters for the chosen models to reduce the error of the models.

### Model Evaluation Metrics
- Mean Squared Error (MSE) was used to evaluate model performance.

## Results
### Performance Comparison (based on MSE metric)
Linear Regression and Ridge Regression had the lowest test MSEs of 0.211012 and 0.211039, respectively, while Lasso Regression coupled with Sequential Feature Selection performed relatively poorly (test MSEs > 1.0).

### Training Times
Training times varied significantly, with model choices employing higher-order Polynomial terms taking the longest (max of 179.401999 seconds). The models that did not use PolynomialFeatures were much quicker, ranging between 0.696997 to 2.166995 seconds.

Training time is an important consideration when selecting machine learning models as it greatly affects considerations about scalability and practical implementation. A theoretical "ideal" model that takes an unreasonable amount of time would hardly work for farmers who often have relatively short windows of time to sow seeds/saplings depending on weather conditions and various logistics of the farm.

### Model Insights
Models that included polynomial features and SFS showed some signs of overfitting, as can be observed with choosing more of the higher-order polynomial terms during the feature selection process. Overfitting could indicate that the model is capturing noise in the training data, which may lead to poor generalization to new data. Please see the "Analyzing Results" section of the Notebook for more details on the specific features that were chosen by each of the models employing SFS.

## Findings and Recommendations
### Key Findings
The models that performed best (Linear Regression and Ridge Regression) seemed to have a strong preference to utilize the type of Crop planted and Country as the deciding factors for the yield. However, it is interesting to note that the LASSO with SFS models often relied on the Rainfall, Pesticides, and Temperature (plus their lag features) data for their predictions, as observed during the permutation importance analysis. Please see the relevant section of the notebook for more details on the relative importance given to features by each of the models.

### Actionable Insights
I think more research would need to be done to draw actionable insights from the analysis. There seems to be a high degree of variability between the yields of crop types, so perhaps retraining for each crop type separately could result in more actionable data. From some of the models, we can infer that areas with higher amounts of rainfall and pesticide use was loosely tied with higher crop yields. However, while none of the models refute this claim, the majority of the simpler models did not appear to give much weight to these factors when trying to predict the final yield.

### Recommendations
As mentioned before, rerunning a version of this analysis to focus on a particular type of crop can help to eliminate the variability we are seeing, and allow the models to better utilize the numerical features. A farmer in a certain country might also just be interested in training these models on the subset of the data that pertains to their country of residence.

One of the steps I took was to generate lag features for the n-1 year for Pesticides, Rainfall, and Temperature data. Generating more of these lag features, or perhaps taking elements from a time-series analysis perspective, could help to not only improve the model to estimate the crop yield but also be used to extrapolate future Rainfall and Temperature data (since these are environmental factors not in our control), and then adjust the quantities of water and pesticides the farmer uses accordingly to then maximize yield.

Another topic that could be interesting to investigate is the case for organic farming. This dataset only has non-zero values for Pesticide use. Datasets/Models that can highlight the differences in crop yields between regular and organic farming practices (which do not use pesticides to grow their crops) could be insightful to farmers who are considering which crops to plant, and whether to use pesticides or practice organic farming. Even though the yields could vary between these two practices, the sale price is often higher for organic produce, potentially tipping the scales for a farmer.
