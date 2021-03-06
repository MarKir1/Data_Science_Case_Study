## **Verve Group Data Science Case Study Answers**

### __Outline of the problem__

_Purpose_

In order to monetise, publishers of apps sell ad space to advertisers.
Advertisers want to show ads to the most relevant users, in order to increase 
their chances of getting a click.

_Task_

The data science task is to try to predict the gender (male/female) of a device 
user in order to help advertisers to target within apps.

_Data_

Every time a user interacts with an app and is shown an ad, data are generated.
Each row of data represents an event, where a user is shown an ad during an app.

### **Answers to assignments**

**Assignment 1:** Look at the given information and identify 3-5 potential problems 
you can see with the provided dataset that might make building a classification 
model difficult.

**Answer**

a) There is some imbalance of the target label: The frequencies of male/female in the 
data are slightly imbalanced (with ~ 7:3 ratio). If the dataset is biased towards one 
class, the trained algorithm on the same data can be biased towards the same class. 
We might need to take care of this:
- when using a train-test split (or a cross-validation): e.g. do stratified sampling 
  (and respectively stratified k-fold cross-validation) by splitting the dataset randomly, 
  although in such a way that maintains the same class distribution in each subset. 
- when choosing the ML model: choose a methods, which can be used to deal with imbalanced 
  datasets (e.g. an ensemble-based method).
  
b) The `'yes'` values of the feature `'click'`  are quite rare relative to the total number of data observations,
which makes the feature hardly usable (e.g. there might be no observations with `'yes'` click 
selected in the training data).

c) The `'ad_category'` can be used only if the `'click'` is `'yes'`, and the number of `'yes'` 
values is rare. 

d) The data point `'device_name'` has quite a few missing values (~60%). And also is a text information,
the standard missing values handling methods for categorical features (e.g. removing, filling with 
common ones etc.) are not suitable. First need to extract the name of the device owner.
Possible options could be:
- either match them with male/female name external dictionaries. Use the method as an alternative 
  for the ML model prediction, where suitable.
  
- use as a categorical features, but as there can be too many of unique values, need to 
  treat the high cardinality issue.
  
**Assignment 2:** Describe briefly how you would find the features that are likely to be 
the most important for your model.

**Answer**

- For continuous features (e.g. interaction_with_app) I would do the ANOVA f-statistic test 
  and select features with high f-statistic and low p value (close to 0 or less than a selected threshold)
- For categorical features I would calculate Mutual Information statistic or Chi-Square 
  statistic similarly with preference to higher statistic values.

As one user's data can be in multiple rows of data-set, it may also be useful to generate 
new features aggregated from all their event data points (e.g. the % of time the user spent 
on each category of app; or the top app category the user spends most of their time, 
average time spent in one app session etc.).

**Assignment 3:** Identify which model you would try first, and at least one advantage and 
disadvantage of this choice.

**Answer**

I would try first CatBoost model (taking into account also, that the % of categorical 
features in the data-set is high). 

Possible advantages:
- It supports sophisticated categorical features;
- Can treat the target label class imbalance.

Possible disadvantages:

- There might be some difficulty in tuning the parameters to optimize the model for 
  categorical features.
  