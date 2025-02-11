---
name: Optimizing marketing campaigns with data
title: Optimizing marketing campaigns with data
image: /assets/images/portfolio_marketing_case_intro.png
position: 2
period: 2025
toc: true
mathjax: true
header:
  image: /assets/images/portfolio_marketing_case.png
  og_image: /assets/images/portfolio_marketing_case_intro.png
description: |
  Marketing campaigns play a crucial role in business growth, but their success depends
  on targeting the right customers. In this project, I analyzed a real-world case where
  a retail food company launched a direct marketing campaign that resulted in a low
  response rate and financial loss.
---

## Abstract

Effective marketing relies on targeting the right customers. This project analyzes a
retail food company’s direct marketing campaign, which suffered a low response rate
(15%) and financial loss. Using data-driven strategies, we optimized future
campaigns through customer segmentation, predictive modeling, and profit analysis.  

Customers were grouped into five segments using RFM analysis, revealing that high-value
customers (Valuable & Elite segments) were significantly more likely to convert. A
logistic regression model identified key purchase drivers, showing that Elite customers
were over 140 times more likely to buy than Dormant customers.  

Profit analysis determined the optimal targeting threshold (0.27), increasing the
**conversion rate from 15% to 32.5%** and turning losses into a projected 3,722 MU
gain. This study highlights how data-driven marketing can improve efficiency, reduce
costs, and maximize returns, providing actionable insights for businesses looking to
optimize their campaigns.

The dataset and the project notebooks, with all the code, can be seen on the following
GitHub repository:

