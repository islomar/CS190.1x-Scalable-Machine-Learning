# Week 1: course overview and Machine Learning basics

## Distributed computing and Apache Spark Text
* Apache Spark: general, open source cluster computing engine that is,
** well suited for ML functionality:
*** Fast iterative computations.
*** Efficient communication primitives.
**Simple and expressive:
*** APIs in Scala, Java, R and Python.
*** Interactive shell.
** Integrated higher-level libraries:
*** Spark SQL.
*** Spark streaming.
*** MLM: Spark in-house ML library.

* MLlib: Spark's ML library

* Selected Research Papers
Spark: Cluster Computing with Working Sets: http://people.csail.mit.edu/matei/papers/2010/hotcloud_spark.pdf
Resilient Distributed Datasets: A Fault-Tolerant Abstraction for In-Memory Cluster Computing: http://usenix.org/system/files/conference/nsdi12/nsdi12-final138.pdf
MLlib: Machine Learning in Apache Spark: http://arxiv.org/pdf/1505.06807.pdf


## What is Machine Learning?
Constructind and identifying methods that learn from and make predictions on data.
Observations (items for learning, e.g. emails), features (length, presence of keywords, date), labels (e.g. spam vs not-spam)
Training data is what we give to a learning algorithm, e.g. a set of emails along with their labels.
Test data, in contrast, is something that we would hold at training time and subsequently use to evaluate the learning model that we've devised.
Supervised vs Unsupervised learning.


## Typical Supervised Learning Pipeline Text

Success of entire pipeline often depends on choosing good descriptions of observations (features).

If we evaluate our model on the test set and then retrain the model to obtain a better result, it is possible that we will overfit our model to the data in the test set.  This means that our subsequent evaluation of the model could provide an overly optimistic view of the model's performance on future data.  Later in the course we will discuss how we can use another hold out set (called a validation set) to circumvent this issue.


## Sample classification pipeline