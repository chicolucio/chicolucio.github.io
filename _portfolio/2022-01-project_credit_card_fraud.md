---
name: Credit card fraud detection
title: Credit Card Fraud Detection
image: /portfolio/project_credit_card_fraud_files/fraud_logo.jpg
position: 46
period: January 2022
toc: true
toc_min_header: 2
toc_max_header: 3
mathjax: true
header:
  image: /portfolio/project_credit_card_fraud_files/fraud_header.jpg
description: |
  In this work, we will use a credit card transactions database to study 
  different algorithms, which can be used to classify a given transaction 
  as fraudulent or not. We will see how to deal with unbalanced databases
  in machine learning.
---

## Abstract

In recent decades, with the constant increase in financial transactions, credit card
fraud has become a significant problem. Great computational power is devoted to
detecting fraudulent operations, especially with the ever-increasing use of online
banking and e-commerce. Several Data Mining and Machine Learning strategies have been
employed in this context.

In this work, we will use a dataset of credit card transactions and study different
algorithms that can be used to classify a given transaction as fraudulent or not.

## Context

<figure style="width:50%">
    <img alt="hacked" src="../project_credit_card_fraud_files/hacked.jpg">
</figure>

### Brief Description of a Fraud Detection System

Many types of fraud can be carried out, ranging from the omission of income sources,
through money laundering, to card cloning. In an attempt to prevent these, the financial
industry makes use of the so-called *Rule-Based Systems*, also known as *Production
Systems*, which store and manipulate data to infer whether a given operation is
fraudulent or not. Currently, such systems use machine learning tools.

We can envisage a fraud detection system in a bank as shown in the following figure.
There is the institution's database on which algorithm training is performed, generating
models that represent the characteristics of the stored transactions.


<figure>
<a href="https://linktr.ee/flsbustamante"><img src="../project_credit_card_fraud_files/fraud.png" alt="fraud flux"></a>
<figcaption>Fluxogram of a Fraud Detection System. Adapted <a href="https://doi.org/10.22937/IJCSNS.2021.21.9.4">from literature.</a></figcaption>
</figure>

The model is then used to decide whether new transactions will be accepted as genuine or
rejected as fraudulent. An accepted transaction will be executed and added to the
database to improve the model. A rejected one will be set aside for manual evaluation.
If it is considered genuine, it will be executed and added to the database as such. If
fraud is confirmed, the rejection is confirmed and recorded in the database as an actual
fraud.

As we can see, a crucial part of the process is to train an effective model. A
well-developed model would not only identify fraud but also determine the probability of
fraudulent behavior.

### Dataset Used in This Study