[<center><img alt="GitHub" width="10%" src="https://github.githubassets.com/images/modules/logos_page/GitHub-Logo.png
"></center>](https://github.com/chicolucio/marketing-campaign-optimization)

## Introduction  

<figure style="justify-content: center;">
<img src="/assets/images/portfolio_marketing_case_intro.png" alt="marketing target" style="width:40%"/>
</figure>


Marketing campaigns are a critical component of business growth, but their success
hinges on targeting the right customers. The case study presented here focuses on a
real-world scenario where a retail food company launched a direct marketing campaign
that resulted in a low response rate and financial loss. Leveraging data-driven
strategies, we aim to optimize future campaigns by identifying key customer segments,
predicting purchase behavior, and maximizing profitability.

The methodology involves:

- Data cleansing and preprocessing to refine the dataset for analysis.
- Exploratory data analysis to understand customer behavior and past campaign responses.
- Customer segmentation based on RFM analysis to identify distinct behavioral patterns.
- Predictive modeling to forecast customer responses.
- Profit analysis to determine the optimal targeting strategy for future campaigns.
- Recommendations for marketing teams to enhance campaign effectiveness.

The dataset and the project notebooks, with all the code, can be seen on the following
GitHub repository:

[<center><img alt="GitHub" width="10%" src="https://github.githubassets.com/images/modules/logos_page/GitHub-Logo.png
"></center>](https://github.com/chicolucio/marketing-campaign-optimization)


### The company context

The dataset consists of customer information, purchase history, and responses to past
marketing campaigns of a retail food company. Let's understand the company context first.

They sell products from 5 major categories: wines, rare meat products, exotic fruits,
specially prepared fish and sweet products. These can further be divided into gold and
regular products. The customers can order and acquire products through 3 sales channels:
physical stores, catalogs and company’s website. Globally, the company had solid
revenues and a healthy bottom line in the past 3 years, but the profit growth
perspectives for the next 3 years are not promising... For this reason, several
strategic initiatives are being considered to invert this situation. One is to improve
the performance of marketing activities, with a special focus on marketing campaigns.

The marketing department was pressured to spend its annual budget more wisely. The CMO 
perceives the importance of having a more quantitative approach when taking decisions,
reason why a small team of data scientists was hired with a clear objective in mind: to
build a predictive model which will support direct marketing initiatives. Desirably, the
success of these activities will prove the value of the approach and convince the more
skeptical within the company. 
 
### The aim of the project

The objective of the team is to build a predictive model that will produce the highest
profit for the next direct marketing campaign, scheduled for the next month. The new
campaign, sixth, aims at selling a new gadget to the Customer Database.

To build the model, a pilot campaign involving 2.240 customers was carried out. The
customers were selected at random and contacted by phone regarding the acquisition of
the gadget.  During the following months, customers who bought the offer were properly
labeled. The success rate of the campaign was 15%. 

The objective of the team is to develop a model that predicts customer behavior and
to apply it to the rest of the customer base.  Hopefully, the model will allow the
company to cherry pick the customers who are most likely to purchase the offer while
leaving out the non-respondents, making the next campaign highly profitable. Moreover,
apart from maximizing the profit of the campaign, the CMO is interested in understanding
the characteristic features of those customers who are willing to buy the gadget.

## Understanding the Data  

The data set contains socio-demographic and firmographic features about 2.240 customers
who were contacted. Additionally, it contains a flag for those customers who responded
the pilot campaign, by buying the product. 

The following features were included in the dataset:

- Personal Data:
    - `ID`: Unique customer identifier
    - `Year_Birth`: Customer's year of birth
    - `Education`: Customer's education level
    - `Marital_Status`: Customer's marital status
    - `Income`: Customer's annual family income
    - `Kidhome`: Number of children in the customer's household
    - `Teenhome`: Number of teenagers in the customer's household
    - `Dt_Customer`: Date the customer enrolled with the company
    - `Recency`: Number of days since the customer's last purchase
    - `Complain`: 1 if the customer has complained in the last 2 years, 0 otherwise
- Product Data
    - `MntWines`: Amount spent on wine in the last 2 years
    - `MntFruits`: Amount spent on fruits in the last 2 years
    - `MntMeatProducts`: Amount spent on meat in the last 2 years
    - `MntFishProducts`: Amount spent on fish in the last 2 years
    - `MntSweetProducts`: Amount spent on sweets in the last 2 years
    - `MntGoldProds`: Amount spent on gold products in the last 2 years
- Campaign Data
    - `NumDealsPurchases`: Number of purchases made with a discount
    - `AcceptedCmp1`: 1 if the customer accepted the offer in the 1st campaign, 0 otherwise
    - `AcceptedCmp2`: 1 if the customer accepted the offer in the 2nd campaign, 0 otherwise
    - `AcceptedCmp3`: 1 if the customer accepted the offer in the 3rd campaign, 0 otherwise
    - `AcceptedCmp4`: 1 if the customer accepted the offer in the 4th campaign, 0 otherwise
    - `AcceptedCmp5`: 1 if the customer accepted the offer in the 5th campaign, 0 otherwise
    - `Response`: 1 if the customer accepted the offer in the last (pilot) campaign, 0 otherwise (*target*)
- Purchase Location Data
    - `NumWebPurchases`: Number of purchases made through the company's website
    - `NumCatalogPurchases`: Number of purchases made using a catalog
    - `NumStorePurchases`: Number of purchases made directly in stores
    - `NumWebVisitsMonth`: Number of visits to the company's website in the last month

### Data cleansing and feature engineering

The dataset was available as a CSV file, which was loaded into a Pandas DataFrame for
further analysis. The initial exploration revealed a few missing values,
which were dropped from the dataset. Additionally, some features were deemed irrelevant
for the analysis and were removed. Outliers were identified and treated accordingly to
ensure the integrity of the data.

Some features were transformed to better represent the customer's behavior, such as the
`Dt_Customer` column, which was converted to the number of days and months since the
customer enrolled with the company. This transformation allowed for a more meaningful
analysis of the recency feature. Also, the age of the customers was calculated based on
their year of birth to provide a more intuitive understanding of the data.

As described earlier, the company sells products from 5 major categories, which were
represented in the dataset as the amount spent on each category in the past 2 years.
The values of these features were summed to create a total amount spent feature, which
was used to analyze the customer's purchasing behavior in subsequent steps.

Finally, the features were divided into numerical and categorical groups for further
analysis.

### Exploratory Data Analysis

After cleaning and preprocessing the data, an exploratory data analysis was conducted to
gain insights into customer behavior and past campaign responses. The analysis included
descriptive statistics, distribution plots, and correlation matrices to identify patterns
and relationships within the data.

Concerning the numerical features, it was observed that most features have a right-skewed
distribution, indicating that a few customers spend significantly more than
the rest. The histograms below show the histograms of these features, using the target
variable `Response` to differentiate between customers who accepted the offer and those
who did not. A `Response` value of 1 indicates that the customer accepted the offer in
the pilot campaign, while a value of 0 indicates that the customer did not accept the
offer.

![histograms](../project_marketing_case_files/histograms_hue_response.png)

We see that customers who accepted the offer in the last campaign tend to have higher
expenses in most product categories, as well as a higher number of purchases and
lower recency values. This suggests that customers who are more engaged with the company
are more likely to respond positively to marketing campaigns.

For the categorical features, the distribution of education levels, marital status, among
others, was analyzed to understand the customer profile. The bar plots below show the
distribution of these features based on the `Response` variable.

![categoricals](../project_marketing_case_files/categorical_hue_response.png)

The analysis revealed that customers with higher education levels and income tend to
respond more positively to marketing campaigns. Additionally, customers with partners
and those with children in the household are less likely to accept the offer. The age
does not seem to have a significant impact on the response rate.

Some of these relationships were further explored through boxplots:


![boxplot1](../project_marketing_case_files/boxplot_education_income_hue_response.png)

![boxplot2](../project_marketing_case_files/boxplot_education_income_hue_haschildren.png)

We can see that customers with children tend to have lower incomes therefore, they are
less likely to accept the offer. The education level seems to have a significant
impact on the income of the customers.

Besides the visual analysis, statistical tests were performed to identify significant
differences between the groups of customers who accepted and did not accept the offer.
Mann-Whitney U tests were conducted for numerical features, while chi-square tests were
used for categorical features. The results of these tests were used to identify the most
relevant features to keep in the dataset for further analysis.

## Customer Segmentation: Identifying Behavioral Patterns  

Customer segmentation is a powerful technique that allows businesses to group customers
based on their behavior and preferences. In this project, we used the RFM segmentation
methodology.

RFM analysis is a customer segmentation technique that uses past purchase behavior to
divide customers into groups. RFM stands for Recency, Frequency, and Monetary Value.

- Recency: How recently a customer has made a purchase. The more recent the purchase,
the higher the score.
- Frequency: How often a customer makes a purchase. The more frequent the purchase, the
higher the score.
- Monetary Value: How much money a customer spends on purchases. The more money spent,
the higher the score.
- RFM score: A score calculated by combining the three RFM values.
- RFM segment: A segment of customers with similar RFM scores.

There are many ways to calculate the RFM score. The number of groups may vary depending
on the business needs, the size of the dataset, and the distribution of the RFM values.
Similarly, the way to calculate the RFM score may vary depending on the business needs.
And, finally, the segment labels may vary depending on the business needs.

For small and medium datasets, it is better to calculate the RFM score as a number. This
number can be calculated by summing the three RFM values. Considering a scale of 1 to 5
for each RFM value, the RFM score will be between 3 (1 + 1 + 1 = 3) and 15 (5 + 5 + 5 =
15). So, the number of possible RFM scores is 15 - 3 + 1 = 13. This is the
approach we are going to use in this project since our dataset is relatively small.

Irrespective of the way to calculate the RFM score, the RFM segment is calculated by
dividing the RFM score into groups. Each group can be labeled with a name, such as
"Champions", "Loyal Customers", "Potential Loyalists", "New Customers", "At Risk",
"Can't Lose Them", "Hibernating", "Lost Customers", etc. The number of groups and the
labels may vary depending on the business needs and the distribution of the RFM values.
The labels try to describe the behavior of the customers in each group.

For instance, in this project, we are going to consider the following RFM score
calculation:

- Each RFM value is divided into 5 equal groups, with a score of 1 to 5.
- The RFM score is calculated by the sum of the three RFM values.
- The RFM score is divided into 5 segments, that may have different marketing
strategies, as follows:

**1. Dormant (Score: 3 - 5)**
   - *Behavior:* These customers have the lowest engagement. They haven't purchased
   recently, buy infrequently, and spend little.

**2. Occasional (Score: 6 - 7)**
   - *Behavior:* Customers in this group make occasional purchases but are not highly
   engaged. They may have made a few transactions but do not shop consistently.

**3. Engaged (Score: 8 - 10)**
   - *Behavior:* These customers have moderate engagement. They purchase somewhat
   frequently, and their spending is stable.

**4. Valuable (Score: 11 - 12)**
   - *Behavior:* These are high-value customers who purchase often and spend
   significantly. They have shown consistent engagement.

**5. Elite (Score: 13 - 15)**
   - *Behavior:* The most valuable customers. They buy frequently, spend a lot, and
   have remained engaged over time.

This segmentation allows for targeted marketing strategies to maximize customer
retention and revenue growth.

This segmentation was applied to the dataset, and the customers were assigned to one of
the five segments based on their RFM scores. The distribution of scores and the number
of customers in each score is shown below:

![rfm_scores](../project_marketing_case_files/rfm_distribution_score.png)

Customers were then grouped into the five segments based on their RFM scores. The
distribution of customers across the segments is shown below:

![rmf_segments](../project_marketing_case_files/customer_rfm_segments.png)

Considering the median values of the normalized RFM scores, we can plot the distribution
of the RFM scores for each segment:

![normalized_rfm](../project_marketing_case_files/normalized_rfm.png)

We can see that the *Elite* and *Valuable* segments are the most important in terms of
recency, frequency, and monetary value. These segments represent the ones which are most
likely to respond positively to marketing campaigns and generate higher revenue for the
company.

### Profiling the Customer Segments

It is always important to understand the characteristics of each customer segment to
tailor marketing strategies effectively. One way to do this is by analyzing the key
features of each segment and how they differ in terms of engagement and spending.
We can create profiles, or personas, for each segment to better understand their
behavior and preferences.

In order to do this, let's analyze how the numerical and categorical features vary
across the five segments. First, we will look at the numerical features:

![pairplot](../project_marketing_case_files/pairplot_segments.png)

As we can see, the *Elite* and *Valuable* segments have higher spending, more frequent
purchases, and more recent transactions compared to the other segments. Also, they have
a higher income.  This indicates that these segments are more engaged with the company
and are likely to respond positively to marketing campaigns.

For the categorical features, we can use each category as a hue to see how the segments
differ in terms of education, marital status, and other characteristics:

![hue_category](../project_marketing_case_files/segments_hue_categories.png)

It is clear that the higher the RFM score the lower the tendency to have children. Also,
the most valuable segments have older customers (higher percentages for 46-60 and 61+
age groups) and higher percentages of customers with higher education levels (graduation
or higher).

The marital status is more evenly distributed among the segments, with no clear pattern.

We can as well use the segments as a hue:

![hue_segments](../project_marketing_case_files/categories_hue_segments.png)

The findings are consistent with the previous analysis. There is a clear pattern in the
distribution of the segments for the number of children, age group, and education level.
The marital status is more evenly distributed among the segments.

We can now describe our personas or customer profiles for each segment:

- **Dormant**
  - *Description:* These customers have the lowest engagement. They haven't purchased
  recently, buy infrequently, and spend little.
  - *Marketing Strategy:* Reactivation campaigns such as discounts or personalized
  offers may help re-engage them.
  - *Customer Profile:*
    - *Demographics:* Customers with children, younger age group, lower education
    level, and lower income.
    - *Behavior:* Low recency, low frequency, low monetary value.
    - *Tendency to convert:* Lowest.

- **Occasional**
  - *Description:* Customers in this group make occasional purchases but are not
  highly engaged. They may have made a few transactions but do not shop consistently.
  - *Marketing Strategy:* Encourage more frequent purchases through targeted
  promotions or loyalty incentives.
  - *Customer Profile:*
    - *Demographics:* Customers with children, younger age group, lower education
    level, and lower income.
    - *Behavior:* Mid-range recency, low frequency, low monetary value.
    - *Tendency to convert:* Mid to low.

- **Engaged**
  - *Description:* These customers have moderate engagement. They purchase somewhat
  frequently, and their spending is stable.
  - *Marketing Strategy:* Strengthen the relationship by providing personalized
  recommendations and exclusive offers.
  - *Customer Profile:*
    - *Demographics:* Customers with children, mid-age group, at least graduation as
    education level, and mid-range income.
    - *Behavior:* Mid-range recency, mid-range frequency, low monetary value.
    - *Tendency to convert:* Mid-range.

- **Valuable**
  - *Description:* These are high-value customers who purchase often and spend
  significantly. They have shown consistent engagement.
  - *Marketing Strategy:* Maintain their loyalty through rewards programs, premium
  offers, and early access to promotions.
  - *Customer Profile:*
    - *Demographics:* Customers may have children or not, older age group, higher
    education level, and higher income.
    - *Behavior:* Mid-range recency, high frequency, high monetary value.
    - *Tendency to convert:* High.

- **Elite**
  - *Description:* The most valuable customers. They buy frequently, spend a lot, and
  have remained engaged over time.
  - *Marketing Strategy:* Focus on VIP treatment, premium experiences, and strong
  retention strategies to ensure long-term loyalty.
  - *Customer Profile:*
    - *Demographics:* Customers probably without children, older age group, higher
    education level, and higher income.
    - *Behavior:* Low recency, high frequency, high monetary value.
    - *Tendency to convert:* Highest.


## Predicting Customer Responses with Machine Learning


The segmentation analysis provided valuable insights into customer behavior, but to
maximize the effectiveness of future marketing campaigns, we need to predict customer
responses. This is where machine learning comes into play.

The segments identified through RFM analysis can be used as input features, along with
the existing customer data, to build a predictive model that forecasts the likelihood of
a customer accepting the offer in the next campaign.

The Sci-Kit Learn library in Python offers a wide range of machine learning algorithms
for classification tasks. For this project, an initial evaluation of several models was
conducted with the following models:

- Dummy Classifier (Baseline)
- Logistic Regression
- Decision Tree
- LightGBM
- Support Vector Machine
- K-Nearest Neighbors

Since the target variable is imbalanced, with only 15% of customers accepting the offer
in the pilot campaign, the parameter `class_weight='balanced'` was used to account for
this imbalance in the models that support it.

The preprocessing steps included one-hot encoding and ordinal encoding for the
categorical features, and standard and power transformations for the numerical features
(only for the non-tree-based models).

A stratified 5-fold cross-validation was used to evaluate the models' performance. The
evaluation metrics considered were accuracy, precision, recall, F1-score, AUROC, and
AUPRC. The time to train and predict was also considered. The results are summarized
in the performance comparison chart below:

![models_performance](../project_marketing_case_files/models_comparison.png)

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

Concerning the metrics, some
[literature](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0118432)
suggests that the area under the precision-recall curve (AUPRC, represented as
`average_precision` on Scikit-Learn) is a better metric for imbalanced datasets than the
area under the ROC curve (AUROC, or `roc_auc`). This is because the ROC curve is not
sensitive to changes in the false positive rate when the true positive rate is high. The
precision-recall curve, on the other hand, is sensitive to changes in the false positive
rate when the true positive rate is high. I've written an article about this topic,
which can be seen [here](../2022-auprc/).

We can see that the logistic regression model has the best performance in terms of the
chosen metric and is also one of the fastest models. Besides, it is a very interpretable
model. So, we chose the logistic regression model for this project.

The chosen model was then fine-tuned using grid search to optimize the hyperparameters.

The final model had the following metrics:

| Metric            | Mean  | Std Dev |
|-------------------|-------|---------|
| Accuracy          | 0.819 | 0.006 |
| Balanced Accuracy | 0.817 | 0.020 |
| F1                | 0.576 | 0.019 |
| Precision         | 0.445 | 0.009 |
| Recall            | 0.814 | 0.042 |
| ROC AUC           | 0.898 | 0.011 |
| Average Precision | 0.669 | 0.028 |


We see that our model highly favors recall in detriment of precision. This is because
the model is giving more importance to the conversion class, which is the minority
class. It seems fine, as we are more interested in capturing the conversion cases.

The coefficients of the logistic regression model can provide insights into which factors
influence the likelihood of a customer accepting the offer. The coefficients can be
transformed into odds ratios to understand the impact of each feature on the target
variable. Below is a chart showing the odds ratios of the features:

![odds_ratios](../project_marketing_case_files/odds_ratio.png)

Using `MntMeatProducts` as an example for a numerical feature, we see that the odds
ratio is ~4.47. This means that a one-unit increase in the `MntMeatProducts` feature 
multiplies the odds of the target variable by 4.47. This corresponds to a 347% increase
in the odds (i.e., the odds become 4.47 times larger).

For `HasAcceptedCmp_1`, a binary feature, we have to consider that we have set the
OneHotEncoder to drop the first category when dealing with binary features. So, the
reference category is the category that was dropped. In this case, the reference
category is `0`, that is why there is not `HasAcceptedCmp_0` in the coefficients list.
The odds ratio is ~1.42. This means that customers who have accepted at least one
previous campaign have 1.42 times the odds of converting compared to customers who have
not accepted any campaign.

Finally, considering now a multi-class feature, `Segment`. We have set the OneHotEncoder
to drop the first category only for binary features. Since `Segment` has 5 categories,
we have 5 coefficients. It is easier to interpret the odds ratio of a multi-class
feature by comparing the odds ratio of each category to a reference category. This can
be done dividing the odds ratio of each category by the odds ratio of the reference
category. Let's consider the Dormant category as the reference category. The odds ratio
of the Dormant category is 1. 

$$
\text{Relative Odds Ratio} = \frac{\text{Odds Ratio of Segment}}{\text{Odds Ratio of Lowest Segment (Dormant)}}
$$

Applying this to your data:

| Segment  | Absolute Odds Ratio | Adjusted (Relative to Dormant) |
|----------|---------------------|--------------------------------|
| **Dormant** | 0.0619  | **1.00 (Baseline)** |
| **Occasional** | 0.3161  | $$\frac{0.3161}{0.0619} = 5.11$$ |
| **Engaged** | 1.2011  | $$\frac{1.2011}{0.0619} = 19.40$$ |
| **Valuable** | 3.9805  | $$\frac{3.9805}{0.0619} = 64.28$$ |
| **Elite** | 8.8405  | $$\frac{8.8405}{0.0619} = 142.76$$ |

- **Occasional customers** are **5.1 times** more likely to convert compared to Dormant customers.
- **Engaged customers** are **19.4 times** more likely than Dormant customers.
- **Valuable customers** are **64.3 times** more likely than Dormant customers.
- **Elite customers** are **142.8 times** more likely than Dormant customers.

These are important insights that can help the marketing team to focus on the most valuable customers and improve the conversion rate. The figures are quite impressive, showing that the segmentation of customers is a powerful tool to predict the probability of conversion.

## Maximizing Campaign Profitability

We have already built a model that predicts the probability of a customer buying the
gadget. Also, we have already segmented our customers. Now, we will use this model to
predict the profit of the campaign, aiming to maximize it.

The profit of the campaign is given by the formula:

$$
\text{Profit} = \text{Number of customers} \times \text{Probability of buying} \times \text{Revenue} - \text{Number of customers} \times \text{Cost}
$$

Where:

- Number of customers: the number of customers that we target in the campaign
- Probability of buying: the probability of a customer buying the gadget
- Revenue: the revenue that we get from selling the gadget
- Cost: the cost of the campaign
- Profit: the profit of the campaign

From the problem description, we know that:

- The revenue from selling the gadget is 11
- The cost of the campaign is 3

We can use our model to predict the probability of each customer buying the gadget.
Then, we will search for a threshold that maximizes the profit of the campaign. We will
try different thresholds and calculate the profit for each one. We will choose the
threshold that gives us the highest profit.

What is the threshold? The threshold is the probability above which we will consider a
customer as a buyer. For example, if the threshold is 0.5, we will consider that a
customer is a buyer if the probability of buying is greater than 0.5. If the threshold
is 0.7, we will consider that a customer is a buyer if the probability of buying is
greater than 0.7.

Why do we need to choose a threshold? Because the model gives us the probability of a
customer buying the gadget, not a binary answer. We need to choose a threshold to
convert the probability into a binary answer (buyer or not buyer).

If our threshold is too low, we will consider many customers as buyers, but many of them
will not buy the gadget. Our cost will be high (many customers targeted), but our
revenue will be low (few customers buying the gadget).

If our threshold is too high, we will consider few customers as buyers (low cost), and
even considering that all of them will buy the gadget, our revenue will also be low (few
customers buying the gadget).

We need to find a balance between these two situations.

For each threshold, we are going to calculate:

- the number of customers that we target in the campaign
- the expected conversion (probability of buying, we will consider as the sum of the probabilities of the customers that we target)
- the conversion rate (expected conversion divided by the number of customers that we target)
- the expected revenue (expected conversion times the revenue)
- the expected cost (number of customers that we target times the cost)
- the profit (expected revenue minus expected cost)

We can then plot the profit as a function of the threshold. The threshold that gives us
the highest profit is the optimal threshold. The chart below shows the profit as a
function of the threshold:

![profit_threshold](../project_marketing_case_files/profit_vs_threshold.png)

The optimal threshold is 0.27. This threshold gives us the highest profit for the
campaign, with a profit of 3722.18.

Using this threshold, we can have a look at the selected customers (those with a 
probability of buying equal or greater than 0.27) and see the segments they belong to.
The table below shows the conversion rates per segment at the optimal threshold:

| Segment   | Conv. Rate | Selected Cust. | Cum. % of Total | Converted Cust.|  Cum. % Converted |
|-----------|------------|--------------- |-----------------|----------------|------------------|
| Dormant   | 0.231      | 26             |  1.2            | 6              |  0.6             |
| Occasional| 0.218      | 119            |  6.6            | 25             |  3.2             |
| Engaged   | 0.275      | 309            |  20.6           | 84             |  11.9            |
| Valuable  | 0.336      | 271            |  32.9           | 91             |  21.3            |
| Elite     | 0.452      | 239            |  43.7           | 108            |  32.5            |

We see that this threshold takes 43.7% of the original customers of our dataset. This is
good, as we are not targeting too few customers (which would decrease our revenue) or
too many customers (which would increase our cost).

From the selected customers, we converted 32.5% of them into buyers. This is a good
conversion rate, as it is higher than the conversion rate of pilot campaigns (15 %). So
the segmentation and the model are working well. Also, the conversion rate per segment
is higher than the conversion rate of the pilot campaign.

Most of the selected customers are in segments "Valuable" and "Elite". These are the
segments that we are most interested in targeting, as they are the most likely to buy
the gadget (higher conversion rate).

We can plot the conversion rates per segment at the optimal threshold:

![conversion_rates](../project_marketing_case_files/conversion_rates_segments.png)


Another interesting result is that every segment has a higher conversion rate than the
pilot campaign. The pilot campaign had a conversion rate of 15%, and all segments have a
conversion rate higher than 15%.

We can think of this as the following: without the model, if we targeted all customers,
randomly, we would have a conversion rate of 15%. With the model, we can target only
43.7% of the customers and have a conversion rate of 32.5%. This is a significant
improvement. More, each segment has a conversion rate higher than 15%. So, even segments
with lower conversion rates are better than the pilot campaign.

A formal way of saying the above statement is that the model lifts the conversion rate
of the campaign. The lift is the ratio between the conversion rate of the campaign with
the model and the conversion rate of the campaign without the model. We can calculate
the lift for each segment based on the conversion rate of the pilot campaign and the
overall lift:

![lift](../project_marketing_case_files/lift_segments.png)

We see that all segments have a lift higher than 1, which means that the model is
improving the conversion rate of the campaign. The overall lift is 2.15, which means
that the model is improving the conversion rate of the campaign by 115%. The lift is
even higher for the segments "Valuable" and "Elite", which are the segments that we are
most interested in targeting.

## Key Takeaways and Business Impact

This project demonstrated how data-driven strategies can significantly enhance the
effectiveness of marketing campaigns. By leveraging customer segmentation and predictive
modeling, we identified high-value customers, optimized targeting strategies, and
improved campaign profitability.  

### Main Insights from the Analysis  

1. Customer behavior varies significantly by segment  

   - Customers were successfully segmented using RFM analysis into **Dormant,
   Occasional, Engaged, Valuable, and Elite** groups.  
   - Higher-value segments (**Valuable & Elite**) exhibit more frequent purchases,
   higher spending, and greater engagement with previous campaigns.  

2. Predictive modeling improves campaign efficiency

   - A logistic regression model was trained and fine-tuned to predict customer
   responses.  
   - Key factors influencing purchase behavior included past campaign engagement,
   spending habits, and recency of purchases.  
   - The model was able to **identify customers up to 142 times more likely to convert**
   compared to the least engaged segment.  

3. **Data-driven targeting leads to higher profitability**  

   - The optimal decision threshold (0.27) **increased the conversion rate from 15% to
   32.5%** while reducing the number of customers contacted.  
   - The campaign profit, which was negative in the pilot test, was optimized to a
   projected 3,722 MU gain.  
   - Every customer segment performed better than the original pilot campaign when
   targeted using model predictions.  

### How These Findings Improve Future Campaigns

- *More precise targeting*: By focusing on high-potential customers, marketing teams
can **reduce costs** while increasing conversion rates.  
- *Better budget allocation*: Resources can be directed towards segments with the
**highest expected return**, maximizing overall profitability.  
- *Personalized marketing*: Understanding customer segments allows for **tailored
promotions** that align with specific behaviors and preferences.  
- *Data-driven decision-making*: Predictive analytics can help **forecast campaign
performance** and refine strategies over time.  

### Actionable Recommendations for Marketing Teams  

📌 *Leverage customer segmentation*  
   - Develop targeted messaging and promotions for **Valuable and Elite** segments, as
   they have the highest likelihood of conversion.  
   - Design reactivation campaigns for **Dormant and Occasional** customers to increase
   engagement.  

📌 *Use predictive modeling to refine campaign targeting*  
   - Apply machine learning models to predict customer responses and focus marketing
   efforts on the most promising prospects.  
   - Optimize decision thresholds based on profitability rather than simple conversion
   rates.  

📌 *Incorporate profitability analysis into marketing strategy*  
   - Regularly analyze **profit vs. targeting threshold** to balance reach and expected
   return.  
   - Continuously monitor campaign performance and adjust strategies accordingly.  

📌 *Experiment with personalized offers*  
   - Tailor promotions based on customer **purchase history and engagement level** to
   increase response rates.  
   - Test different pricing strategies for **higher- and lower-engagement** customer
   groups.  

### Final Thoughts 

This case study highlights the transformative power of data in marketing
decision-making. By combining segmentation, predictive modeling, and profit analysis,
businesses can **enhance efficiency, improve customer engagement, and maximize
returns**. Companies that adopt these strategies will gain a competitive advantage in
customer retention and revenue growth.  

See the full project in the [GitHub repository](https://github.com/chicolucio/marketing-campaign-optimization).


<center>
<a href="https://www.linkedin.com/in/flsbustamante" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 
<br>
<a href="https://franciscobustamante.com.br" target="_blank"><img src="https://img.shields.io/badge/portfolio-000000?style=for-the-badge&logo=About.me&logoColor=white" target="_blank"></a>
</center>
[<center><img alt="GitHub" width="10%" src="https://github.githubassets.com/images/modules/logos_page/GitHub-Logo.png
"></center>](https://github.com/chicolucio)
