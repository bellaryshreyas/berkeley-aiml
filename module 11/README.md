### Link to notebook
[Notebook](./notebook.ipynb)

### Findings:

Through this project, I got to implement the CRISP-DM framework to analyze the used car sales data.
After cleaning the data and splitting it into 80/20 train/test sets, I created a model using the Ridge regression function, fine-tuning the hyperparameter alpha using GridSearchCV. Feature selection was done through SequentialFeatureSelector, choosing the top 10 features to use in the model.

The conclusions I was able to draw from my analysis were that the odometer had the highest influence on the final sale price of the car, followed by the manufacturer and then year.
I created several graphs to visualize this.
