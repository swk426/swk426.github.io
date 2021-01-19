---
layout: post
title:      "How to interpret the Confusion Matrix?"
date:       2021-01-18 18:58:36 -0500
permalink:  how_to_interpret_the_confusion_matrix
---


![](https://www.evidentlycochrane.net/wp-content/uploads/2017/06/question-marks-on-squares.jpg)

Just like its name, interpreting a confusion matrix can be very puzzling. However, it is one of the strongest tools to analyze your binary and multiclass classification results in machine learning. Thus, it is a form of communication that data scientists must know. The purpose of this article is to help better understand the confusion matrix. Then let us dig into the confusion matrix and what information does it hold?

## What is the Confusion Matrix?

The Confusion Matrix is a table of comparisons on a given model’s predictions. How well your model performed on its prediction. To proceed, the following are important terms to know. 

* True Positive: Cases when the model predicted yes and the actual is yes (Correct Prediction)
* True Negative: Cases when the model predicted no and the actual is no (Correct Prediction)
* False Positive: Cases when the model predicted yes and the actual is no (Incorrect Prediction) Aka. “Type I Error”
* False Negative: Cases when the model predicted no and the actual is yes (Incorrect Prediction) Aka. “Type II Error”
* Precision: How much predicted yes was correct?
* Recall or Sensitivity: How much actual yes was correctly predicted?
* Specificity: How much actual no was correctly predicted?
* Accuracy Rate: How much total prediction was correct?
* Error Rate: How much total prediction was incorrect?

![](https://www.researchgate.net/profile/Bin_Xia17/publication/283330762/figure/fig3/AS:619254843985928@1524653268934/The-confusion-matrix-and-relevant-evaluation-index-True-Positive-TP-The-number-of.png)

### Sample

Let us go over the Confusion Matrix with a sample. Let us look at the table below.

The purpose of the model was to predict the Maglinant skin cancers.

| | Predicted  Malig | Predicted Benign|
|------|------|------|
| Actual Malig | 95 | 26 |
| Actual Bengin| 5 | 627 |

* True Positive: 95. The model correctly predicted 95 malignants.
* True Negative: 672. The model correctly predicted 627 benigns.
* False Positive: 5. The model incorrectly predicted 5 benigns as malignants.
* False Negative: 26. The model incorrectly predicted 26 malignants as benigns.
* Precision: 95/(95+5) = 95/100 = .95 or 95% . Out of 100 predicted malignants, 95 was correctly predicted.
* Recall: 95/(95 + 26) = 95/121 = .79 or 79%. Out of 121 actual malignants, 95 was correctly predicted.
* Specificity: 627/ (627+5) = 627/632 = .99 or 99%. Out ot 632 actual benigns, 627 was correctly predicted.
* Acurracy: (95+627)/(95+26+5+627) = 722/753 = .96 or 96%. Out of 753 predictions, 722 was correctly predicted.
* Error Rate: (26+5)/(95+26+5+627) = 31/753 = .04 or 4%. Out of 753 predictions, 31 was incorrectly predicted.

## What value do you need to focus on when you are building the model?

When you are making decisions in Data Science, everything depends on the nature of the business. It would be nice to build a model that predicts 100% accuracy all the time. However, you cannot always build a model that exhibits 100%. When the model does not exhibit 100% accuracy, we need to decide which feature to put our weight on. For example, in the spam email filtering model, you would like to build a model that exhibits high precision rather than recall. When a message is predicted as spam mail, it correctly predicts spam mails. There will be more costly to recover from the spam mail than having an important message identified as spam mail. More so, if you have an important message that ends up in the spam mail folder, you can easily move the conversation to non-spam since it is an important conversation that you are paying attention to; however, when you are building a model that is health-related like breast cancer prediction, you would like to build a model that exhibits high recall compared to precision. You would like to predict more breast cancer patients in the population to save them because life has more value than the cost of diagnosis. 

I suggest following article for more detail concept on Precision vs. Recall and which one you should focus on when building the model. [Beyond Accuracy: Precision and Recall](https://towardsdatascience.com/beyond-accuracy-precision-and-recall-3da06bea9f6c).

## Wrap up

In this blog, I have covered the interpretation of the confusion matrix and related terms. In next blog, I will be covering the actual practice of confusion matrix in machine model with code along in python. [Confusion Matrix Code Along](https://swk426.github.io/confusion_matrix_code_along)

![](https://images.unsplash.com/photo-1533601017-dc61895e03c0?ixid=MXwxMjA3fDB8MHxzZWFyY2h8Mnx8dG8lMjBiZSUyMGNvbnRpbnVlZHxlbnwwfHwwfA%3D%3D&ixlib=rb-1.2.1&w=1000&q=80)

