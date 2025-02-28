---
name: Customer churn prediction
title: Customer churn prediction
image: https://github.com/Ciencia-Programada/articles-images/blob/master/churn/1.jpg?raw=true
position: 6
period: June 2022
toc: true
mathjax: true
header:
  image: https://github.com/Ciencia-Programada/articles-images/blob/master/churn/churn-prediction-article-banner.jpg?raw=true
  og_image: https://github.com/Ciencia-Programada/articles-images/blob/master/churn/1.jpg?raw=true
description: |
  Churn is a measure of how many customers stop using a service or product,
  often evaluated for a specific period of time. In this study, a churn level
  prediction process is carried out using machine learning. A dataset with over
  7000 customers of a telecom company is used. An action plan for the company is
  designed based on the results.
---

## Abstract

Churn is a measure of how many customers stop using a service or product, often
evaluated for a specific period of time. One of the biggest difficulties in
telecommunication industry is to retain the customers and prevent the churn.

Customers in the telecom industry can choose from a variety of service providers and
actively switch from one to the next. Acquiring new customers is not only more
difficult, but also much more costly to companies than maintaining existing customer
relationships. Therefore, many *Customer Churn Prediction* (CCP) models have been
implemented.

In this study, a churn level prediction process is carried out using machine learning. A
dataset with over 7000 customers of a telecom company is used. An action plan for the
company is designed based on the results.

The complete dataset, code, and auxiliary files can be found in the project repository on
GitHub linked below:

<div class="social-container">
<a href="https://github.com/chicolucio/churn-prediction" target="_blank"><img src="https://img.shields.io/badge/GitHub-000000?style=for-the-badge&logo=GitHub&logoColor=white" target="_blank"></a>
</div>

In this article, we will focus on the methodology used to predict churn and the results
obtained. The complete code can be found in the project repository.

## Contextualization

<figure style="width:50%">
    <img alt="churn" src="https://github.com/Ciencia-Programada/articles-images/blob/master/churn/1.jpg?raw=true">
</figure>


