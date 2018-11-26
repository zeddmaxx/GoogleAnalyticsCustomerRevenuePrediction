# GoogleAnalyticsCustomerRevenuePrediction
Machine LEarning project

**Objective of the notebook:**

In this notebook, let us explore the given dataset and make some keen insights into the data. We will build a baseline light gbm model and a Gradient Boosting Regressor model to begin with 

**Objective of the competition:**

In this competition, we a’re challenged to analyze a Google Merchandise Store (also known as GStore, where Google swag is sold) customer dataset to predict revenue per customer. 

We have two datasets and a sample submission file
* train.csv
* test.csv
* sample_submission.csv

**About the dataset:**
Each row in the dataset is one visit to the store. We are predicting the natural log of the sum of all transactions per user. 
    
The data fields in the given files are 
* fullVisitorId- A unique identifier for each user of the Google Merchandise Store.
* channelGrouping - The channel via which the user came to the Store.
* date - The date on which the user visited the Store.
* device - The specifications for the device used to access the Store.
* geoNetwork - This section contains information about the geography of the user.
* sessionId - A unique identifier for this visit to the store.
* socialEngagementType - Engagement type, either "Socially Engaged" or "Not Socially Engaged".
* totals - This section contains aggregate values across the session.
* trafficSource - This section contains information about the Traffic Source from which the session originated.
* visitId - An identifier for this session. This is part of the value usually stored as the _utmb cookie. This is only unique to the user. For a completely unique ID, you should use a combination of fullVisitorId and visitId.
* visitNumber - The session number for this user. If this is the first session, then this is set to 1.
* visitStartTime - The timestamp (expressed as POSIX time).

This confirms the first two lines of the competition overview.
    * The 80/20 rule has proven true for many businesses–only a small percentage of customers produce most of the revenue. As such, marketing teams are challenged to make appropriate investments in promotional strategies.
Infact in this case, the ratio is even less.    

**Columns with constant values: **

We decided to remove these as they don't add any insights to our prediction

**Baseline Models:**

With the kind of diverse data that we have at hand, we decided to use an ensembling technique to predict the total revenue per user.
Ensembing techniques require us to combine the predictions of multiple algorithms(estimators) and then outputs the best prediction by voting or averaging. These lone algorithms are called weak learners as they are not capable of efficient predictions by themselves, but once we combine them, we start getting significantly better results.

Light GBM is a gradient boosting framework that uses tree based learning algorithm. Other tree algorithms grow the tree horizontally while LGBM grows vertically by performing a leaf-wise growth instead of a level-wise growth. It grows the leaf that has the maximum delta loss. This helps reduce loss as compared to growing a tree level wise. This loss function is then usually optimized by performing gradient descent. 

It is not advised to use this algorithm on small data sets as it is sensitive to overfitting. It is generally advised to use it on data sets having more than 10,000 rows. 

Since the data set for the given problem has more than 9 lakh train records and more than 8 lakh test records, Light GBM seems like a good choice.

Gradient Boosting Regressor
This machine learning model computes a sequence of (very) simple trees, and successive trees are built from the prediction results of the preceding trees. This method builds binary trees, it partitions the data into two samples at each split node. Consider a case where we wish to limit the complexities of the trees to 3 nodes only: a root node and two child nodes, i.e., a single split. 

A simple (best) partitioning of the data is determined at each step of the algorithm, and we compute the deviations of the observed values from the respective means (residuals for each partition) are computed. The next 3-node tree will then be fitted to those residuals, to find another partition that aims to further reduce the variance(residual error) for the data, given the earlier sequence of trees.

"Additive weighted expansions" of trees eventually produce an excellent fit of the predicted values to the observed values, even if the specific nature of the relationships between the predictor variables and the dependent variable of interest is very complex and non linear in nature.
 
We need to do a sum for all the transactions of the user and then do a log transformation on top. Let us also make the values less than 0 to 0 as transaction revenue can only be 0 or more. 
So we are getting a **RMSE score of 1.70** for our Light GBM and MSe score of 3.53 
