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
Models: The prediction from a logistic regression model can be interpreted as the probability that the label is 1. In contrast K-means is an unsupervised clustering model that returns cluster membership while linear regression returns real numbers as predictions. Neither returns results that can directly be interpreted as probabilities.

Since we have used the same data for training and testing, it is likely that we've overfit our model to the test data, so evaluating the model on the test data no longer provides a fair estimate of expected performance. It is likely that our model will perform worse than our estimate when used on unseen data.

## Sample classification pipeline
Obtain raw data --> Feature extraction --> Supervised learning --> Evaluation --> Predict
* Raw data consists of a set of labeled training observations. Observactions are documents.
* Feature extraction typically transforms each observations into a vector of real numbers (features).
In order to "featurize" the observations:
1. Create vocabulary: words that appear in the observations.


##Linear Algebra Review
* Matrices. Anxm = n rows and m columns >> A is an R to the n times m
* Vector: matrix with many rows and one column. Vector a is an R to the m. Vectors are assumed to be in column form (mx1).
* Transpose: swap rows and columns of a matrix. Aij = ATji
* Additions and substractions: the matrices must have the same dimensions.
* Scalar product (= dot product = inner product): a function that maps two vectors to a scalar.
* Matrix-Matrix multiplication is Associative (A(BC)=(AB)C) but not commutative (AB <> BA)
* Inner product: it returns a single number, (1xn)(nx1)
* Outer product: (nx1)(1xm)=(nxm)
* Identity matrix: square, with 1s in the diagonal and 0s elsewhere.
* Inverse matrix: multiplying a matrix by its inverse matrix returns the identity matrix. Only a square matrix can have an inverse.
* Euclidean Norm for Vectors: taking the square root of the sum of the squares of the vector values.
* The scalar product of a vector with its transposed version, equals the square of the eculidean norm.

## Big O Notation for time and space complexity
Complexity = Big-O Notation: Describes how algorithms respond to changes in input size, both in terms of processing time and space requirements.
* O(1):  Constant time algorithms perform the same number of operations every time they’re called.
* O(n): linear time algorithms
* O(n2): Quadratic time algorithms perform a number of operations proportional to the square of the number of inputs. E.g. outer product of two n-dimensional vectors.
Similarly, quadratic space algorithms require storage proportional to the square of the size of the inputs.
* Matrix-Vector multiply (nxm)(mx1): it takes O(mn) time, O(n) space
* Matrix-Matrix multiply (nxm)(mxp): it takes O(nmp) time, O(np) storage

Complexity is based off of how the algorithm scales for very large n. Constants and lower-order terms are dropped. For example, a model that takes 100*n^2 time has O(n^2) complexity, as does a model that takes 4n + n^2 time. A counterexample for this question is: algorithm A has running time n^3 and algorithm B has running time 10n^2. Until n >= 10, A is faster than B.

## Map, filter and reduce
* Map transforms a series of elements by applying a function individually to each element in the series. It then returns the series of transformed elements. 
* Filter also applies a function individually to each element in a series; however, with filter, this function evaluates to True or False and only elements that evaluate to True are retained. 
* Finally, reduce operates on pairs of elements in a series. It applies a function that takes in two values and returns a single value. Using this function, reduce is able to, iteratively, "reduce" a series to a single value.¶