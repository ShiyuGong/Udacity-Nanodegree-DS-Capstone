# Udacity-Nanodegree-DS-Capstone 
# Starbucks Challenge
Medium link: https://medium.com/@sygong/one-stop-guide-to-creating-predictive-models-udacity-nanodegree-data-scientist-capstone-starbucks-3e19619ad765
## Project Overview
Once every few days, Starbucks sends out an offer to users of the mobile app. An offer can be merely an advertisement for a drink or an actual offer such as a discount or BOGO (buy one get one free). And not all users receive the same offer. We can image that ideally, Starbucks want to send different types of offer to different customers to maximize their possibilities to complete the offer and buy more coffees! :)

<b> Overall, the aim of this project is to combine demographic, offer and also customer behavior data to build a model that can predict whether or not a specific customer will complete an offer received.</b>

## Data Sets

The data is contained in three files:

* portfolio.json - containing offer ids and meta data about each offer (duration, type, etc.)
* profile.json - demographic data for each customer
* transcript.json - records for transactions, offers received, offers viewed, and offers completed

Here is the schema and explanation of each variable in the files:

**portfolio.json**
* id (string) - offer id
* offer_type (string) - type of offer ie BOGO, discount, informational
* difficulty (int) - minimum required spend to complete an offer
* reward (int) - reward given for completing an offer
* duration (int) - time for offer to be open, in days
* channels (list of strings)

**profile.json**
* age (int) - age of the customer 
* became_member_on (int) - date when customer created an app account
* gender (str) - gender of the customer (note some entries contain 'O' for other rather than M or F)
* id (str) - customer id
* income (float) - customer's income

**transcript.json**
* event (str) - record description (ie transaction, offer received, offer viewed, etc.)
* person (str) - customer id
* time (int) - time in hours since start of test. The data begins at time t=0
* value - (dict of strings) - either an offer id or transaction amount depending on the record

## Steps to work on the project
### 1. Exploratory Data Analysis (EDA)
### 2. Data Processing
To prepare for modeling part later, we need to clean each dataset and merge them to be a master dataset in this section.

The following several steps are really important To prepare the data for the modeling part!!
1. Since the aim of this project is to predict whether one customer will finally complete one offer or not, and informational offer does not have 'offer completed' event at all, we will delete all the informational offer related events
2. Pivot the table to reformat it to be <b>one row for one customer-offer pair </b>
3. Add one new column 'offer_completed' to indicate whether the customer completed the offer 
4. Add one more new column 'offer_viewed'
### 3. Data Modeling
In this section, a model would be built to help predict whether a given customer will finally complete a specific offer or not.
The independent variables are as follows:
1. offer related variables:

    >'offer_id','offer_type','difficulty','duration_h','reward','channel_email','channel_mobile', 'channel_social', 'channel_web'

2. customer demographic related variables:

    >'gender','age_group', 'income_range', 'member_type'
    
3. customer behavior related variable:
    > 'offer_viewed'
    
Dependent variable (the outcome to predict) is  <b>'offer_completed'</b>

### Now we can start to build our predicting model as follows:

* Split the data into independent variables and target variable.
* Train_test_split: use 70% of the data for training and 30% for testing
* MinMax scaler: normalize some numerical values
* Try different ML algorithms and select the best model according to model performance.
1. Grid search is used to help choose parameters in the meantime.
2. Model performance is mainly based on accuracy_score, F1 score, AUC curve etc.
3. Model is explained by using permutation_importance and SHAP value

## Conclusions
1. Random Forest model is the best model among the three we tried. The accuracy of that is as high as 0.85 and AUC is 0.93, which is pretty good.
2. offer_viewed, member_type, income_range are the most important three features for the model
3. If a customer views the offer, he/she is more likely to complete the offer.
4. Longer one customer joins the Starbuck platform and becomes a member, more likely they would complete the offer. Building up customer loyalty is important.
5. Higher the income one customer has, more likely they would complete the offer.

## Further Improvement
For this project, we successfully build a Machine Learning model that can predict whether or not a specific customer will complete an offer received with demographic, offer and also customer behavior data.

<b>How could this model be useful in the real business world? </b>

One implementation could be personalizing the offers.

For each customer, we could calculate the probability of his/her completing each type of offers via our model, and then send the most-likely-to-complete offer to the customer.
Another topic which is also interesting to work on is to identify groups who will make purchases even if they don't receive an offer. (I might be this type of customer lol I am really a coffee lover!) From a business perspective, if a customer is going to make a 10 dollar purchase without an offer anyway, you wouldn't want to send a buy 10 dollars get 2 dollars off offer.
