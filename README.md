# Kickstarter-Predictive-Analytics

## Section 1: Introduction

Kickstarter is an American public-benefit corporation that maintains a global crowdfunding platform focused on creativity and merchandising. Project creators choose a deadline and a minimum funding goal. If the goal is not met by the deadline, no funds are collected. <br/> The goal of this project is to construct 2 supervised-learning models that predicts the pledged amount for a project and whether a project will be successful or not.

# Section 2. Data Description and pre-processing

Step 1: The original dataset had 45 variables (including predictor and target). Some of them were removed before including them in our model and the list below talks about them:<br/>

![Kickstarter - data description](https://user-images.githubusercontent.com/54965123/68997557-cfe04a00-0875-11ea-9abf-e33cb8e4bd53.PNG)

Step 2: Filtered only for successful and failed projects. (2973 rows got removed)<br/>

Step 3: Dropped rows having any NA values. This operation is performed after removing the launch_to_state_change_days column (which has 70% NA values). 1471 rows got removed<br/>

Step 4: Removed outliers for all numerical columns. (1261 rows got removed)<br/>

Step 5: Created buckets for hour and day columns for date columns (deadline, created and launched. Bucketing helps in writing efficient codes and once can group certain hours/days to build a good model. Days were bucketed based on weeks (0-7 days, 7-14 days, 14-21 days and 21+ days). Hours are bucketed based on 0-8 hrs, 8-16 hrs, 16+ hrs of the day<br/>

Step 6: The goal column has the goal amount in the currency of the country where the project has been developed. To keep all projects on the same scale, we multiply it with static_usd_rate<br/>

Step 7:  Select the relevant columns as per the previous steps<br/>

Step 8: Dummify the categorical variables<br/>

Step 9: Run a correlation test to find perfectly correlated columns. Removed columns: Currency<br/>

Step 10: For the classification task, the Y variable is state, while for the regression task, the Y-variable is usd_pledged. The X variable depends on the feature selection.<br/>

# Section 3. Summary Statistics
Before we discuss feature selection, it is important to understand the summary statistics for the shortlisted X-variables as shown below:

![Kickstarter- summary stats](https://user-images.githubusercontent.com/54965123/68997558-cfe04a00-0875-11ea-84c8-00f5dedc097f.PNG)


# Section 4. Feature selection
Next, feature selection was performed to select the predictors which play a significant role in explaining the variation between X and Y better. There are 3 methods of carrying out feature selection. For each method, we tweak the value of parameters to get the best set of features (for ex: RFE is model dependent, LASSO depends on α, and Random Forest depends on gini threshold). 

**Feature selection results for the classification task:**

![Results Classification - Kickstarter](https://user-images.githubusercontent.com/54965123/68997559-cfe04a00-0875-11ea-9a12-86c68444e534.PNG)

**Feature selection results for regression task (for only the Random Forest selection method)**

![Results Regression- Kickstarter](https://user-images.githubusercontent.com/54965123/68997560-cfe04a00-0875-11ea-885c-0d81dd2a278a.PNG)

# Section 5. Final Model selection 
Clearly, Random Forest model seems to be the winner with features selected using the Random Forest feature Selection method. Upon finding the best hyperparameters, the best accuracy obtained is 74.48% and the mean squared error is 1.06 billion. See below:

![Best hyperparameters Kickstarter - Random Forest](https://user-images.githubusercontent.com/54965123/68997555-cfe04a00-0875-11ea-8f78-7924b0a4b070.PNG)

Model selection was done considering the time constraint. The below algorithms were not run:

**Regression:** SVM (usually performs poorly on regression), ANN (time-consuming) <br/>
**Classification:** KNN (does not run of RFE), ANN (time-consuming)<br/>
Since random forest performs better than decision trees, hence the latter was not run.

# Section 6. Conclusion and Business Interpretation
Based on the train dataset, 2 supervised models were built to predict the pledged amount for a project on Kickstarter, and to predict if the project is going to be successful or not.<br/>

**Regression model :** R squared value of the regression model – 0.16. The model explains 16% of the variation in the target (usd_pledged) variable.<br/>
The MSE is $1.02 Billion, therefore the error is $31,937, which means that the regression model will able to predict the pledged amount for a project with an error of +$31,937 and -$31,937.<br/>
**Classification model :** Using the confusion matrix (shown below), 5 metrics were calculated (shown below) to get statistical and business interpretation of the model  (Section 3.2.3.2 in code):

![Kickstarter - confusion matrix](https://user-images.githubusercontent.com/54965123/68997556-cfe04a00-0875-11ea-8a21-da34fe0de270.PNG)

Translating the metrics shown above into business language (retaining the same order)

•	Among the failed projects, the probability that the algorithm would identify them as successful will be 9%<br/>
•	Among the successful projects, the probability that the algorithm would identify them as failed will be 64%<br/>
•	Among all the projects, the probability of a correct prediction of their state is 74%<br/>
•	If the algorithm identifies a client as a successful project, the probability that the algorithm is correct is 66%<br/>
•	Among the successful projects, the probability that the algorithm would identify them as successful will be 36%<br/>

For a business like Kickstarter, it is desirable that the model should have a high precision but low recall, as the company spends both time and resources on advertising good projects which have higher chances of being successful. Hence, a high precision and low recall will ensure that the company is able to maximize its investment into good projects which are more likely to be successful.<br/>

In conclusion, with the given data the final model is able to predict the state of the projects on Kickstarter with a high-level of accuracy. The final model address both issues of overfitting and underfitting while simultaneously capturing the full essence of the dataset accurately. 