The dataset used in this work can be downloaded [from this Kaggle
link](https://www.kaggle.com/mlg-ulb/creditcardfraud). It is a database containing
credit card transactions recorded by European card operators over two days in September
2013. There are 284,807 transactions, of which only 492 were recorded as frauds. Note
the high number of transactions given that they cover only two days (and considering it
was more than 10 years ago, imagine what the volume might be nowadays for the same time
interval) and how it is a highly imbalanced dataset, as only 0.172 % of the total
records are fraudulent.

In the next section, we will provide a more detailed description of the dataset and
begin to understand how to train an effective model given this imbalance.

A cross-industry standard process for data mining
([CRISP-DM](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining#cite_note-Shearer00-1))
will be used to guide the development of the project as shown in the figure below.

<figure>
<img src="../project_credit_card_fraud_files/pipeline_simple.png" alt="diagram crisp" style="width:100%"/>
<figcaption>Proposed methodology flow diagram</figcaption>
</figure>


In this article, only the results will be shown. If you wish to see the complete code,
[visit the project's
repository](https://github.com/chicolucio/credit-card-fraud).


## Exploratory Data Analysis (EDA)

<figure style="width:50%">
    <img alt="eda" src="../project_credit_card_fraud_files/eda.jpg">
</figure>

Let us examine the dimensions:

    Number of entries: 284807
    Number of variables: 31

As discussed above, we have 284807 entries and 31 variables. In the
[dataset description on Kaggle](https://www.kaggle.com/mlg-ulb/creditcardfraud), some
explanations about the variables are provided:

- Due to confidentiality issues, the identifications of most of the original variables
are not provided;
- The variables represented as *V1, V2,... V28* are the result of a transformation by
[Principal Component Analysis
(PCA)](https://en.wikipedia.org/wiki/Principal_component_analysis), a technique used to
condense the information contained in several original variables into a smaller set of
statistical variables (components) with minimal loss of information. Later, we will see
some consequences of this transformation in our analysis;
- The variables `Time` (time) and `Amount` (transaction value) did not undergo PCA.
    - Time refers to the interval, in seconds, between each transaction and the first
    one in the dataset.
- The variable `Class` is the target variable:
    - The value `0` signifies a genuine, legitimate transaction
    - The value `1` signifies fraud

Obviously, `Class` has many 0 values since this value means a genuine transaction.
It was seen that there are 1825 entries of transactions with a value of 0 (variable
`Amount`). It is not clear what this means, whether these are cases of reversals, for
example, or something similar. As this is a very small number of cases relative to the
total number of entries in the dataset, such entries were not removed.

We have already seen in the introduction that the dataset is imbalanced. Let us verify
this visually:

<figure style="width:60%">
<img src="../project_credit_card_fraud_files/eda_class_dist.png" alt="class_dist"/>
<figcaption>Class imbalance. Logarithmic scale</figcaption>
</figure>

We can check if there is a time dependency in the data by analyzing the distribution of
the `Time` variable for each class. If there is a time dependency, it means that the
time at which the transaction occurred can influence the likelihood of fraud.

<figure style="width:90%">
<img src="../project_credit_card_fraud_files/eda_time_hue.png" alt="time"/>
<figcaption>Time distribution for each class</figcaption>
</figure>

While legitimate transactions have an increasing trend during the day, keeping a plateau
until evening when they start to decrease, frauds have a smoother distribution, being
more evenly distributed throughout the day. Then, they are easier to spot on the plot in
the early morning hours, when the number of legitimate transactions is lower.

It is important to check for outliers in the dataset. We can do this by analyzing the
distribution of the features. Using boxplots for each feature, we can identify outliers
in the data. We can also check the distribution of the features for each class to see if
there are differences between them.

<figure style="width:90%">
<img src="../project_credit_card_fraud_files/eda_outlier.png" alt="outlier"/>
<figcaption>Outliers in the dataset</figcaption>
</figure>

Some observations:

- For legitimate transactions, the outliers are more spread out, with some variables
having numerous outliers. This may be considered expected, as legitimate transactions
are more numerous than frauds.
- For frauds, the outliers are more concentrated, with fewer variables having a large
number of outliers. This is also expected, as frauds are less numerous than legitimate
transactions.
- The `Amount` variable has a large number of outliers for both classes. This is
expected, as the transaction value can vary greatly. But, as we can see, the outliers
for legitimate transactions are more spread out, while the outliers for frauds are more
concentrated.

Since the outliers are more concentrated for frauds, they were not remove, as they may
be important for the model to learn the patterns of fraud. Only the outliers for
legitimate transactions were removed, as they may be noise in the data. This is not a
standard approach, but since we are dealing with a classification problem and a huge
class imbalance, we need to keep as much information as possible about the minority
class.

It was used a quantile-based method to remove the outliers. The method is based on
values that are below a certain quantile or above another quantile. The quantiles used
were 0.0025 and 0.9975, a very small percentage of the data. This way, we can remove
only the most extreme outliers.

Let's check the boxplots for the features after removing the outliers:

<figure style="width:90%">
<img src="../project_credit_card_fraud_files/eda_outlier_treatment.png" alt="outlier_treatment"/>
<figcaption>Distribution after removing outliers with quantile-based method</figcaption>
</figure>

We see that now the range of values for the features is more balanced between the two
classes.

The shape of the distribution of the features is also important. We can check the
distribution of the features for each class to see if there are differences between
them. This can help us understand the data better and choose the best algorithm to
classify the transactions. Let's check the distribution of the features for each class:

<figure style="width:90%">
<img src="../project_credit_card_fraud_files/eda_dist_hue.png" alt="dist_hue"/>
<figcaption>Distribution of the features for each class</figcaption>
</figure>

We see that some features, like *V14* and *V16* have a quite different distribution for
each class. This is a good sign that they might be useful for a future model. And
features like *V13* and *V15* have a similar distribution for both classes. This is a
sign that they might not be useful for a future model.

Even though the plots show some differences between the classes, some statistical tests
were performed to confirm if the differences are statistically significant.
Specifically, the Mann-Whitney U test was used to check if the distributions of the
features for each class are different. The test was performed for each feature, and the
p-values were calculated. This way, it was shown that the features *V13*, *V15*, and
*V22* have a p-value greater than 0.05, the significance level adopted,
which means that the distributions of these features for each class are not
statistically different.

With this information, these three features were removed from the dataset. The other
features were kept, as they showed a statistically significant difference between the
classes.

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
- Support Vector Machine for Classification (SVC - only after resampling - see below)

A Scikit-Learn pipeline was used to preprocess the data and train the models. The
features were preprocessed using power transformations (features `Time` and `Amount`)
and robust scaling (features `V1` to `V28`). Due to the imbalance in the dataset, a
stratified 5-fold cross-validation technique was used to train and evaluate the models.
Moreover, the class weight parameter was used in the models (if available) to deal with
the imbalance.

<figure style="width:90%">
<img src="../project_credit_card_fraud_files/pipeline_no_resampling.png" alt="pipeline no resampling"/>
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
<img src="../project_credit_card_fraud_files/models_performance.png" alt="models_performance"/>
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

Concerning the metrics, some
[literature](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0118432)
suggests that the area under the precision-recall curve (AUPRC, represented as
`average_precision` on Scikit-Learn) is a better metric for imbalanced datasets than the
area under the ROC curve (AUROC, or `roc_auc`). This is because the ROC curve is not
sensitive to changes in the false positive rate when the true positive rate is high. The
precision-recall curve, on the other hand, is sensitive to changes in the false positive
rate when the true positive rate is high. I already covered this in a
[previous article](https://franciscobustamante.com.br/portfolio/2022-auprc/).

We can see that the logistic regression model has the best performance in terms of the
chosen metric and is also one of the fastest models. However, we see that although the
recall is high, the precision is extremely low. This means that the model is classifying
most of the samples as positive, which is not good. We need to find a balance between
precision and recall.

Now, let's check the second-best model concerning average precision, the KNN model. It has
a good balance between precision and recall. However, it is one of the slowest models.
And here it is not an issue related to fine-tuning and hyperparameters. We have a
considerable dataset, and KNN essentially calculates the distance between each sample
and all the other samples. This is a very time-consuming process during the scoring
phase (you can compare fit and score times in the results dataframe).

To deal with the time issue, resampling techniques were explored.

### Resampling Techniques

<figure style="width:50%">
    <img alt="" src="../project_credit_card_fraud_files/performance.jpg">
</figure>

Resampling techniques are used to deal with imbalanced datasets. They can be divided
into two categories: oversampling and undersampling. Oversampling techniques increase the
number of samples in the minority class, while undersampling techniques decrease the
number of samples in the majority class. In this work, the Imbalanced-Learn library was
used to apply the resampling technique.

An undersampling technique called near miss was implemented. NearMiss is a technique that
selects points of the majority class that are going to be removed. This way, we will have
a balanced dataset, and we will be able to train the models faster.

You can learn more about the near miss technique on the [imbalanced-learn
documentation](https://imbalanced-learn.org/stable/references/generated/imblearn.under_sampling.NearMiss.html).

As can be seen in the docs, there are 3 versions of NearMiss: NearMiss-1, NearMiss-2,
and NearMiss-3.  NearMiss-3 was chosen. It is a 2-steps algorithm. First, for each
negative sample, their nearest-neighbors will be kept. Then, the positive samples
selected are the one for which the average distance to the nearest-neighbors is the
largest.

The value `1/2` was set for the parameter `sampling_strategy`. This value
means that we want a final dataset with a proportion of 1:2 between the minority and
majority classes. This is still an imbalanced dataset, but it is far less imbalanced
than the original dataset so that the models can learn better.

The same pipeline used before, with the addition of the nearmiss technique, was used to
train the models. The same metrics were used to evaluate the models. 

<figure style="width:90%">
<img src="../project_credit_card_fraud_files/pipeline_full.png" alt="pipeline_resampling"/>
<figcaption>Scikit-Learn pipeline used to train the models after resampling</figcaption>
</figure>

The consolidated metrics for each model are shown below as boxplots:

<figure style="width:90%">
<img src="../project_credit_card_fraud_files/models_resampling_performance.png" alt="models_resampling_performance"/>
<figcaption>Consolidated metrics for each model after resampling</figcaption>
</figure>

It is interesting that the performance of KNN degraded a lot. Since we removed many
points from the majority class, and the removal considered the nearest neighbors, the
KNN algorithm lost a lot of information.

However, our dataset now is small enough for algorithms like SVC to perform well. Even
though it is not the best model concerning our metric of choice (it is the second best),
it is the only model to have a good balance between precision and recall. This is
important because we want to predict the minority class, and we would rather not have
plenty of false positives.

It is the slowest model of all, but since we have a small dataset after resampling,
it is not a problem now as it was before. Comparing, the SVC training after resampling
is almost 10 times faster than the KNN training before resampling.

The SVC model was fine-tuned using a grid search. The parameters `C`, `gamma` and
`kernel` were tuned. The best parameters found were `kernel=rbf`,  `C=1` and
`gamma=scale`.

The final model has the following metrics:

| Metric            | Score    | Std Dev |
| ----------------- | -------- | ------- |
| Accuracy          | 0.99968  | 0.00004 |
| Balanced Accuracy | 0.91     | 0.01    |
| F1                | 0.90     | 0.01    |
| Precision         | 1.00     | 0.00    |
| Recall            | 0.82     | 0.02    |
| ROC AUC           | 0.941    | 0.008   |
| Average Precision | 0.86     | 0.02    |
| Neg. Brier Score  | -0.009   | 0.002   |
| F1 Weighted       | 0.999665 | 0.00005 |

## Model Interpretability

Model interpretability is an essential factor when choosing a model. It is important to
understand how the model makes its predictions.

Some models are more interpretable than others. For example, decision trees are very
interpretable, as we can see the rules that the model uses to make its predictions. On
the other hand, models like SVM are not very interpretable, as we cannot see the rules
that the model uses to make its predictions.

In this work, the permutation importance technique was used to interpret the models. The
permutation importance technique is a model-agnostic technique that can be used to
interpret any model. It works by permuting the values of a feature and checking how much
the model's performance decreases. The more the performance decreases, the more important
the feature is for the model.

The figure below shows the permutation importance for the chosen model, SVC:

<figure style="width:90%">
<img src="../project_credit_card_fraud_files/models_resampling_permutation_importance.png" alt="perm_imp"/>
<figcaption>Permutation importance for the chosen model</figcaption>
</figure>

We see that features *V17* and *V14* decrease the average precision the most when they
are permuted. This means that these features are the most important for the SVC model.
This is consistent with the results of the graphical and statistical feature importance
analysis we did previously.

The permutation importance may seem a bit abstract for some people. So, other tools
can be used to show to stakeholders how the model is making its predictions. 
Plots are usually easier for stakeholders to understand. Let's try one.

The Kolmogorov-Smirnov (KS) plot is a way to check how well a binary classification
model separates two groups: positives (class = 1) and negatives (class = 0). It helps us
understand if the model is making clear distinctions between the two classes based on
the probabilities it assigns.

The KS plot is based on something called the Cumulative Distribution Function (CDF),
which is just a way of showing how the probabilities are distributed in each class.
Imagine listing all predicted probabilities for a class in order from smallest to
largest. The CDF tells us, for each probability value, how many cases fall at or below
that value.

Consider the plot below:

- The blue line represents the positive class (y=1). It shows how many actual positive
cases have a predicted probability less than or equal to a given value.
- The red line represents the negative class (y=0). It shows how many actual negative
cases have a predicted probability less than or equal to that value.
- The KS statistic is the biggest vertical gap between the two lines. This gap tells us
how well the model is distinguishing between positives and negatives.

The KS statistic is a number between 0 and 1 that indicates how well the model separates
the two classes:

- A high KS value (closer to 1) means the model is doing a good job distinguishing
between positives and negatives.
- A low KS value (close to 0) means the model struggles to tell the two classes apart.

If the KS value is greater than 0.4, it usually means the model is making strong
distinctions between the two classes.

This plot is a great way to visually confirm how well a model is performing beyond
standard metrics like accuracy and AUC-ROC.

<figure style="width:70%">
<img src="../project_credit_card_fraud_files/models_resampling_ks_plot.png" alt="ks_plot"/>
<figcaption>Kolmogorov-Smirnov plot for the chosen model</figcaption>
</figure>

Confusion matrices are also a great way to interpret the model. They show how many
samples were classified correctly and incorrectly. The confusion matrix for the chosen
model, SVC, is shown below:

<figure style="width:60%">
<img src="../project_credit_card_fraud_files/models_resampling_confusion_matrix.png" alt="conf_matrix"/>
<figcaption>Confusion matrix for the chosen model</figcaption>
</figure>

We see that, for class 0, the model classified all samples correctly. For class 1, the
model classified 83% of the samples correctly. This is a good result, considering that
the dataset is highly imbalanced.

## Key Takeaways and Business Impact

<figure style="width:50%">
    <img alt="" src="../project_credit_card_fraud_files/conclusion.jpg">
</figure>

This project demonstrates that data science techniques can significantly improve fraud
detection by addressing challenges associated with imbalanced datasets. Key insights
include:

- **Metric Selection is Crucial:** Traditional metrics such as accuracy can be
misleading in highly imbalanced scenarios. Our analysis shows that metrics like average
precision (AUPRC), recall, and balanced accuracy provide a more realistic picture of
model performance.
- **Resampling Techniques Enhance Model Performance:** Applying undersampling (using the
Nearmiss-3 method) allowed us to balance the data and train models more effectively. The
resulting improvements in precision and recall are critical for detecting fraudulent
transactions.
- **Support Vector Machines (SVC) Show Promise:** After resampling and fine-tuning, SVC
emerged as the best-performing model, balancing a high recall with an acceptable
precision level. This balance is essential for minimizing both false negatives and false
positives.
- **Interpretability Aids Decision-Making:** Using model-agnostic interpretability
techniques, such as permutation importance and the Kolmogorov-Smirnov plot, provides
transparency on which features most influence the model’s predictions. This insight is
invaluable when communicating results to business stakeholders and aligning the model
with operational strategies.

The business impact of this work is substantial. A robust fraud detection system that
leverages these techniques can reduce financial losses, improve customer trust by
minimizing false alarms, and provide a scalable solution that evolves with emerging
fraud tactics. Integrating such a model into existing systems can support real-time
decision making, allowing financial institutions to act swiftly and allocate resources
more efficiently.

## Actionable Recommendations for the Fraud Detection Team

Based on our analysis, the following steps are recommended to enhance fraud detection
operations:

- **Adopt a Metrics-Driven Approach:**  
  - Monitor not just accuracy but also AUPRC, recall, balanced accuracy, and the KS
  statistic to ensure the model effectively differentiates between genuine and
  fraudulent transactions.

- **Implement Resampling Strategies:**  
  - Use techniques such as NearMiss-3 to balance datasets, which can improve model
  learning and speed up training, especially when dealing with large volumes of
  transactions.

- **Integrate the SVC Model into the Fraud Detection Workflow:**  
  - Deploy the fine-tuned SVC model to flag potential fraud cases in real time, ensuring
  a balance between detecting fraud (high recall) and minimising false alerts
  (maintaining precision).

- **Enhance Model Interpretability:**  
  - Utilize tools such as permutation importance and KS plots to continuously monitor
  and explain model decisions. This will help in fine-tuning the system and ensuring
  transparency with stakeholders.
  
- **Establish a Continuous Improvement Cycle:**  
  - Set up a process for regular model retraining and validation using new transaction
  data to adapt to emerging fraud patterns.
  - Incorporate feedback from manual evaluations of flagged transactions to refine the
  model further.

- **Collaborate with Cross-Functional Teams:**  
  - Work closely with IT, risk management, and customer service teams to align the
  model’s thresholds and alert mechanisms with operational needs and customer
  expectations.

- **Assign cost values to false positives and false negatives:**  
  - By assigning costs to false positives and false negatives, the model can be
  fine-tuned to minimize financial losses and operational costs.

- **Check the original features:**  
  - The features `V1` to `V28` are the result of a PCA transformation. It is important
  to check the original features to understand the model's predictions better.
  Which are the original features with higher contribution to the components `V1` to
  `V28`? Specifically for the features `V14` and `V17`, which are the most important
  features for the model? Surely, this will give more insights into the model's
  predictions.

Implementing these recommendations will not only improve the detection of fraudulent
transactions but also help in managing operational costs, improving customer
satisfaction, and ultimately reducing financial losses associated with fraud.



## Final Thoughts

This project underscores the transformative potential of data science in tackling
real-world challenges such as fraud detection. By addressing issues like data imbalance
through resampling techniques and carefully selecting performance metrics, we developed
a robust model that balances the trade-off between recall and precision. The integration
of interpretability tools further enhances stakeholder trust and aids in continuous
model improvement. Overall, this work demonstrates how data-driven strategies can
significantly enhance operational efficiency and reduce financial risk, while also
paving the way for ongoing advancements in fraud prevention.

You can check the complete code for this project on the [GitHub
repository](https://github.com/chicolucio/credit-card-fraud).

Reach out to me if you have any questions or would like to discuss how these techniques
can be applied to your specific business challenges. Here are some ways to connect with
me and learn more about my work:

<div class="social-container">

<a href="https://www.linkedin.com/in/flsbustamante" target="_blank"><img src="https://img.shields.io/badge/-LinkedIn-%230077B5?style=for-the-badge&logo=linkedin&logoColor=white" target="_blank"></a> 

<a href="https://github.com/chicolucio" target="_blank"><img src="https://img.shields.io/badge/GitHub-000000?style=for-the-badge&logo=GitHub&logoColor=white" target="_blank"></a>

<a href="https://franciscobustamante.com.br" target="_blank"><img src="https://img.shields.io/badge/portfolio-333333?style=for-the-badge" target="_blank"></a>

</div>
