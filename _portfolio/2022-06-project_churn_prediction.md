---
name: Customer churn prediction
title: Customer churn prediction
image: https://github.com/Ciencia-Programada/articles-images/blob/master/churn/1.jpg?raw=true
position: 1
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

Churn is a measure of how many customers stop using a service or product, often evaluated for a specific period of time. One of the biggest difficulties in telecommunication industry is to retain the customers and prevent the churn.

Customers in the telecom industry can choose from a variety of service providers and actively switch from one to the next. Acquiring new customers is not only more difficult, but also much more costly to companies than maintaining existing customer relationships. Therefore, many *Customer Churn Prediction* (CCP) models have been implemented.

In this study, a churn level prediction process is carried out using machine learning. A dataset with over 7000 customers of a telecom company is used. An action plan for the company is designed based on the results.

The complete dataset and auxiliary files can be found in the project repository on GitHub linked below:

[<center><img alt="GitHub" width="10%" src="https://github.githubassets.com/images/modules/logos_page/GitHub-Logo.png
"></center>](https://github.com/chicolucio/customer-churn-prediction)

The full project notebook, with all the code, can also be seen on Deepnote:
[<img src="https://deepnote.com/buttons/launch-in-deepnote.svg">](https://deepnote.com/@flsbustamante/customer-churn-prediction-8eac729e-7ba2-4fb2-9ce0-7018e476d572)

In this article, all the discussion, results, and outputs of the Jupyter Notebook used to develop the study will be shown. The code can be seen on the links above.

## Contextualization

![churn](https://github.com/Ciencia-Programada/articles-images/blob/master/churn/1.jpg?raw=true)

The telecom industry, while in a consolidation phase in developed markets, is booming in emerging markets [[1](http://link.springer.com/10.1007/978-3-642-41154-0_19)]. With the increasing variety of service providers, telecom churn has emerged as one of the most important causes of revenue erosion for telecom operators [[2](https://ieeexplore.ieee.org/document/8538420/)]. Then, predicting churners from the demographic and behavioral data of customers has been of great interest. Acquiring new subscribers can cost up to 6 times more than retaining an existing one [[3](https://ieeexplore.ieee.org/document/8706988)].

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

Again, there is no silver bullet. But increasing the perceived value proposition of the service to current users is obviously a key point. This can be achieved in several ways, among them:

- make sure customers get the most out of the product
- recruit the right kind of customers
- price based on value
- continually add value without breaking what already works
- don't take customers for granted

In this study, an IBM dataset will be analyzed. The *Telco customer churn* data contains information about a telecom company named Telco that provide home phone and internet services to 7043 customers [[5](https://www.kaggle.com/datasets/blastchar/telco-customer-churn)]. It indicates which customers have left, stayed, or signed up for their service. Multiple important demographics and services are included for each customer, totaling 20 features. The aim is to predict behavior to retain customers.

A cross-industry standard process for data mining (CRISP-DM) [[6](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining#cite_note-Shearer00-1)] approach will be used as shown in Figure 1.

<figure>
<img src="https://github.com/chicolucio/customer-churn-prediction/blob/master/images/diagram01.png?raw=true" alt="diagram crips" style="width:100%"/>
<figcaption align="center">Figure 1. Proposed methodology flow diagram</figcaption>
</figure>

Each step will be detailed in the appropriate section.


## Loading libraries

The following libraries were used. A Conda environment file is available at the project repository at GitHub [[7](https://github.com/chicolucio/customer-churn-prediction)] so that the exact development environment can be replicated by anyone.

    Versions of the packages:
    
    -------------------- | ----------
          Package        |  Version  
    -------------------- | ----------
    Imbalanced-Learn     |      0.9.1
    Matplotlib           |      3.5.1
    NumPy                |     1.22.3
    Pandas               |      1.4.2
    Scikit-Learn         |      1.1.1
    Seaborn              |     0.11.2
    
    Python version: 3.9.12

In this article all the results and outputs of the Jupyter Notebook used to develop the study will be shown. The complete code can be found on the project repository [[7](https://github.com/chicolucio/customer-churn-prediction)].

## Summarizing data and understanding the problem in hand

![eda](https://github.com/Ciencia-Programada/articles-images/blob/master/churn/2.jpg?raw=true)

### Data dictionary

Each row represents a customer, and each column contains the customer's attributes, as described below. The dataset includes information about:

- Customers who left within the last month:
    - `Churn`: *Yes* = the customer left the company within the last month. *No* = the customer remained with the company.
- Customers' demographic info:
    - `gender`: customer's gender: *Male*, *Female*
    - `SeniorCitizen`: customer is 65 or older: *1*, *0* (meaning *Yes* and *No*, respectively)
    - `Partner`: customer is married: *Yes*, *No*
    - `Dependents`: customer lives with any dependents: *Yes*, *No*. Dependents could be children, parents, grandparents, etc.
- Services that each customer has signed up for:
    - `PhoneService`: customer subscribes to home phone service with the company: *Yes*, *No*
    - `MultipleLines`: customer subscribes to multiple telephone lines with the company: *Yes*, *No*, *No internet service*
    - `InternetService`: customer subscribes to Internet service with the company: *No*, *DSL*, *Fiber Optic*
    - `OnlineSecurity`: customer subscribes to an additional online security service provided by the company: *Yes*, *No*, *No internet service*
    - `OnlineBackup`: customer subscribes to an additional online backup service provided by the company: *Yes*, *No*, *No internet service*
    - `DeviceProtection`: customer subscribes to an additional device protection plan for their Internet equipment provided by the company: *Yes*, *No*, *No internet service*
    - `TechSupport`: customer subscribes to an additional technical support plan from the company with reduced wait times: *Yes*, *No*, *No internet service*
    - `StreamingTV`: customer uses their Internet service to stream television programming from a third-party provider: *Yes*, *No*, *No internet service*
    - `StreamingMovies`: customer uses their Internet service to stream movies from a third-party provider: *Yes*, *No*, *No internet service*
- Customer account information:
    - `tenure`: total number of months that the customer has been with the company.
    - `Contract`: customer's current contract type: *Month-to-Month*, *One Year*, *Two Year*.
    - `PaperlessBilling`: customer has chosen paperless billing: *Yes*, *No*
    - `PaymentMethod`: how the customer pays their bill: *Electronic check*, *Credit Card*, *Mailed Check*, *Bank transfer*
    - `MonthlyCharge`: customer's current total monthly charge for all their services from the company
    - `TotalCharges`: customer's total charges, calculated to the end of the quarter
- Finally, each customer has a `CustomerID`, a unique ID that identifies the customer.

### What problem we have and which metric to use?

Based on data dictionary, it is a classification problem.
- `Churn` is the target variable
- It is a binary (*yes* or *no*) classification problem

We will start our study with data wrangling to:
- structure and organize the data
- clean the data
    - wrong data types
    - remove duplicates
- enrich the data
    - decide how to deal with empty entries (if any)

Then, we will perform an exploratory data analysis (EDA) to:
- identify categorical and non-categorical features
- visualize the distribution of each feature
- identify correlations
- identify outliers
- check the need for data transformations
- check the balance of the target variable
    - as we will see, the target is imbalanced

After the EDA, we are going to evaluate some machine learning algorithms to see which one give the best churn prediction. Scikit-Learn pipelines will be used with preprocessing steps (standardization and encoding). Some algorithms may have problems with imbalanced targets [[8](https://linkinghub.elsevier.com/retrieve/pii/S0957417408002121)]. So, some strategies will be evaluated to deal with the imbalance and added to the preprocessing steps. After the preprocessing steps, each model will be added to the pipeline and evaluated through cross validation.

Imbalanced data also have consequences in the choice of the evaluation metrics. Although the dataset is not heavily imbalanced, some known problems arise when dealing with such cases, mainly the accuracy paradox [[9](https://en.wikipedia.org/wiki/Accuracy_paradox)] and the unreliability of the ROC curves. Based on the literature, the chosen metric is the area under the precision-recall curve [[10](https://dx.plos.org/10.1371/journal.pone.0118432)]. A more detailed explanation will be given at the appropriate section.

With the chosen metric, a grid search will be performed to tune the hyperparameters of selected algorithms to maximize the score of such metric. Finally, a critical analysis of the results will be made with some proposed actions to address the features that have more correlation with churn.

We can now enhance our methodology flow chart with all these steps, as shown in Figure 2:

<figure>
<img src="https://github.com/chicolucio/customer-churn-prediction/blob/master/images/diagram02.png?raw=true" alt="flow chart" style="width:100%"/>
<figcaption align="center">Figure 2. Methodology flow diagram</figcaption>
</figure>



### Basic info and data wrangling

Let's get some basic info about our dataset to get familiarized with it. The following table has the first 5 entries of the dataset:


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>customerID</th>
      <th>gender</th>
      <th>SeniorCitizen</th>
      <th>Partner</th>
      <th>Dependents</th>
      <th>tenure</th>
      <th>PhoneService</th>
      <th>MultipleLines</th>
      <th>InternetService</th>
      <th>OnlineSecurity</th>
      <th>OnlineBackup</th>
      <th>DeviceProtection</th>
      <th>TechSupport</th>
      <th>StreamingTV</th>
      <th>StreamingMovies</th>
      <th>Contract</th>
      <th>PaperlessBilling</th>
      <th>PaymentMethod</th>
      <th>MonthlyCharges</th>
      <th>TotalCharges</th>
      <th>Churn</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7590-VHVEG</td>
      <td>Female</td>
      <td>0</td>
      <td>Yes</td>
      <td>No</td>
      <td>1</td>
      <td>No</td>
      <td>No phone service</td>
      <td>DSL</td>
      <td>No</td>
      <td>Yes</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>Month-to-month</td>
      <td>Yes</td>
      <td>Electronic check</td>
      <td>29.85</td>
      <td>29.85</td>
      <td>No</td>
    </tr>
    <tr>
      <th>1</th>
      <td>5575-GNVDE</td>
      <td>Male</td>
      <td>0</td>
      <td>No</td>
      <td>No</td>
      <td>34</td>
      <td>Yes</td>
      <td>No</td>
      <td>DSL</td>
      <td>Yes</td>
      <td>No</td>
      <td>Yes</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>One year</td>
      <td>No</td>
      <td>Mailed check</td>
      <td>56.95</td>
      <td>1889.5</td>
      <td>No</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3668-QPYBK</td>
      <td>Male</td>
      <td>0</td>
      <td>No</td>
      <td>No</td>
      <td>2</td>
      <td>Yes</td>
      <td>No</td>
      <td>DSL</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>Month-to-month</td>
      <td>Yes</td>
      <td>Mailed check</td>
      <td>53.85</td>
      <td>108.15</td>
      <td>Yes</td>
    </tr>
    <tr>
      <th>3</th>
      <td>7795-CFOCW</td>
      <td>Male</td>
      <td>0</td>
      <td>No</td>
      <td>No</td>
      <td>45</td>
      <td>No</td>
      <td>No phone service</td>
      <td>DSL</td>
      <td>Yes</td>
      <td>No</td>
      <td>Yes</td>
      <td>Yes</td>
      <td>No</td>
      <td>No</td>
      <td>One year</td>
      <td>No</td>
      <td>Bank transfer (automatic)</td>
      <td>42.30</td>
      <td>1840.75</td>
      <td>No</td>
    </tr>
    <tr>
      <th>4</th>
      <td>9237-HQITU</td>
      <td>Female</td>
      <td>0</td>
      <td>No</td>
      <td>No</td>
      <td>2</td>
      <td>Yes</td>
      <td>No</td>
      <td>Fiber optic</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>Month-to-month</td>
      <td>Yes</td>
      <td>Electronic check</td>
      <td>70.70</td>
      <td>151.65</td>
      <td>Yes</td>
    </tr>
  </tbody>
</table>
</div>


Let's check the size of the dataset:

    Number of instances (rows):       7043
    Number of attributes (columns):    21

The dataset has the following problems that were solved (see [notebook on GitHub](https://github.com/chicolucio/customer-churn-prediction) for details):

- inconsistent representation of the categorical column `SeniorCitizen`
- wrong data type, `object` in `TotalCharges` due to spaces (` `) that should be null entries
  - the spaces were converted to `NaN` revealing that 11 entries were null ones
  - these null entries were filled with the median value of the column
  - the final data type of the column was `float64`, as expect for a numeric column

After the due wrangling of the dataset, we can generate descriptive statistics to all numeric columns:


<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>tenure</th>
      <th>MonthlyCharges</th>
      <th>TotalCharges</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>7043.00</td>
      <td>7043.00</td>
      <td>7032.00</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>32.37</td>
      <td>64.76</td>
      <td>2283.30</td>
    </tr>
    <tr>
      <th>std</th>
      <td>24.56</td>
      <td>30.09</td>
      <td>2266.77</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.00</td>
      <td>18.25</td>
      <td>18.80</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>9.00</td>
      <td>35.50</td>
      <td>401.45</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>29.00</td>
      <td>70.35</td>
      <td>1397.47</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>55.00</td>
      <td>89.85</td>
      <td>3794.74</td>
    </tr>
    <tr>
      <th>max</th>
      <td>72.00</td>
      <td>118.75</td>
      <td>8684.80</td>
    </tr>
  </tbody>
</table>
</div>


As expected, `TotalCharges` has a broad range of values. 


Now, we can check the unique values for each column. This is a great way to understand categorical features:


    customerID          7043
    gender                 2
    SeniorCitizen          2
    Partner                2
    Dependents             2
    tenure                73
    PhoneService           2
    MultipleLines          3
    InternetService        3
    OnlineSecurity         3
    OnlineBackup           3
    DeviceProtection       3
    TechSupport            3
    StreamingTV            3
    StreamingMovies        3
    Contract               3
    PaperlessBilling       2
    PaymentMethod          4
    MonthlyCharges      1585
    TotalCharges        6531
    Churn                  2
    dtype: int64



The demographics columns have two categories each, while most of the services related columns have three. We can extract more information of these columns:



<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>customerID</th>
      <th>gender</th>
      <th>SeniorCitizen</th>
      <th>Partner</th>
      <th>Dependents</th>
      <th>PhoneService</th>
      <th>MultipleLines</th>
      <th>InternetService</th>
      <th>OnlineSecurity</th>
      <th>OnlineBackup</th>
      <th>DeviceProtection</th>
      <th>TechSupport</th>
      <th>StreamingTV</th>
      <th>StreamingMovies</th>
      <th>Contract</th>
      <th>PaperlessBilling</th>
      <th>PaymentMethod</th>
      <th>Churn</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
      <td>7043</td>
    </tr>
    <tr>
      <th>unique</th>
      <td>7043</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>2</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>3</td>
      <td>2</td>
      <td>4</td>
      <td>2</td>
    </tr>
    <tr>
      <th>top</th>
      <td>7590-VHVEG</td>
      <td>Male</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>Yes</td>
      <td>No</td>
      <td>Fiber optic</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>No</td>
      <td>Month-to-month</td>
      <td>Yes</td>
      <td>Electronic check</td>
      <td>No</td>
    </tr>
    <tr>
      <th>freq</th>
      <td>1</td>
      <td>3555</td>
      <td>5901</td>
      <td>3641</td>
      <td>4933</td>
      <td>6361</td>
      <td>3390</td>
      <td>3096</td>
      <td>3498</td>
      <td>3088</td>
      <td>3095</td>
      <td>3473</td>
      <td>2810</td>
      <td>2785</td>
      <td>3875</td>
      <td>4171</td>
      <td>2365</td>
      <td>5174</td>
    </tr>
  </tbody>
</table>
</div>



Now we know that most customers:
- are male
- are younger than 65 years
- have no partner and no dependents
- have phone service, with a single line
- have fiber optic internet service
- do not have online services (security, backup, device protection)
- do not have tech support
- do not have streaming services
- have monthly contracts
- have paperless billing and pay with electronic check

The target feature `Churn` has `No` as the most frequent value, occurring 5174 times.

It's time to get visual.

### Data visualization

We are interested in the churn rate, so let's start with the target column. Let's find out the proportion of churn:


    
![png](../project_churn_prediction_files/project_churn_prediction_47_0.png)
    


A little more than one quarter of the customers left the company in the last month. A high number that we need to understand.

First, we will look at the numerical features. Starting with histograms to search for patterns in the distributions:


    
![png](../project_churn_prediction_files/project_churn_prediction_49_0.png)
    


The tenure distribution has an interesting shape. Most customers have been with the company for just a few months, but also many have been for about 72 months (maximum value for tenure). This is probably related with different contracts, something that we will check soon. Probably some marketing campaign was ran recently to capture new customers due to the high number of customers with few months.

We can see that most customers pay low monthly charges, but there is a great fraction with medium values. Since most customers have been with the company for just a few months, the total charges plot shows most customers with low values.

Let's check if the tenure distribution has some kind of relation with the kind of contract:


    
![png](../project_churn_prediction_files/project_churn_prediction_52_0.png)
    


As can be seen, most of the monthly contracts last for a few months, while the 2 years contracts tend to last for years, with a great increase towards the greater values of tenure in this dataset. This implies that customers with a great commitment at the beginning, like a 2-year contract, tend to stay with the company for a longer period of time. Long-term contracts usually have contractual fines. Therefore, customers have to wait until the end of the contract to churn. It is not clear if it is the case. A time-series data would be better to study this.

As seem before, we have numerical and categorical columns. Let's start to see how our target variable relates with our numerical features.


    
![png](../project_churn_prediction_files/project_churn_prediction_57_0.png)
    


The above plot shows that short tenure (recent) customers have higher churn rates. Moreover, the higher the monthly charge, the higher the churn rate.


    
![png](../project_churn_prediction_files/project_churn_prediction_59_0.png)
    


The boxplots show that the churn rate is higher among customers with low tenure and high monthly charges. In details:
- the median tenure for customers who have left is around 10 months, while it is around 40 months for those who have stayed with the company
- the median monthly charge for customers who have churned is around 80, while it is around 65 for those who have not churned
- since most customers who have churned spent less time with the company, they have low total charges compared with those who have stayed
    - There are many outliers in the total charges boxplot of customers who have churned. It is not clear the cause, but it could be wrong billing or expensive services that guided the customers away from the company.

Now the categorical features. Let's find out the proportion of each category:


    
![png](../project_churn_prediction_files/project_churn_prediction_62_0.png)
    


Some highlights:

- the dataset is almost equally distributed in terms of gender
- 55.0 % of the customers have month-to-month contracts
- 21.7 % of the customers do not have internet service
- 90.3 % of the customers have phone service
- there are only 16.2 % senior customers. Thus, most customers are young people (less than 65 years)
- 48.3% have a partner, but only 30 % have dependents

We see that some categorical features have 'No' and 'No internet service' (or 'No phone service') as categories. Maybe all of them can be labeled as 'No' if the categories provide no additional information. We can check this, plotting the churn rate by category:


    
![png](../project_churn_prediction_files/project_churn_prediction_64_0.png)
    


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

It is interesting to see that each service that has the "No internet service" category has much lower churn rates. Maybe the internet service provided by the company has connectivity problems, particularly the fiber optic one. It could also be that the setup is not easy, so that those who opted not to have tech support may not be able to use the services. And that would be more severe in senior customers. While it seems that there are issues with the fiber optic internet, the DSL one has a much lower churn rate despite being a slower connection.

Since the "No internet service" category provided insights, it will not be merged with the "No" category.

We can explore more details about the internet service:


    
![png](../project_churn_prediction_files/project_churn_prediction_66_0.png)
    


It's interesting that customers with DSL and higher monthly charges have lower churn rate.

The data suggest that the more people at the customers' places, the less churn. Customers with partners or dependents have lower churn rates. Probably because more people are involved in the decision of leaving, making it more difficult.

For marketing reasons, we can see how many customers who have partners also have dependents. Assuming that, in most cases, "dependents" mean children, it is more likely that customers with partners will also have dependents. Let's check this assumption.

    
![png](../project_churn_prediction_files/project_churn_prediction_73_0.png)
    


Almost half of the customers with partners have dependents. Again, assuming that in most cases "dependents" mean children, this means that marketing campaigns which aim at avoiding churn may focus on single people.

To quantify correlation, we need to convert categorical variables into indicators. The `get_dummies` Pandas method was used to convert. With the indicators, we can plot the correlation of each categorical and numerical feature with the target:


    
![png](../project_churn_prediction_files/project_churn_prediction_77_0.png)
    


The correlation plot confirms the trends we have seen before:
- strong negative correlation with churn:
    - tenure
    - two years contract
    - no internet service
- strong positive correlation with churn:
    - month to month contract
    - no online services (security, tech support, backup, device protection)
    - fiber optic internet
    - electronic check payment

We could use these pieces of information to perform a feature selection [[11](https://scikit-learn.org/stable/modules/feature_selection.html)]. We are not going through this path here, but is something to explore in a future work. Feature selection can be used to reduce sample sets to improve scores or to boost performance on very high-dimensional datasets. The dataset in hand is not so large, then the feature selection path is left aside for a future study.

## Data transforms

![transforms](https://github.com/Ciencia-Programada/articles-images/blob/master/churn/3.jpg?raw=true)

Before evaluating some machine learning algorithms, we need to do some data transforms as follows:
- the numerical features will be standardized to get the same range. This benefits some algorithms.
    - we are going to use `StandardScaler` from Scikit-Learn
- the categorical features will be encoded to be a suitable input to the algorithms
    - we are going to use `OneHotEncoder` from Scikit-Learn
- the target column will be encoded
    - we are going to use `LabelEncoder` from Scikit-Learn

The `StandardScaler` and the `OneHotEncoder` will be part of a preprocessing step on a Scikit-Learn pipeline in all following procedures.

After the transforms, the dataset will be split in two, one for features and the other for the target.

Full details with code can be seen on the [GitHub repository](https://github.com/chicolucio/customer-churn-prediction).


## Evaluate algorithms

![algorithms_image](https://github.com/Ciencia-Programada/articles-images/blob/master/churn/4.jpg?raw=true)

We need a reference to begin our model evaluation. A `DummyClassifier` instance will be used to create a frame of reference. The classifier was configured so that it will always classify as the minority class, 1 ('Churn') in our case, a common configuration with imbalanced datasets.

In order to test the efficiency of a given classifier, we need to:
- split the data in a train and a test sets
    - the model will be evaluated with the test set (data that it has not dealt with before)
    - in an imbalanced dataset, the split should be stratified to maintain the same class distribution in each subset
    - the splits will be repeated with random samples
- perform a cross-validation to avoid overfitting

Since we are going to do these steps with our real models, they will be used with our reference too. The results for the dummy classifier:

        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.265    |   0.000   
    Precision       |   0.265    |   0.000   
    Recall          |   1.000    |   0.000   
    F1 score        |   0.419    |   0.000   
    F2 score        |   0.644    |   0.000   
    AUROC           |   0.500    |   0.000   
    AUPRC           |   0.265    |   0.000   


There are 7 metrics in the score table above. The accuracy value is exactly the proportion of churn in the dataset, since we adopted a constant strategy. The recall score is also consistent with the constant strategy. But which metric is more suitable to our case?

### Which metric to use?

Since we have an imbalanced dataset, accuracy might not be the best metric. This is known as the accuracy paradox [[9](https://en.wikipedia.org/wiki/Accuracy_paradox)]. In our dataset, if a model predicts every example as zero (no churn), it will have an accuracy of 73.5 %, which seems a high score but "predicts" only based on the majority class.

A common metric to compare models is the area under the ROC curve. However, as described in the literature [[10](https://dx.plos.org/10.1371/journal.pone.0118432)], it is also a metric that can be misleading with imbalanced data.

There are many approaches described in the literature. We don't have any information regarding costs (marketing costs, revenue lost due to churn) in this dataset, so we can't make a cost analysis. Then, remains the choice among metrics that deal with precision and recall like these metrics themselves, F-beta score and area under precision-recall curve (AUPRC). It will be considered that recall is more important than precision in this study, but we don't want low precision scores. This means that we are considering more important to predict real churn, minimizing false negatives, but we want to avoid increasing too much false positives. Considering these restrictions, we have F-beta score, with beta greater than one, and AUPRC. The values of all metrics will be shown, but the chosen one is AUPRC.

The Scikit-Learn docs [[12](https://scikit-learn.org/stable/modules/model_evaluation.html#precision-recall-f-measure-metrics)] state that the `average_precision_score` function from the package can be used as a AUPRC value, so it was the chosen function in this study.

Below, there are the models that will be tested. For the initial screening, there will be no modification of default parameters values, besides for those that have the option to set as a binary classifier. Those that have random behavior were seeded with a fixed value for reproducibility.


    [('LR', LogisticRegression(max_iter=10000)),
     ('LDA', LinearDiscriminantAnalysis()),
     ('KNN', KNeighborsClassifier()),
     ('CART', DecisionTreeClassifier(random_state=42)),
     ('NB', GaussianNB()),
     ('SVC', SVC(random_state=42)),
     ('RF', RandomForestClassifier(random_state=42)),
     ('SGD', SGDClassifier(loss='modified_huber', random_state=42)),
     ('LGBM', LGBMClassifier(objective='binary', random_state=42)),
     ('XGB',
      XGBClassifier(tree_method='hist', objective='binary:logistic))]

As previously stated, each model will be added to a pipeline after the preprocessing step (standardization and encoding).

Full details with code can be seen on the [GitHub repository](https://github.com/chicolucio/customer-churn-prediction).

Results:

    LR
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.804    |   0.010   
    Precision       |   0.657    |   0.022   
    Recall          |   0.552    |   0.027   
    F1 score        |   0.599    |   0.022   
    F2 score        |   0.570    |   0.025   
    AUROC           |   0.724    |   0.014   
    AUPRC           |   0.482    |   0.020   
    
    LDA
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.798    |   0.009   
    Precision       |   0.637    |   0.019   
    Recall          |   0.555    |   0.026   
    F1 score        |   0.593    |   0.020   
    F2 score        |   0.570    |   0.023   
    AUROC           |   0.720    |   0.013   
    AUPRC           |   0.472    |   0.017   
    
    KNN
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.763    |   0.009   
    Precision       |   0.558    |   0.017   
    Recall          |   0.525    |   0.025   
    F1 score        |   0.541    |   0.020   
    F2 score        |   0.531    |   0.022   
    AUROC           |   0.687    |   0.013   
    AUPRC           |   0.419    |   0.015   
    
    CART
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.724    |   0.009   
    Precision       |   0.481    |   0.016   
    Recall          |   0.500    |   0.022   
    F1 score        |   0.490    |   0.017   
    F2 score        |   0.496    |   0.020   
    AUROC           |   0.652    |   0.011   
    AUPRC           |   0.373    |   0.011   
    
    NB
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.696    |   0.010   
    Precision       |   0.460    |   0.010   
    Recall          |   0.843    |   0.023   
    F1 score        |   0.595    |   0.011   
    F2 score        |   0.722    |   0.016   
    AUROC           |   0.743    |   0.011   
    AUPRC           |   0.430    |   0.010   
    
    SVC
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.801    |   0.011   
    Precision       |   0.670    |   0.028   
    Recall          |   0.493    |   0.028   
    F1 score        |   0.568    |   0.026   
    F2 score        |   0.521    |   0.027   
    AUROC           |   0.703    |   0.015   
    AUPRC           |   0.465    |   0.022   
    
    RF
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.789    |   0.007   
    Precision       |   0.636    |   0.020   
    Recall          |   0.483    |   0.023   
    F1 score        |   0.549    |   0.018   
    F2 score        |   0.507    |   0.021   
    AUROC           |   0.692    |   0.011   
    AUPRC           |   0.444    |   0.014   
    
    SGD
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.762    |   0.020   
    Precision       |   0.612    |   0.099   
    Recall          |   0.458    |   0.222   
    F1 score        |   0.471    |   0.157   
    F2 score        |   0.458    |   0.196   
    AUROC           |   0.665    |   0.071   
    AUPRC           |   0.406    |   0.053   
    
    LGBM
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.797    |   0.010   
    Precision       |   0.642    |   0.021   
    Recall          |   0.528    |   0.031   
    F1 score        |   0.579    |   0.025   
    F2 score        |   0.547    |   0.028   
    AUROC           |   0.711    |   0.016   
    AUPRC           |   0.465    |   0.020   
    
    XGB
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.783    |   0.009   
    Precision       |   0.607    |   0.020   
    Recall          |   0.515    |   0.033   
    F1 score        |   0.557    |   0.025   
    F2 score        |   0.531    |   0.030   
    AUROC           |   0.697    |   0.016   
    AUPRC           |   0.442    |   0.019   
    


    
![png](../project_churn_prediction_files/project_churn_prediction_95_0.png)
    


All models performed better than our dummy model reference considering our AUPRC metric. However, the values are still low. There is probably room for improvement. Many machine learning algorithms are prone to underperform with imbalanced datasets. We can also tune each model hyperparameter.

Regarding imbalance, there are many strategies to deal with this problem:

- Data sampling
    - Undersampling: delete or select a subset of examples from the majority class
    - Oversampling: duplicate examples in the minority class or synthesize new examples from the ones in the minority class
- Select an algorithm which is less affected, or not at all, to imbalance
- Collect more data, if possible
- Select a model that penalizes classification errors in minority class
- Select a metric that has more focus on the minority class

Since we have a static dataset, we can't collect more data. However, we can try all the other strategies. We have already discussed the metric that will be used. In the next sections, sampling strategies will be evaluated to deal with the imbalance problem, and a grid search for hyperparameters tuning will be performed in selected models.

### Undersampling

Our first strategy is using `RandomUnderSampler` from the Imbalanced Learn library. This sampler applies a random undersampling technique, which randomly delete examples in the majority class. The sampler will be added to the pipeline of steps to be applied after the preprocessing and before the model. Importantly, the change to the class distribution is only applied to the training dataset. The intent is to influence the fit of the models. The sampling is not applied to the test or holdout datasets used to evaluate the performance of a model.

Full details with code can be seen on the [GitHub repository](https://github.com/chicolucio/customer-churn-prediction).

Results:

    LR
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.745    |   0.006   
    Precision       |   0.512    |   0.008   
    Recall          |   0.804    |   0.020   
    F1 score        |   0.625    |   0.010   
    F2 score        |   0.721    |   0.014   
    AUROC           |   0.763    |   0.009   
    AUPRC           |   0.464    |   0.009   
    
    LDA
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.741    |   0.007   
    Precision       |   0.508    |   0.009   
    Recall          |   0.805    |   0.021   
    F1 score        |   0.622    |   0.010   
    F2 score        |   0.720    |   0.015   
    AUROC           |   0.761    |   0.009   
    AUPRC           |   0.460    |   0.010   
    
    KNN
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.704    |   0.008   
    Precision       |   0.466    |   0.008   
    Recall          |   0.794    |   0.019   
    F1 score        |   0.587    |   0.009   
    F2 score        |   0.696    |   0.013   
    AUROC           |   0.733    |   0.009   
    AUPRC           |   0.425    |   0.008   
    
    CART
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.679    |   0.012   
    Precision       |   0.433    |   0.014   
    Recall          |   0.682    |   0.032   
    F1 score        |   0.530    |   0.018   
    F2 score        |   0.611    |   0.025   
    AUROC           |   0.680    |   0.016   
    AUPRC           |   0.380    |   0.014   
    
    NB
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.686    |   0.009   
    Precision       |   0.451    |   0.009   
    Recall          |   0.854    |   0.021   
    F1 score        |   0.590    |   0.011   
    F2 score        |   0.725    |   0.015   
    AUROC           |   0.739    |   0.011   
    AUPRC           |   0.424    |   0.010   
    
    SVC
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.743    |   0.006   
    Precision       |   0.510    |   0.007   
    Recall          |   0.794    |   0.020   
    F1 score        |   0.621    |   0.011   
    F2 score        |   0.714    |   0.015   
    AUROC           |   0.759    |   0.010   
    AUPRC           |   0.460    |   0.010   
    
    RF
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.740    |   0.009   
    Precision       |   0.507    |   0.011   
    Recall          |   0.764    |   0.021   
    F1 score        |   0.610    |   0.014   
    F2 score        |   0.694    |   0.017   
    AUROC           |   0.748    |   0.012   
    AUPRC           |   0.450    |   0.013   
    
    SGD
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.686    |   0.070   
    Precision       |   0.472    |   0.076   
    Recall          |   0.727    |   0.192   
    F1 score        |   0.546    |   0.037   
    F2 score        |   0.633    |   0.110   
    AUROC           |   0.699    |   0.029   
    AUPRC           |   0.402    |   0.022   
    
    LGBM
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.740    |   0.008   
    Precision       |   0.507    |   0.010   
    Recall          |   0.783    |   0.026   
    F1 score        |   0.615    |   0.013   
    F2 score        |   0.706    |   0.019   
    AUROC           |   0.754    |   0.011   
    AUPRC           |   0.454    |   0.011   
    
    XGB
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.729    |   0.009   
    Precision       |   0.494    |   0.010   
    Recall          |   0.761    |   0.022   
    F1 score        |   0.599    |   0.011   
    F2 score        |   0.687    |   0.016   
    AUROC           |   0.739    |   0.010   
    AUPRC           |   0.439    |   0.010   
    



    
![png](../project_churn_prediction_files/project_churn_prediction_101_0.png)
    


As can be seen, recall values increased in all models, with decrease in precision values, since there is a trade-off between these metrics. Our models become better classifying the minority class with less false negatives but more false positives.

We have chosen the metric AUPRC. We are going to select 4 models with higher AUPRC to tune the hyperparameters: LR, LDA, SVC, LGBM.

#### Tuning hyperparameters - random undersampling

A grid search will be performed to find the best combination of hyperparameters for each model. 

Full details with code can be seen on the [GitHub repository](https://github.com/chicolucio/customer-churn-prediction).

Results:

    LR_l1
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.744    |   0.005   
    Precision       |   0.511    |   0.006   
    Recall          |   0.805    |   0.020   
    F1 score        |   0.625    |   0.009   
    F2 score        |   0.722    |   0.014   
    AUROC           |   0.763    |   0.008   
    AUPRC           |   0.463    |   0.008   
    
    LR_l2
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.743    |   0.005   
    Precision       |   0.511    |   0.006   
    Recall          |   0.805    |   0.019   
    F1 score        |   0.625    |   0.008   
    F2 score        |   0.721    |   0.013   
    AUROC           |   0.763    |   0.008   
    AUPRC           |   0.463    |   0.008   
    
    LDA
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.742    |   0.010   
    Precision       |   0.509    |   0.012   
    Recall          |   0.794    |   0.023   
    F1 score        |   0.621    |   0.013   
    F2 score        |   0.714    |   0.017   
    AUROC           |   0.759    |   0.012   
    AUPRC           |   0.459    |   0.013   
    
    SVC
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.705    |   0.014   
    Precision       |   0.469    |   0.015   
    Recall          |   0.829    |   0.020   
    F1 score        |   0.599    |   0.014   
    F2 score        |   0.718    |   0.015   
    AUROC           |   0.745    |   0.012   
    AUPRC           |   0.434    |   0.014   
    
    LGBM
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.752    |   0.007   
    Precision       |   0.523    |   0.009   
    Recall          |   0.777    |   0.029   
    F1 score        |   0.625    |   0.013   
    F2 score        |   0.708    |   0.020   
    AUROC           |   0.760    |   0.012   
    AUPRC           |   0.465    |   0.011   
    



    
![png](../project_churn_prediction_files/project_churn_prediction_114_0.png)
    


As can be seen, LGBM performed slightly better than LR considering the AUPRC metric. Given that LR is usually easier to explain to all stakeholders and faster to implement, maybe is the best option for production. The LR algorithm gives a slightly higher recall (and slightly lower precision). Here, we will stay with LGBM.

Let's take a look at the 10 most important features according to the tuned LGBM classifier:


    
![png](../project_churn_prediction_files/project_churn_prediction_118_0.png)
    


We see that all listed features are among the ones that we thoroughly discussed during the EDA performed in the summarizing section. It is interesting to notice that `tenure` is by far the most important feature. And, like detailed in the EDA, fiber optic internet is definitively a service the company should take a closer look at, being the second most important feature to our model.

Will oversampling be a better strategy?


### Oversampling

Now, we are going to use `SMOTE` from the Imbalanced Learn library. This sampler applies a Synthetic Minority Oversampling Technique (SMOTE), which select examples that are close in the feature space, drawing a line between the examples and drawing a new sample at a point along that line. Specifically, a random example from the minority class is first chosen. Then *k* of the nearest neighbors for that example are found. A randomly selected neighbor is chosen, and a synthetic example is created at a randomly selected point between the two examples in feature space.

The sampler will be added to the pipeline of steps to be applied after the preprocessing and before the model. Importantly, the change to the class distribution is only applied to the training dataset. The intent is to influence the fit of the models. The sampling is not applied to the test or holdout datasets used to evaluate the performance of a model.

The same models from before will be tested. For the initial screening, there will be no modification of default parameters values, besides for those that have the option to set as a binary classifier.

Full details with code can be seen on the [GitHub repository](https://github.com/chicolucio/customer-churn-prediction).

Results:


    LR
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.748    |   0.006   
    Precision       |   0.517    |   0.008   
    Recall          |   0.799    |   0.023   
    F1 score        |   0.627    |   0.010   
    F2 score        |   0.720    |   0.016   
    AUROC           |   0.764    |   0.009   
    AUPRC           |   0.466    |   0.009   
    
    LDA
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.745    |   0.007   
    Precision       |   0.513    |   0.008   
    Recall          |   0.795    |   0.024   
    F1 score        |   0.623    |   0.011   
    F2 score        |   0.716    |   0.017   
    AUROC           |   0.761    |   0.010   
    AUPRC           |   0.462    |   0.010   
    
    KNN
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.689    |   0.010   
    Precision       |   0.447    |   0.011   
    Recall          |   0.724    |   0.019   
    F1 score        |   0.553    |   0.012   
    F2 score        |   0.644    |   0.015   
    AUROC           |   0.700    |   0.011   
    AUPRC           |   0.397    |   0.010   
    
    CART
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.722    |   0.010   
    Precision       |   0.480    |   0.018   
    Recall          |   0.545    |   0.033   
    F1 score        |   0.510    |   0.023   
    F2 score        |   0.531    |   0.029   
    AUROC           |   0.666    |   0.016   
    AUPRC           |   0.383    |   0.016   
    
    NB
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.701    |   0.010   
    Precision       |   0.465    |   0.010   
    Recall          |   0.838    |   0.022   
    F1 score        |   0.598    |   0.012   
    F2 score        |   0.722    |   0.016   
    AUROC           |   0.745    |   0.011   
    AUPRC           |   0.433    |   0.011   
    
    SVC
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.764    |   0.009   
    Precision       |   0.541    |   0.012   
    Recall          |   0.729    |   0.029   
    F1 score        |   0.621    |   0.015   
    F2 score        |   0.682    |   0.021   
    AUROC           |   0.753    |   0.012   
    AUPRC           |   0.466    |   0.013   
    
    RF
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.779    |   0.010   
    Precision       |   0.590    |   0.020   
    Recall          |   0.556    |   0.029   
    F1 score        |   0.572    |   0.023   
    F2 score        |   0.562    |   0.026   
    AUROC           |   0.708    |   0.015   
    AUPRC           |   0.446    |   0.019   
    
    SGD
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.720    |   0.034   
    Precision       |   0.489    |   0.042   
    Recall          |   0.792    |   0.093   
    F1 score        |   0.600    |   0.032   
    F2 score        |   0.700    |   0.057   
    AUROC           |   0.743    |   0.025   
    AUPRC           |   0.440    |   0.027   
    
    LGBM
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.785    |   0.008   
    Precision       |   0.595    |   0.013   
    Recall          |   0.598    |   0.033   
    F1 score        |   0.596    |   0.021   
    F2 score        |   0.597    |   0.028   
    AUROC           |   0.725    |   0.015   
    AUPRC           |   0.462    |   0.017   
    
    XGB
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.779    |   0.009   
    Precision       |   0.586    |   0.017   
    Recall          |   0.571    |   0.030   
    F1 score        |   0.578    |   0.022   
    F2 score        |   0.574    |   0.027   
    AUROC           |   0.713    |   0.015   
    AUPRC           |   0.449    |   0.018   
    



    
![png](../project_churn_prediction_files/project_churn_prediction_127_0.png)
    


As can be seen, compared to no sampling, recall values increased in all models, with decrease in precision values since there is a trade-off between these metrics. Our models become better, classifying the minority class with less false negatives but more false positives.

We have chosen the metric AUPRC. We are going to select 4 models with higher AUPRC to tune the hyperparameters: LR, LDA, SVC, LGBM.

#### Tuning hyperparameters - oversampling

A grid search will be performed to find the best combination of hyperparameters for each model. The same hyperparameters evaluate for RUS will be evaluated here.

Full details with code can be seen on the [GitHub repository](https://github.com/chicolucio/customer-churn-prediction).

Results:


    LR_l1
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.751    |   0.008   
    Precision       |   0.520    |   0.010   
    Recall          |   0.798    |   0.022   
    F1 score        |   0.629    |   0.011   
    F2 score        |   0.720    |   0.016   
    AUROC           |   0.766    |   0.010   
    AUPRC           |   0.468    |   0.011   
    
    LR_l2
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.748    |   0.006   
    Precision       |   0.517    |   0.008   
    Recall          |   0.799    |   0.023   
    F1 score        |   0.627    |   0.010   
    F2 score        |   0.720    |   0.016   
    AUROC           |   0.764    |   0.009   
    AUPRC           |   0.466    |   0.009   
    
    LDA
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.747    |   0.009   
    Precision       |   0.515    |   0.011   
    Recall          |   0.787    |   0.023   
    F1 score        |   0.622    |   0.013   
    F2 score        |   0.712    |   0.018   
    AUROC           |   0.760    |   0.012   
    AUPRC           |   0.462    |   0.013   
    
    SVC
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.725    |   0.017   
    Precision       |   0.490    |   0.018   
    Recall          |   0.816    |   0.023   
    F1 score        |   0.612    |   0.016   
    F2 score        |   0.720    |   0.016   
    AUROC           |   0.754    |   0.013   
    AUPRC           |   0.449    |   0.016   
    
    LGBM
        Metric      |    Mean    |  Std Dev  
    ----------------------------------------
    Accuracy        |   0.788    |   0.007   
    Precision       |   0.587    |   0.012   
    Recall          |   0.681    |   0.034   
    F1 score        |   0.630    |   0.017   
    F2 score        |   0.660    |   0.026   
    AUROC           |   0.754    |   0.014   
    AUPRC           |   0.485    |   0.014   
    



    
![png](../project_churn_prediction_files/project_churn_prediction_137_0.png)
    


LGBM performed better considering the AUPRC metric. However, this result was due to a great precision score and a fair recall score. If more weight in recall is needed, logistic regression, the close second by the metric, seems a better choice, with the bonus of being a faster and easier to comprehend algorithm. As discussed earlier, we do not have costs data to support a choice between more weight to precision or recall. That's why we chose AUPRC as the metric, to have some balance between both.

The oversampling strategy did not improve the metrics compared with undersampling. Since it takes longer, and the SMOTE algorithm is more complex to explain to stakeholders, it is reasonable to choose undersampling, with the LGBM or the LR model, for production.

So, undersampling is better. Just for the sake of completeness, and since the LR algorithm is simpler and faster, let's take a look at the most important features according to this model combined with oversampling. We will choose the model with the `saga` solver, since both LR models shown before performed almost equally by the metric. This solver is a variation of gradient descent and incremental aggregated gradient approaches that uses a random sample of previous gradient values. Since it is fast for big datasets, and oversampling results in larger datasets compared with undersampling, it seems a reasonable choice.



The way to get information about feature importance in logistic regression is different from the one we saw for LGBM in the undersampling section. In order to understand, we need to remember some math.

Representing as $$p_+(x)$$ the model's estimate of the probability of class membership of a data item represented by feature vector **x**, we have the following equation that specifies that the log-odds of the class is equal to a linear function $$f(x)$$:

$$
\log \left( \frac{p_+(x)}{1 - p_+(x)} \right) = f(x) = w_0 + w_1 x_1 + w_2 x_2 + \cdots
$$

where $$w_i$$ are the coefficients, or weights, of each feature. Solving the equation for $$p_+(x)$$, it yields the logistic function [[13](https://amzn.to/3OgzzTJ)]:

$$
p_+(x) = \frac{1}{1+\exp(-f(x))}
$$

This is a classification problem with classes 0 (no churn) and 1 (churn). The logistic regression model has a `coef_` property that contains the coefficients found for each feature. These coefficients are both positive and negative. The positive scores indicate a feature that predicts class 1, whereas the negative scores indicate a feature that predicts class 0. We can order these values ascending and see the first 5 features (negative ones, predict 0), and the last 5 (positive, predict 1):




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Feature</th>
      <th>Importance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>std_scaler__tenure</td>
      <td>-1.545057</td>
    </tr>
    <tr>
      <th>5</th>
      <td>ohe__Contract_Two year</td>
      <td>-0.889923</td>
    </tr>
    <tr>
      <th>1</th>
      <td>std_scaler__MonthlyCharges</td>
      <td>-0.444953</td>
    </tr>
    <tr>
      <th>11</th>
      <td>ohe__InternetService_DSL</td>
      <td>-0.432502</td>
    </tr>
    <tr>
      <th>14</th>
      <td>ohe__MultipleLines_No</td>
      <td>-0.252727</td>
    </tr>
  </tbody>
</table>
</div>






<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Feature</th>
      <th>Importance</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>41</th>
      <td>ohe__TechSupport_No</td>
      <td>0.295541</td>
    </tr>
    <tr>
      <th>29</th>
      <td>ohe__PaymentMethod_Electronic check</td>
      <td>0.379205</td>
    </tr>
    <tr>
      <th>12</th>
      <td>ohe__InternetService_Fiber optic</td>
      <td>0.544658</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ohe__Contract_Month-to-month</td>
      <td>0.782041</td>
    </tr>
    <tr>
      <th>2</th>
      <td>std_scaler__TotalCharges</td>
      <td>0.896876</td>
    </tr>
  </tbody>
</table>
</div>



The features listed are essentially the same that the LGBM model listed as the top 10 most important features in the undersampling section. We can see that `tenure` has the larger absolute value, being the feature with most weight.

We have seen that, mathematically, there is a relationship between these coefficients and odds. Actually, we can "convert" these coefficients to odds in order to make more sense of them by simply exponentiating the values [[14](https://towardsdatascience.com/interpreting-coefficients-in-linear-and-logistic-regression-6ddf1295f6f1)]:




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="0" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Feature</th>
      <th>odds</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2</th>
      <td>std_scaler__TotalCharges</td>
      <td>2.451932</td>
    </tr>
    <tr>
      <th>3</th>
      <td>ohe__Contract_Month-to-month</td>
      <td>2.185929</td>
    </tr>
    <tr>
      <th>12</th>
      <td>ohe__InternetService_Fiber optic</td>
      <td>1.724018</td>
    </tr>
    <tr>
      <th>29</th>
      <td>ohe__PaymentMethod_Electronic check</td>
      <td>1.461123</td>
    </tr>
    <tr>
      <th>41</th>
      <td>ohe__TechSupport_No</td>
      <td>1.343853</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>14</th>
      <td>ohe__MultipleLines_No</td>
      <td>0.776680</td>
    </tr>
    <tr>
      <th>11</th>
      <td>ohe__InternetService_DSL</td>
      <td>0.648884</td>
    </tr>
    <tr>
      <th>1</th>
      <td>std_scaler__MonthlyCharges</td>
      <td>0.640854</td>
    </tr>
    <tr>
      <th>5</th>
      <td>ohe__Contract_Two year</td>
      <td>0.410687</td>
    </tr>
    <tr>
      <th>0</th>
      <td>std_scaler__tenure</td>
      <td>0.213300</td>
    </tr>
  </tbody>
</table>
</div>



Verbally, we can say that, for every one-unit increase in `TotalCharges`, the odds that the observation is in class 1 (churn) are 2.45 times as large as the odds that the observation is not in class 1 when all other variables are held constant. The same meaning stands for every value greater than 1. The top 5 are: `TotalCharges`, `Contract_Month-to-month`, `InternetService_Fiber optic`, `PaymentMethod_Electronic check` and `TechSupport_No`. It is interesting to see `TotalCharges` here, we have seen in the visualization section of this study that there are outliers in the churn class for this feature. Since the Scikit-Learn standard scaler, used in the preprocessing, is sensitive to outliers [[15](https://scikit-learn.org/stable/auto_examples/preprocessing/plot_all_scaling.html#)], it might have some influence in this result. All the other features were discussed before as features that indicate churn.

For odds less than 1, we can take *1/odds* to make even better sense of them. So, for every one-unit increase in `tenure`, the odds that the observation is NOT in class 1 (churn) are 1/0.21 or 4.76 times as likely as the odds that it is in target class 1. The top 5, considering the inverse value, are: `tenure`, `Contract_Two year`, `MonthlyCharges`, `InternetService_DSL` and `MultipleLines_No`. All of these features were discussed in the EDA as features that do not indicate churn.

We can visualize the odds through a colorized bar chart as follows:


    
![png](../project_churn_prediction_files/project_churn_prediction_147_0.png)
    


## Conclusions and action plan

![conclusion_image](https://github.com/Ciencia-Programada/articles-images/blob/master/churn/5.jpg?raw=true)

Customer churn is a critical issue that needs to be analyzed and predicted. Therefore, a model that can predict customer churn and deal with a huge amount of data is significant. In this study:

- an extensive EDA was performed
    - detailed insights were gained from the data regarding important features
- pipelines were used with the following pattern: preprocessing (scaling, encoding); resampling; model
    - the pipelines were then input to cross validation with repeated stratified K-fold (5 fold, 3 repeats)
- resampling strategies were analyzed and compared
    - random undersampling and SMOTE had similar results
    - random undersampling is easier to explain to stakeholders and outputs a smaller dataset, resulting in faster train times when models are applied
        - it might be preferable in a production scenario
- AUPRC was the main metric to compare models due to its literature background in imbalanced datasets
    - the top 4 classifiers were chosen to hyperparameter tuning
- LGBM classifier had the best scores when applied after both resampling strategies
    - LR had close scores to LGBM
        - since it is an easier to explain and faster to train model, it might be preferable in a production scenario
        - if it is desired higher recall values, LR is the best model
- both LGBM and LR important features were among the ones detailed in the EDA

**The results suggest that the Telco company should:**

- give more attention to technical support
- improve the fiber optic service
- invest in marketing strategies targeting
    - customers with short-term contracts, trying to move them to long contracts
    - customers without online services, offering these services
    - single customers, since their churn rate is higher than that of those who have partners and dependents
- guide customers towards simple paying methods like paper billing and credit card

The results shown here give some background and ideas for future works exploring:

- feature selection
- `class_weight` in models that support it or some similar parameter
- a scaler, apart from StandardScaler, less sensitive to outliers
- combination of random undersampling and oversampling
- tuning more hyperparameters for the selected models
- other models, like the ones used in the cited papers

Anyway, I hope to have provided interesting insights, and a valuable project. Should you have any comments, questions or suggestions, don't hesitate to contact me:

<center>
<a href="https://www.linkedin.com/in/flsbustamante" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 
<br>
<a href="https://franciscobustamante.com.br" target="_blank"><img src="https://img.shields.io/badge/portfolio-000000?style=for-the-badge&logo=About.me&logoColor=white" target="_blank"></a>
</center>
[<center><img alt="GitHub" width="10%" src="https://github.githubassets.com/images/modules/logos_page/GitHub-Logo.png
"></center>](https://github.com/chicolucio)

## References

1. N. Modani, K. Dey, R. Gupta, and S. Godbole, “CDR Analysis Based Telco Churn Prediction and Customer Behavior Insights: A Case Study,” in Web Information Systems Engineering – WISE 2013, vol. 8181, X. Lin, Y. Manolopoulos, D. Srivastava, and G. Huang, Eds. Berlin, Heidelberg: Springer Berlin Heidelberg, 2013, pp. 256–269. doi: 10.1007/978-3-642-41154-0_19.
2. S. Agrawal, A. Das, A. Gaikwad, and S. Dhage, “Customer Churn Prediction Modelling Based on Behavioural Patterns Analysis using Deep Learning,” in 2018 International Conference on Smart Computing and Electronic Enterprise (ICSCEE), Shah Alam, Jul. 2018, pp. 1–6. doi: 10.1109/ICSCEE.2018.8538420.
3. I. Ullah, B. Raza, A. K. Malik, M. Imran, S. U. Islam, and S. W. Kim, “A Churn Prediction Model Using Random Forest: Analysis of Machine Learning Techniques for Churn Prediction and Factor Identification in Telecom Sector,” IEEE Access, vol. 7, pp. 60134–60149, 2019, doi: 10.1109/ACCESS.2019.2914999.
4. “Churn.” https://www.productplan.com/glossary/churn/ (accessed Jun. 22, 2022).
5. IBM. "Telco customer churn". https://www.kaggle.com/datasets/blastchar/telco-customer-churn (accessed Jun. 1, 2022)
6. Shearer C., The CRISP-DM model: the new blueprint for data mining, J Data Warehousing (2000); 5:13—22
7. F. L. S. Bustamante, Customer churn prediction, 2022. https://github.com/chicolucio/customer-churn-prediction
8. J. Burez and D. Van den Poel, “Handling class imbalance in customer churn prediction,” Expert Systems with Applications, vol. 36, no. 3, pp. 4626–4636, Apr. 2009, doi: 10.1016/j.eswa.2008.05.027.
9. Wikipedia. "Accuracy paradox". https://en.wikipedia.org/wiki/Accuracy_paradox
10. T. Saito and M. Rehmsmeier, “The Precision-Recall Plot Is More Informative than the ROC Plot When Evaluating Binary Classifiers on Imbalanced Datasets,” PLoS ONE, vol. 10, no. 3, p. e0118432, Mar. 2015, doi: 10.1371/journal.pone.0118432.
11. Scikit-Learn. "Feature selection". https://scikit-learn.org/stable/modules/feature_selection.html
12. Scikit-Learn. "Model evaluation". https://scikit-learn.org/stable/modules/model_evaluation.html#precision-recall-f-measure-metrics
13. Provost, Foster, and Tom Fawcett. Data Science for Business: what You Need to Know About Data Mining and Data-analytic Thinking. Sebastopol, Calif.: O'Reilly, 2013
14. J. Benton, “Interpreting Coefficients in Linear and Logistic Regression,” Medium, Jul. 22, 2020. https://towardsdatascience.com/interpreting-coefficients-in-linear-and-logistic-regression-6ddf1295f6f1 (accessed Jun. 21, 2022).
15. Scikit-Learn. "Compare the effect of different scalers on data with outliers". https://scikit-learn.org/stable/auto_examples/preprocessing/plot_all_scaling.html#
