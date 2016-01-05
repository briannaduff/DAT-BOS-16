---
title: Logistic Regression
duration: 3:00
creator:
    name: Ed Podojil
    city: NYC
    dataset: college admissions
---

# ![](https://ga-dash.s3.amazonaws.com/production/assets/logo-9f88ae6c9c3871690e33280fcf557f33.png) Introduction to Logistic Regression
Week # | Lesson 9

### LEARNING OBJECTIVES
*After this lesson, you will be able to:*
- Build a Logistic regression classification model using the sci-kit learn library
- Describe the sigmoid function, odds, and odds ratios as well as how they relate to logistic regression
- Evaluate a model using metrics, such as: classification accuracy/error, confusion matrix, ROC / AOC curves, and loss functions

### STUDENT PRE-WORK
*Before this lesson, you should already be able to:*
- Implement a linear model (LinearRegression) with sklearn
- Understand what is a coefficient
- Recall metrics accuracy and misclassification
- Recall the differences between L1 and L2 regularization

### INSTRUCTOR PREP
*Before this lesson, instructors will have to:*
- review materials
- be familiar with the datasets

### LESSON GUIDE
| TIMING  | TYPE  | TOPIC  |
|:-:|---|---|
| 5 min  | [Opening](#opening) | Discuss lesson objectives, Reviewing Probability |
| 10-15 mins | [Introduction](#introduction) | Intro to Logistic Regression |
| 10-15 mins | [Demo](#demo)  |  |
| 20-25 mins | [Guided Practice](#guided-practice)  | |
| 10-15 mins | [Introduction](#introduction) | |
| 10-15 mins | [Demo](#demo)  |  |
| 20-25 mins | [Guided Practice](#guided-practice) | |
| 10-15 mins | [Introduction](#introduction-eval) | Intro to additional classification metrics and the confusion matrix |
| 10-15 mins | [Guided Practice](#guided-practice-eval) | Determining proper metrics given classification problems |
| 30-35 mins | [Independent Practice](#ind-practice-eval)  | Optimizing a logistic regression using new metrics  |
| 5-10 mins | [Conclusion](#conclusion) | Wrapup |

## Opening (5 minutes)

Read through the next two questions and brainstorm some ideas on how to answer each.

1. In class we've covered two different algorithms thus far: the linear model (ordinary least squares, OLS) and k-nearest neighbors (knn). The main difference between these two would be that OLS is used to solve a continuous regression problem, while KNN is used to solve a categorical problem. What other differences lie in how they _approach_ solving the problem? (For example, what is _interpretable_ about OLS, compared to what's _interpretable_ in KNN)

2. What advantages could we have, compared to KNN, using a linear model like OLS to solve classification? What would be the challenges to using OLS to solve classification (say, if the values were either 1 or 0)?

## Introduction to Logistic Regression

Logistic Regression is a _linear_ approach to solving a classification problem. That is, we can use a linear model, simile to Linear Regression, in order so solve if a item belongs or does not belong to a class label.

### Challenge! Linear Regression Results for Classification

Regression results, as defined, can have a value ranged from negative infinity to infinity (even though not all regression problems use that entire range; imagine predicting a football player's salary... it wouldn't be negative, and it has a cap to how high it could go).

We _couldn't_ use this for classification: the math could check out in a binary problem (think: belonging to something, 1, could be greater than not belonging to it, 0), what happens when it predicts a value of 2? 20? 300 Million? -1? We need an approach to normalize the results to _something_ more reasonable.

### Fix 1: Probability

One approach we could take is predicting the probability that an observation belongs to a certain class. We could assume that the _prior_ probability (the _bias_) of a class is the class distribution. For example, if we know that out of 2200 people on the Titanic that roughly 700 survived, then without knowing anything about the passengers and crew, the probability of survival would be ~0.32 (32%). However, we still need a way to use a linear function to either increase or decrease the probability of an individual, given the data we know about them.

**Recall**: This prior probability is most similar to what value in the ordinary least squares formula? (alpha, or the y-intercept)

### Fix 2: Link Functions and the Sigmoid Function

Another advantage to Ordinary Least Squares is that it allows for _generalized_ models using a _link_ function. Link functions allow us to build a relationship between a linear function and the mean of a distribution.

**RECALL**: What was the distribution most aligned with OLS/Linear Regression? (The Normal Distribution)

For classification, we need a distribution associated for categories: the probability of a given event, given all events. The link function that best allows for this is _logit_ function, which is the inverse of the _sigmoid_ function.

We'll start with sigmoid function. A _sigmoid function_, quite simply, is a function that visually looks like an s. While it serves many purposes, a sigmoid function is useful in logistic regression.

Our sigmoid function is defined mathematically as `1 / 1 + e^-t`: (recall, `e` is the inverse of the natural log), where as t increases/decreases, the result is closer to 1 or 0 (when t = 0, the result would be 0.5). Since it is `t` that decides how much to increase or decrease the value away from 0.5, `t` can help with interpretation to solve for something like a coefficient. But in its current form, it is not as useful.

### Demo: What does the Sigmoid Function Look Like on a chart?

Use the sigmoid function above (`1 / 1 + e^-t`) with values of `t` between -6 and 6 and chart in on a graph. Do this by hand or write some python code to evaluate it (`e = 2.71`). Confirm: do we get the s shape we expect?

### Fix 3: Odds, and Log-Odds

As mentioned above, the _logit_ function is the inverse of the _sigmoid_ function, and acts as our _link_ function. Mathematically it's represented as:

`ln(p / (1 - p))`

where the value within the natural log (`p / (1 - p)`) represents _odds_. Taking the natural log of odds generates _log odds_ (hence, logit). The beauty of the logit function is that it allows for values between negative infinity and infinity (**recall**: why is this important? What does this remind us of?), but provides us probabilities between 0 and 1.

For example, a logit value (log odds) of .2 (or odds of ~1.2/1):

`0.2 = ln(p / (1 -p))` ()

with a mean probability of 0.5, means the adjusted probability would be _about_ 0.55:

`1 / (1 + e^-.2)` (python: `1 / (1 + numpy.exp(-0.2)`)

While the logit value (log odds) represents the _coefficients_ in the logistic function, we can convert them into odds ratios that would be more easily interpretable.

## Guided Practice: Wager these odds!

Given the odds below for some football games, use the _logit_ function and the _sigmoid_ function to solve for the _probability_ that better team would win.

You'll first want to write two python functions:

```python
def logit_func(odds):
    # uses a float (odds) and returns back the log odds (logit)
    return None

def sigmoid_func(logit):
    # uses a float (logit) and returns back the probability
    return None

```

1. Stanford : Iowa, 5:1
2. Alabama : Michigan State, 20:1
3. Clemson : Oklahoma, 1.1:1
4. Houston : Florida State, 1.8:1
5. Ohio State : Notre Dame, 1.6:1

### Independent Practice: Logistic Regression Implementation

Use the data `collegeadmissions.csv` and the (LogisticRegression)[http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html] estimator in sklearn in order to predict the target variable `admit`. Your objectives are:

1. What is the bias, or prior probability, of the dataset?
2. Build a simple model with one feature and explore the `coef_` value: does this represent the odds, or logit (log odds)?
3. Build a more complicated model using multiple features. Interpreting the odds, which features have the most impact on admission rate? Which features have the least?
4. Report back the accuracy of your model.

<a name="intro-eval"></a>
## Introduction: Advanced Classification Metrics: Precision, Recall, AUC.

Accuracy is only one of several metrics used when solving for a classification problem. It is best defined as `total predicted correct / total data set`. But accuracy alone isn't always usable: for example, if we know a prediction is 75% accurate, accuracy does not provide _any_ insight into why the 25% was wrong: was it wrong equally across all class labels? Did it just guess one class label for all predictions and 25% of the data was just the other label? It's important to look at other metrics to fully understand the problem.

![confusion_matrix](https://github.com/podopie/DAT18NYC/raw/83dc789584a3349096988bbe14ffd7b87acef5e8/classes/img/confusion_matrix_metrics.png)

We can split up the accuracy of each label by using _true positive rate_ and _false positive rate_.

**True Positive Rate (TPR)**: Out of all of the target class label, how many were accurately predicted to belong to that class?

**False Positive Rate (TPR)**: The inverse of TPR: out of all not belonging to a class label, how many were predicted as the target class label?

A very good classifier would have a true positive rate approaching 1, and a false positive rate approaching 0. In a binary problem (say, predicting if someone smokes or not), It would accurately predict all of the smokers as smokers, and not predict any of the nonsmokers as smokers.

Logically, we still like single numbers for optimizing, so we can use a metric called Area Under the Curve (AUC), which summarizes the impact of TPR and FPR in once single value. This is also called the Receiver Operating Characteristic (ROC). ROC/AUC is a measure of area under a curve that is described by the TPR and FPR.

![auc](http://scikit-learn.org/stable/_images/plot_roc_001.png)

Using the logic of TPR and FPR above:

1. If we have a TPR of 1 (all positives are marked positive) and an FPR of 0 (all negatives are not marked positive), we'd have an AUC of 1. This means everything was accurately predicted.
2. If we have a TPR of 0 (all positives are not marked positive) and an FPR of 0 (all negatives are marked positive), we'd have an AUC of 0. This means nothing was predicted accurately.
3. An AUC of .5 would suggest, somewhat, randomness, and is an excellent benchmark to use for prediction (is my AUC above .5?)

Keep in mind that sklearn has all of these metrics on [one handy page](http://scikit-learn.org/stable/modules/classes.html#sklearn-metrics-metrics).

<a name="guided-practice-eval"></a>
## Guided Practice: How to decide which metric to use?

While AUC seems like a nice "golden standard" for evaluating binary classification, it could be _further_ improved, dependent on your classification problem. There will be instances where error in positives vs negative matches will be very important.

For each of the following examples:

* Write a confusion matrix (like above: true positive, false positive, true negative, false negative) and decide what each square represents for the example
* Define the _benefit_ of a true positive and true negative
* Define the _cost_ of a false positive and false negative
* Determine: at what point does the cost of a failure outweigh the benefit of a success? This would help you decide how to optimize TPR, FPR, and AUC.

1. A test is developed for determining if a patient has cancer or not
2. A newspaper company is targeting a marketing campaign for "at risk" users that may stop paying for the product soon.
3. You build a spam classifier for your email system.

<a name="ind-practice-eval"></a>
## Independent Practice: evaluating KNN with alternative metrics

[Kaggle's common online exercise](https://www.kaggle.com/c/titanic) is exploring survival data from the Titanic.

**Learning Goals**:

1. Spend a few minutes determining which data would be most important to use in the prediction problem. You may need to create new features based on the data available. Consider using a feature selection aide in sklearn. But a worst case scenario; identify one or two strong features that would be useful to include in the model.
2. Spend 1-2 minutes considering which _metric_ makes the most sense to optimize. Accuracy? FPR or TPR? AUC? Given the business problem (understanding survival rate aboard the Titanic), why should you use this metric?
3. Build a KNN model that solves for K. Be prepared to explain why you picked this K, metric, and feature set in predicting survival using the tools necessary (such as a fit chart).

Use the starter code included to get you going.