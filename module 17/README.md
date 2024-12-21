## Project Overview
This project aims to improve the bank's marketing strategy by promoting term deposits. By leveraging the data from the 17 campaigns conducted, we can generate models that can help predict/classify new customers as ones who would or would not subscribe. This information can be leveraged to more efficiently target customers who would likely subscribe to term deposits.
We explored various model types, including Logistic Regression, K-Nearest Neighbors (KNN), Decision Tree, and Support Vector Machine (SVM).

### Dataset
The dataset includes the following features:
- **Bank Client Data**: age, job, marital status, education, default, housing loan, personal loan
- **Last Contact Data**: contact type, last contact month, last contact day, duration
- **Other Attributes**: campaign, pdays, previous, outcome of previous campaign
- **Economic Context Attributes**: employment variation rate, consumer price index, consumer confidence index, euribor 3 month rate, number of employees
- **Target Variable**: whether the client subscribed to a term deposit (`yes` or `no`)

### Preprocessing
- **'Duration' column**: Dropped, since this information would not have been available ahead of the call
- **Categorical Variables**: Encoded using OneHotEncoder.
- **Numerical Variables**: Standardized using StandardScaler.
- **Target Variable**: Label encoded (`yes` = 1, `no` = 0).

### Results

#### Default Parameters
| Model               | Training Time (s) | Training Accuracy | Testing Accuracy |
|---------------------|-------------------|-------------------|------------------|
| Logistic Regression | 0.36              | 0.9005            | 0.9009           |
| KNN                 | 0.004             | 0.9132            | 0.8923           |
| Decision Tree       | 0.217             | 0.9956            | 0.8344           |
| SVM                 | 47.80             | 0.9053            | 0.9009           |

#### With GridSearchCV
| Model         | Time (s)   | Train Accuracy | Test Accuracy | AUC     | F1      |
|---------------|------------|----------------|---------------|---------|---------|
| KNN           | 4.54       | 0.9077         | 0.8947        | 0.6213  | 0.8776  |
| Decision Tree | 1.61       | 0.9187         | 0.8959        | 0.6273  | 0.8796  |
| SVM           | 167.83     | 0.9053         | 0.9009        | 0.6207  | 0.8815  |

### Summary of Findings
- **Logistic Regression and KNN**: good performance with short training times.
- **Decision Tree**: prone to overfitted on the training data, as seen with the high training accuracy but low test accuracy when running with default values for hyperparameters.
- **SVM**: slight improvement in terms of test accuracy and F1 score but at the cost of significantly longer training times.

## [Link to Notebook](./notebook.ipynb).
