# Overview
* An organization offers solutions in water purification, vacuum clearing, air purification, and security solutions. The company sells both household and industrial products and uses a
multi-channel distribution system, having started with direct sales and later adding retailing to its mix. While the organization is a multi-product company, their flagship product
focuses on water purification and they have established themselves as a household name in the company’s domestic market of India.
* Millions of customers visit the Eureka Forbes website and of course many end up not being converted into prospects. The company spends millions of dollars annually on digital
marketing campaigns in order to try to turn site visitors into prospects (for lead generation) and eventually satisfied customers.
* The task at hand is to achieve better conversion at lower costs by using rich historical clickstream data to develop predictive models that can effectively identify the most desirable set of customers for remarketing campaigns.

## Dataset
* The dataset that was provided included high-dimensional variables that are categorical, numerical, date/time, binary, and geographic. 
* Accounting for the customer base’s online interactions, the set is geared towards uncovering insights to help drive their marketing solutions and drive the effectiveness of their campaigns. 
* Amongst these, we have online elements as time spent on page, visit date, device, and region, etc.
* The dataset was highly imbalanced

## Data cleaning
* Certain categories were also highly imbalanced such as region that had observations occurring only one or two times. These were fixed by creating another group “Other” which contained these smaller categories. 
* For missing values, it was only the 30-day (any column with hist) features that had missing values, all of which were numeric, so we used 0 to substitute missing values. 
* Surrogate columns were also created for these missing values enabling the algorithm to understand that imputed values were used.

## Model Development
* By splitting the dataset into training and test groups based on a split of 70/30, we created a holdout dataset to see how the model would perform on future data. 
* Using the traincontrol function from the caret package, we ran a repeated cross validation with 10 cross folds and 5 repeats for Random Forest and XGBoost. 
* We also created a tunegrid for hyperparameter tuning to get the optimal value based on ROC/Accuracy as the metrics for evaluation.
* For Random Forest model, the hyperparameters related to ntree(number of trees), nodesize and mtry (number of variables randomly selected) were used and tuned to build the most optimal model 
* For XGBoost model, the hyper parameters related to nround, max_depth(maximum depth of a tree) and eta (learning rate) were used and tuned to build the most optimal model.
* In order to deal with the imbalanced dataset and be able to get more accurate results the data must be balanced. There are various techniques that can be used to balance the dataset such as undersampling, oversampling, smote and rose (undersampling + oversampling). In our case, we specified the sampling technique in traincontrol function as “Smote”.

## Model Evaluation Metrics
* Sensitivity – Is a very crucial metric as it calculates the True Positive Rate for this dataset. This metric helps with detecting the accuracy of the model while classifying the minority class of customers that are being converted in 7 days.
* AUC – We used AUC as a metric to determine how well the models were performing on an overall basis. As an extension of the receiver operator characteristic (ROC), the “Area under the curve” is a measure that summarizes how a measure will perform in sensitivity and specificity under all possible threshold values.

## Ensembling
* Ensembling combines multiple classifiers to produce a better classifier, which helps reduce bias and variance in machine learning tasks. 
* We used stacking technique in the ensembling model that uses predictions from multiple models with different weights to build a new model. Model’s weights are learned from another supervised learning algorithm (i.e logistic regression etc.).
* For the stacking ensemble method, we used caretlist function from the caret package to train the model on the training dataset with the above derived models i.e. best tuned Random Forest and best tuned XGBoost.
* We used caretStack function from the caretEnsemble package which allows each model’s vote to have a different weight. The weight of each model’s vote is determined by a Generalized Linear Model.

## Results

Metric | Random Forest | XGBoost | Ensemble
------------ | ------------- | ------------- | -------------
Accuracy | 94.23% | 86.2% | 80.51%
AUC | 73% | 74.5% | 76%
Sensitivity | 27% | 46.6% | 54%