The telecom industry, while in a consolidation phase in developed markets, is booming in
emerging markets [[1](http://link.springer.com/10.1007/978-3-642-41154-0_19)]. With the
increasing variety of service providers, telecom churn has emerged as one of the most
important causes of revenue erosion for telecom operators
[[2](https://ieeexplore.ieee.org/document/8538420/)]. Then, predicting churners from the
demographic and behavioral data of customers has been of great interest. Acquiring new
subscribers can cost up to 6 times more than retaining an existing one
[[3](https://ieeexplore.ieee.org/document/8706988)].

Churn management involves, among many others:

- predicting customers likely to churn
- preventive actions
- running marketing campaigns

But why do customers churn? [[4](https://www.productplan.com/glossary/churn/)]

There is no unique answer. However, we can think of a few likely causes:

- customer no longer values the product
- motivating factors to use the product no longer exists
- customer frustrated with product user experience
- the product lacks a mandatory capability required by the user
- value to the customer does not justify the expense
- the customer has switched to an alternative solution
- damage to product reputation (e.g., cybersecurity issue, performance, etc.)

And how can a company reduce churn? [[4](https://www.productplan.com/glossary/churn/)]

Again, there is no silver bullet. But increasing the perceived value proposition of the
service to current users is obviously a key point. This can be achieved in several ways,
among them:

- make sure customers get the most out of the product
- recruit the right kind of customers
- price based on value
- continually add value without breaking what already works
- don't take customers for granted

In this study, an IBM dataset will be analyzed. The *Telco customer churn* data contains
information about a telecom company named Telco that provide home phone and internet
services to 7043 customers
[[5](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)]. It indicates
which customers have left, stayed, or signed up for their service. Multiple important
demographics and services are included for each customer, totaling 20 features. The aim
is to predict behavior to retain customers.

A cross-industry standard process for data mining (CRISP-DM)
[[6](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining#cite_note-Shearer00-1)]
approach will be used as shown in the figure below.

<figure>
<img src="../project_churn_prediction_files/pipeline_simple.png" alt="diagram crisp" style="width:100%"/>
<figcaption>Proposed methodology flow diagram</figcaption>
</figure>

Each step will be detailed in the appropriate section.


## Exploratory Data Analysis (EDA)

<figure style="width:50%">
    <img alt="eda" src="../project_credit_card_fraud_files/eda.jpg">
</figure>

As cited before, the dataset contains 7043 entries. There are 21 columns, with 20
features and the target variable `Churn`. The features are a mix of categorical and
numerical data. 

Each row represents a customer, and each column contains the customer's attributes, as
described below. The dataset includes information about:

- Customers who left within the last month:
    - `Churn`: *Yes* = the customer left the company within the last month. *No* = the
    customer remained with the company.
- Customers' demographic info:
    - `gender`: customer's gender: *Male*, *Female*
    - `SeniorCitizen`: customer is 65 or older: *1*, *0* (meaning *Yes* and *No*,
    respectively)
    - `Partner`: customer is married: *Yes*, *No*
    - `Dependents`: customer lives with any dependents: *Yes*, *No*. Dependents could be
    children, parents, grandparents, etc.
- Services that each customer has signed up for:
    - `PhoneService`: customer subscribes to home phone service with the company: *Yes*,
    *No*
    - `MultipleLines`: customer subscribes to multiple telephone lines with the company:
    *Yes*, *No*, *No internet service*
    - `InternetService`: customer subscribes to Internet service with the company: *No*,
    *DSL*, *Fiber Optic*
    - `OnlineSecurity`: customer subscribes to an additional online security service
    provided by the company: *Yes*, *No*, *No internet service*
    - `OnlineBackup`: customer subscribes to an additional online backup service
    provided by the company: *Yes*, *No*, *No internet service*
    - `DeviceProtection`: customer subscribes to an additional device protection plan
    for their Internet equipment provided by the company: *Yes*, *No*, *No internet
    service*
    - `TechSupport`: customer subscribes to an additional technical support plan from
    the company with reduced wait times: *Yes*, *No*, *No internet service*
    - `StreamingTV`: customer uses their Internet service to stream television
    programming from a third-party provider: *Yes*, *No*, *No internet service*
    - `StreamingMovies`: customer uses their Internet service to stream movies from a
    third-party provider: *Yes*, *No*, *No internet service*
- Customer account information:
    - `tenure`: total number of months that the customer has been with the company.
    - `Contract`: customer's current contract type: *Month-to-Month*, *One Year*, *Two
    Year*.
    - `PaperlessBilling`: customer has chosen paperless billing: *Yes*, *No*
    - `PaymentMethod`: how the customer pays their bill: *Electronic check*, *Credit
    Card*, *Mailed Check*, *Bank transfer*
    - `MonthlyCharge`: customer's current total monthly charge for all their services
    from the company
    - `TotalCharges`: customer's total charges, calculated to the end of the quarter
- Finally, each customer has a `CustomerID`, a unique ID that identifies the customer.

Let's verify the balance of the target variable `Churn`:

<figure style="width:60%">
<img src="../project_churn_prediction_files/cleansing_imbalance.png" alt="class_dist"/>
<figcaption>Class imbalance</figcaption>
</figure>

A little more than one quarter of the customers left the company in the last month. A
high number that we need to understand. The dataset is imbalanced, and this will be
considered in the evaluation of the models, since some algorithms may have problems
with imbalanced datasets [[7](https://linkinghub.elsevier.com/retrieve/pii/S0957417408002121)].

### Numeric features

It is important to check for outliers in the dataset. We can do this by analyzing the
distribution of the features. Using boxplots for each feature, we can identify outliers
in the data. We can also check the distribution of the features for each class to see if
there are differences between them.

<figure style="width:90%">
<img src="../project_churn_prediction_files/cleansing_outliers.png" alt="outlier"/>
<figcaption>Outliers in the dataset</figcaption>
</figure>

The boxplots show that the churn rate is higher among customers with low tenure and high
monthly charges. In details:

- the median tenure for customers who have left is around 10 months, while it is around
40 months for those who have stayed with the company. So, customers who have churned
spent less time with the company.
- the median monthly charge for customers who have churned is around 80, while it is
around 65 for those who have not churned. Therefore, customers who have churned have
higher monthly charges.
- since most customers who have churned spent less time with the company, they have low
median total charges compared with those who have stayed
   - There are some outliers in the total charges boxplot of customers who have churned.
   This shows that some customers who have churned have high total charges, even though
   they have spent less time with the company. This could be due to high monthly
   charges. Since the data is consistent, we will keep these outliers.


The shape of the distribution of the features is also important. We can check the
distribution of the features for each class to see if there are differences between
them. This can help us understand the data better and choose the best algorithm to
classify the transactions. Let's check the distribution of the features for each class:

<figure style="width:90%">
<img src="../project_churn_prediction_files/eda_histograms_churn.png" alt="dist_hue"/>
<figcaption>Distribution of the features for each class</figcaption>
</figure>

The tenure distribution has an interesting shape. Most customers have been with the
company for just a few months, but also many have been for about 72 months (maximum
value for tenure). This is probably related with different contracts, something that we
will check soon. Probably some marketing campaign was ran recently to capture new
customers due to the high number of customers with few months.

We can see that most customers pay low monthly charges, but there is a great fraction
with medium values. Since most customers have been with the company for just a few
months, the total charges plot shows most customers with low values.

Let's check if these features distributions have some kind of relation with the kind of contract:

<figure style="width:90%">
<img src="../project_churn_prediction_files/eda_histograms_contract.png" alt="dist_hue"/>
<figcaption>Histograms of the features by contract type</figcaption>
</figure>

As can be seen, most of the monthly contracts last for a few months, while the 2-year
contracts tend to last for years, with a great increase towards the greater values of
tenure in this dataset. This implies that customers with a great commitment at the
beginning, like a 2-year contract, tend to stay with the company for a longer period of
time. Long-term contracts usually have contractual fines. Therefore, customers have to
wait until the end of the contract to churn. It is not clear if it is the case. A
time-series data would be better to study this.

We can further check the relation between the kind of contract and the monthly and total
charges with violin plots:

<figure style="width:90%">
<img src="../project_churn_prediction_files/eda_violin_tenure.png" alt="dist_hue"/>
<figcaption>Violin plot of the Tenure by Contract type. Hue by Churn</figcaption>
</figure>

Violin plots are a combination of boxplots and KDE plots. They show the distribution of
the data and the probability density of the different values. We can see that the
most contracts are month-to-month and that customers with this kind of contract tend to
churn more (larger density of churned customers). Moreover, the churned customers tend to
churn earlier, as we saw in the tenure distribution plot.

We can check the relation between the kind of contract and the monthly charges:

<figure style="width:90%">
<img src="../project_churn_prediction_files/eda_violin_contract.png" alt="dist_hue"/>
<figcaption>Violin plot of the Monthly Charges by Contract type. Hue by Churn</figcaption>
</figure>

Again, it is clear that the month-to-month contracts have higher churn customers. The
shape of the distribution for the churned customers has a broad peak towards the higher
values of monthly charges. This implies that customers with higher monthly charges tend
to churn more. This is probably because they are not satisfied with the service or they
found a better deal.

We can now check how the type of internet service affects the churn rate:

<figure style="width:90%">
<img src="../project_churn_prediction_files/eda_violin_service_churn.png" alt="dist_hue"/>
<figcaption>Violin plot of the Monthly Charges by Internet Service. Hue by Churn</figcaption>
</figure>

As expected, fiber optic has a higher monthly charge than DSL. This is probably because
fiber optic is faster and more reliable. However, the churn rate for fiber optic is also
higher. This is most likely because customers are not satisfied with the service, even
though it is faster. We don't know when the data was collected, but some customers
perhaps didn't think that the additional speed was worth the additional cost.

### Categorical features

For the categorical features, we can check the proportion of each category:

<figure style="width:90%">
<img src="../project_churn_prediction_files/eda_categorical_percentage.png" alt="proportion"/>
<figcaption>Proportion of each category</figcaption>
</figure>

Some highlights:

- the dataset is almost equally distributed in terms of gender
- 55.0 % of the customers have month-to-month contracts
- 21.7 % of the customers do not have internet service
- 90.3 % of the customers have phone service
- there are only 16.2 % senior customers. Thus, most customers are young people (less than 65 years)
- 48.3% have a partner, but only 30 % have dependents

We see that some categorical features have 'No' and 'No internet service' (or 'No phone
service') as categories. Maybe all of them can be labeled as 'No' if the categories
provide no additional information. We can check this, plotting the churn rate by
category:

<figure style="width:90%">
<img src="../project_churn_prediction_files/eda_churn_by_category.png" alt="churn categories"/>
<figcaption>Churn distribution by category</figcaption>
</figure>

Features that seem to be positively correlated with churn:

- month to month contracts
- absence of online backup, online security, and device protection services
- absence of tech support
- being a senior citizen
- paperless billing
- pay with electronic check
- internet service by fiber optic

Features that seem to be negatively correlated with churn:

- two-year contracts
- absence of internet service
- having a partner or dependent

We will quantify these correlations soon. First, let's try to interpret the findings.

Both genders behave similarly when it comes to migrating to another service provider.

It is interesting to see that each service that has the "No internet service" category
has much lower churn rates. Maybe the internet service provided by the company has
connectivity problems, particularly the fiber optic one. It could also be that the setup
is not easy, so that those who opted not to have tech support may not be able to use the
services. And that would be more severe in senior customers. While it appears that there
are issues with the fiber optic internet, the DSL one has a much lower churn rate
despite being a slower connection.

Since the "No internet service" category provided insights, it will not be merged with
the "No" category.

Besides the visual analysis, statistical tests were performed to identify significant
differences between the groups of customers who accepted and did not accept the offer.
Mann-Whitney U tests were conducted for numerical features, while chi-square tests were
used for categorical features. The results of these tests were used to identify the most
relevant features to keep in the dataset for further analysis, performing a feature
selection [[8](https://scikit-learn.org/stable/modules/feature_selection.html)].

Considering the results of the statistical tests and the visual analysis, the following
features were dropped: `Gender`, `PhoneService` and `MultipleLines`. Also, the feature
`TotalCharges` was dropped because it is highly correlated with `MonthlyCharges` and
`Tenure`.

## Evaluating models

<figure style="width:50%">
    <img alt="ai" src="../project_credit_card_fraud_files/ai.jpg">
</figure>

After the exploratory data analysis, some models were trained to classify the
transactions as fraudulent or not. The models used were:

- Dummy Classifier (as a baseline)
- Logistic Regression
- Decision Tree
- LightGBM
- K-Nearest Neighbors
- Support Vector Machine for Classification (SVC)

A Scikit-Learn pipeline was used to preprocess the data and train the models. The
numeric features were preprocessed using power transformations, while the categorical
ones were preprocessed using one-hot encoding. Due to the imbalance in the dataset, a
stratified 5-fold cross-validation technique was used to train and evaluate the models.
Moreover, the class weight parameter was used in the models (if available) to deal with
the imbalance.

<figure style="width:90%">
<img src="../project_churn_prediction_files/pipeline_full.png" alt="pipeline models"/>
<figcaption>Scikit-Learn pipeline used to train the models</figcaption>
</figure>

The following metrics were used to evaluate the models:

- `accuracy`: the proportion of correctly classified samples;
- `balanced_accuracy`: the average of the recall of each class;
- `f1`: the harmonic mean of precision and recall;
- `precision`: the proportion of correctly classified positive samples;
- `recall`: the proportion of positive samples that were correctly classified;
- `roc_auc`: the area under the ROC curve;
- `average_precision`: the area under the precision-recall curve;
- `neg_brier_score`: the Brier score, which is the mean squared difference between the
predicted probabilities and the actual outcomes. The lower the Brier score, the better
the model;
- `f1_weighted`: the weighted average of the f1 score, which is the harmonic mean of
precision and recall. The f1 score is weighted by the number of samples in each class.

We also have some time metrics regarding the training and prediction times, and the
total time. The consolidated metrics for each model are shown below as boxplots:

<figure style="width:90%">
<img src="../project_churn_prediction_files/model_performance.png" alt="models_performance"/>
<figcaption>Consolidated metrics for each model</figcaption>
</figure>

The choice of the best model should consider some factors:

- The metrics: which model has the best performance in the metrics that are most
important for the business;
- The time: the model that has the best performance in the metrics and the lowest time
should be chosen;
- The interpretability: some models are more interpretable than others. If
interpretability is important, the model that is easier to interpret should be chosen.

It happens that the best model in terms of metrics is not always the best model in terms
of time or interpretability. So, the choice of the best model should consider all these
factors.

Concerning the metrics, some literature
[[9](https://dx.plos.org/10.1371/journal.pone.0118432)]
suggests that the area under the precision-recall curve (AUPRC, represented as
`average_precision` on Scikit-Learn) is a better metric for imbalanced datasets than the
area under the ROC curve (AUROC, or `roc_auc`). This is because the ROC curve is not
sensitive to changes in the false positive rate when the true positive rate is high. The
precision-recall curve, on the other hand, is sensitive to changes in the false positive
rate when the true positive rate is high. I already covered this in a
[previous article](https://franciscobustamante.com.br/portfolio/2022-auprc/).

The Scikit-Learn docs
[[10](https://scikit-learn.org/stable/modules/model_evaluation.html#precision-recall-f-measure-metrics)]
state that the `average_precision_score` function from the package can be used as a
AUPRC value, so it was the chosen function in this study.

We can see that the logistic regression model has the best performance in terms of the
chosen metric and is also one of the fastest models. Besides, it is a very interpretable
model. So, was chosen as the best model.

The logistic regression model was then fine-tuned using a grid search to maximize the
`average_precision` metric. The best hyperparameters found were: `penalty='elasticnet'`,
`C=0.1`, `l1_ratio=0.25`, `solver='saga'`, and `max_iter=2500`.

The final model has the following metrics:

| Metric             | Score    | Std Dev  |
|--------------------|----------|----------|
| Accuracy           | 0.752 | 0.007 |
| Balanced Accuracy  | 0.766 | 0.009 |
| F1                 | 0.630 | 0.011 |
| Precision          | 0.521 | 0.009 |
| Recall             | 0.795 | 0.018 |
| ROC AUC            | 0.846 | 0.012 |
| Average Precision  | 0.658 | 0.020 |
| F1 Weighted        | 0.764 | 0.006 |

We see that our model favors recall in detriment of precision. This is fine, as we are
more interested in capturing all the customers that are likely to churn, even if we
include some false positives.

## Model Interpretability


Model interpretability is an essential factor when selecting a model. It is important to
understand how the model makes its predictions.

Some models are more interpretable than others. For example, decision trees are very
interpretable, as we can see the rules that the model uses to make its predictions. On
the other hand, models like SVM are not very interpretable, as we cannot see the rules
that the model uses to make its predictions.

The coefficients of the logistic regression model can give us some insights into the
importance of each feature in predicting the probability of churn. 
Mathematically, the coefficients represent the change in the log-odds of the target
variable for a one-unit change in the feature. In other words, the coefficients
represent the impact of the feature on the probability of churn.

Odds are the ratio of the probability of an event happening to the probability of the
event not happening. The log-odds are the natural logarithm of the odds. The logistic
regression model uses the log-odds to make predictions.

I have made an article explaining the interpretation of the coefficients of the logistic
regression model. You can check it out [here](https://franciscobustamante.com.br/portfolio/2025-logistic-regression-coefficients/).
I will briefly explain the interpretation of the coefficients below, but if you want a
more detailed explanation, I recommend reading the article.

For a numerical feature, the coefficient represents the change in the log-odds of the
target variable for a one-unit change in the feature. For example, if the coefficient of
a numerical feature is 0.5, it means that a one-unit increase in the feature is
associated with a 0.5 increase in the log-odds of the target variable.

While for a categorical feature, the coefficient represents the change in the log-odds
of the target variable for a one-unit change from the reference category to the category
represented by the feature. Considering a binary feature, if the coefficient is 0.5, it
means that the category represented by the feature is associated with a 0.5 increase in
the log-odds of the target variable compared to the reference category. For a
multi-class feature, the interpretation is similar.

Interpreting directly the coefficients of the logistic regression model can be
difficult, especially when the features are not scaled, since log-odds is a not
intuitive measure. To make the interpretation easier, we can transform the coefficients
into odds ratios

$$ \text{odds ratio} = e^{w} $$

where *w* is the coefficient of the feature.

The odds ratio is a more intuitive measure than the log-odds, as it represents the
change in the odds of the target variable for a one-unit change in the feature. An odds
ratio greater than 1 means that the feature has a positive impact on the probability of
churn, while an odds ratio less than 1 means that the feature has a negative impact
on the probability of churn.

We can calculate the odds ratios of the coefficients of the logistic regression model and
plot them to see which features have the most impact on the probability of churn:

<figure style="width:90%">
<img src="../project_churn_prediction_files/model_odds_ratio.png" alt="interpretability"/>
<figcaption>Odds ratios of the coefficients of the logistic regression model</figcaption>
</figure>

Let's understand our results. We have two numeric features, `Tenure` and `MonthlyCharges`.
The `MonthlyCharges` feature has an odds ratio of 1, which means that a one-unit increase
in the monthly charges is associated with no change in the odds of churn. Our model is
regularized, which means that it tries to reduce the number of features to make the model
simpler and more interpretable. The `MonthlyCharges` feature has, therefore, little or no
impact on the probability of churn according to our model.

The `Tenure` feature has an odds ration of 0.45, which means that a one-unit increase in
the tenure is associated with a 0.45 decrease in the odds of churn.

We also have some categorical features, the main ones being `Contract` and
`InternetService` according to our model.
Both underwent one-hot encoding, so we have multiple coefficients for each feature.

The coefficients of the `Contract` feature represent the change in the log-odds of the target
variable for a one-unit change from the reference category to the category represented by
the feature. 

The odds ratios for each category of the `Contract` feature are as follows:

- Month-to-month: 1.95
- One year: 1
- Two year: 0.48

We can divide the odds ratio of each category by the odds ratio of the reference category
to see how much the odds of churn change for each category compared to the reference
category. The reference category can be any category, but we can choose the category
with the lowest odds ratio as the reference category. In this case, the reference category
is the two-year contract. The values we get are:

- Month-to-month: 4.06
- One year: 2.08
- Two year: 1

This means that customers with month-to-month contracts are 4.06 times more likely to
churn than customers with two-year contracts, while customers with one-year contracts are
2.08 times more likely to churn than customers with two-year contracts. This is consistent
with the exploratory data analysis we performed in the previous notebooks. We saw that
customers with month-to-month contracts are more likely to churn than customers with
one-year or two-year contracts. Now we have a quantitative measure of this relationship.

We can do the same analysis for the `InternetService` feature. The odds ratios for each
category of the `InternetService` feature are as follows:

- Fiber optic: 1.85
- No: 0.93
- DSL: 0.80

Dividing the odds ratio of each category by the odds ratio of the reference category
(the one with the lowest odds ratio), we get:

- Fiber optic: 2.31
- No: 1.16
- DSL: 1

This means that customers with fiber optic internet service are 2.31 times more likely to
churn than customers with DSL internet service, while customers with no internet service
are 1.16 times more likely to churn than customers with DSL internet service. 
It is interesting to note that customers with no internet service are more likely to churn
than customers with DSL internet service.

## Key Takeaways and Business Impact

<figure style="width:50%">
    <img alt="" src="../project_credit_card_fraud_files/conclusion.jpg">
</figure>

Customer churn is a critical issue that needs to be analyzed and predicted. Therefore, a
model that can predict customer churn and deal with a huge amount of data is
significant. In this study:

- an extensive EDA was performed
    - detailed insights were gained from the data
    - statistical tests were performed to identify the most relevant features
- pipelines were used to preprocess, train, and evaluate the models
- AUPRC was the main metric to compare models due to its literature background in imbalanced datasets
- the logistic regression model was chosen as the best model due to its performance, time, and interpretability
- the model was fine-tuned to maximize the AUPRC metric
- the coefficients of the model were interpreted to understand the impact of each feature on the probability of churn

**The results suggest that the Telco company should:**

- invest in marketing strategies targeting
    - customers with short-term contracts, trying to move them to long contracts
    - customers without online services, offering these services
    - single customers, since their churn rate is higher than that of those who have
    partners and dependents
- improve the fiber optic service
- give more attention to technical support, especially for senior citizens


## Final Thoughts

This project underscores the transformative potential of data science in tackling
real-world challenges such as churn prediction. By addressing issues like data imbalance
and carefully selecting performance metrics, we developed
a robust model that balances the trade-off between recall and precision. The integration
of interpretability tools further enhances stakeholder trust and aids in continuous
model improvement. Overall, this work demonstrates how data-driven strategies can
significantly enhance operational efficiency and reduce customer churn rates.

You can check the complete code for this project on the [GitHub
repository](https://github.com/chicolucio/churn-prediction).

Reach out to me if you have any questions or would like to discuss how these techniques
can be applied to your specific business challenges. Here are some ways to connect with
me and learn more about my work:

<div class="social-container">

<a href="https://www.linkedin.com/in/flsbustamante" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 

<a href="https://github.com/chicolucio" target="_blank"><img src="https://img.shields.io/badge/GitHub-000000?style=for-the-badge&logo=GitHub&logoColor=white" target="_blank"></a>

<a href="https://franciscobustamante.com.br" target="_blank"><img src="https://img.shields.io/badge/portfolio-333333?style=for-the-badge" target="_blank"></a>

</div>

## References

1. N. Modani, K. Dey, R. Gupta, and S. Godbole, “CDR Analysis Based Telco Churn Prediction and Customer Behavior Insights: A Case Study,” in Web Information Systems Engineering – WISE 2013, vol. 8181, X. Lin, Y. Manolopoulos, D. Srivastava, and G. Huang, Eds. Berlin, Heidelberg: Springer Berlin Heidelberg, 2013, pp. 256–269. doi: 10.1007/978-3-642-41154-0_19.
2. S. Agrawal, A. Das, A. Gaikwad, and S. Dhage, “Customer Churn Prediction Modelling Based on Behavioural Patterns Analysis using Deep Learning,” in 2018 International Conference on Smart Computing and Electronic Enterprise (ICSCEE), Shah Alam, Jul. 2018, pp. 1–6. doi: 10.1109/ICSCEE.2018.8538420.
3. I. Ullah, B. Raza, A. K. Malik, M. Imran, S. U. Islam, and S. W. Kim, “A Churn Prediction Model Using Random Forest: Analysis of Machine Learning Techniques for Churn Prediction and Factor Identification in Telecom Sector,” IEEE Access, vol. 7, pp. 60134–60149, 2019, doi: 10.1109/ACCESS.2019.2914999.
4. “Churn.” https://www.productplan.com/glossary/churn/ (accessed Jun. 22, 2022).
5. IBM. "Telco customer churn". https://www.kaggle.com/datasets/blastchar/telco-customer-churn (accessed Jun. 1, 2022)
6. Shearer C., The CRISP-DM model: the new blueprint for data mining, J Data Warehousing (2000); 5:13—22
7. J. Burez and D. Van den Poel, “Handling class imbalance in customer churn prediction,” Expert Systems with Applications, vol. 36, no. 3, pp. 4626–4636, Apr. 2009, doi: 10.1016/j.eswa.2008.05.027.
8. Scikit-Learn. "Feature selection". https://scikit-learn.org/stable/modules/feature_selection.html
9. T. Saito and M. Rehmsmeier, “The Precision-Recall Plot Is More Informative than the ROC Plot When Evaluating Binary Classifiers on Imbalanced Datasets,” PLoS ONE, vol. 10, no. 3, p. e0118432, Mar. 2015, doi: 10.1371/journal.pone.0118432.
10. Scikit-Learn. "Model evaluation". https://scikit-learn.org/stable/modules/model_evaluation.html#precision-recall-f-measure-metrics

<div class="social-container">

<a href="https://www.linkedin.com/in/flsbustamante" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 

<a href="https://github.com/chicolucio" target="_blank"><img src="https://img.shields.io/badge/GitHub-000000?style=for-the-badge&logo=GitHub&logoColor=white" target="_blank"></a>

<a href="https://franciscobustamante.com.br" target="_blank"><img src="https://img.shields.io/badge/portfolio-333333?style=for-the-badge" target="_blank"></a>

</div>
