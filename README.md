# Kickstarter-Predictive-Analytics

## Introduction (Section 1)

Kickstarter is an American public-benefit corporation that maintains a global crowdfunding platform focused on creativity and merchandising. Project creators choose a deadline and a minimum funding goal. If the goal is not met by the deadline, no funds are collected. <br/> The goal of this project is to construct 2 supervised-learning models that predicts the pledged amount for a project and whether a project will be successful or not.

# Section 2. Data Description and pre-processing

Step 1: The original dataset had 45 variables (including predictor and target). Some of them were removed before including them in our model and the list below talks about them:

Step 2: Filtered only for successful and failed projects. (2973 rows got removed)
Step 3: Dropped rows having any NA values. This operation is performed after removing the launch_to_state_change_days column (which has 70% NA values). 1471 rows got removed
Step 4: Removed outliers for all numerical columns. (1261 rows got removed)
Step 5: Created buckets for hour and day columns for date columns (deadline, created and launched. Bucketing helps in writing efficient codes and once can group certain hours/days to build a good model. Days were bucketed based on weeks (0-7 days, 7-14 days, 14-21 days and 21+ days). Hours are bucketed based on 0-8 hrs, 8-16 hrs, 16+ hrs of the day
Step 6: The goal column has the goal amount in the currency of the country where the project has been developed. To keep all projects on the same scale, we multiply it with static_usd_rate
Step 7:  Select the relevant columns as per the previous steps
Step 8: Dummify the categorical variables
Step 9: Run a correlation test to find perfectly correlated columns. Removed columns: Currency
Step 10: For the classification task, the Y variable is state, while for the regression task, the Y-variable is usd_pledged. The X variable depends on the feature selection.

