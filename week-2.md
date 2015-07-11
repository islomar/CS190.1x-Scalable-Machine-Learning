# Introduction to Apache Spark

## Interesting links
* http://aws.amazon.com/elasticmapreduce/
* http://people.csail.mit.edu/matei/papers/2010/hotcloud_spark.pdf
* https://databricks.com/blog/2014/11/05/spark-officially-sets-a-new-record-in-large-scale-sorting.html
* MLlib: Machine Learning in Apache Spark: http://arxiv.org/pdf/1505.06807.pdf

* Spark documentation: https://spark.apache.org/documentation.html
* Spark Programming guide: https://spark.apache.org/docs/latest/programming-guide.html


This tutorial will teach you how to use Apache Spark, a framework for large-scale data processing, within a notebook. Many traditional frameworks were designed to be run on a single computer. However, many datasets today are too large to be stored on a single computer, and even when a dataset can be stored on one computer (such as the datasets in this tutorial), the dataset can often be processed much more quickly using multiple computers. Spark has efficient implementations of a number of transformations and actions that can be composed together to perform data processing and analysis. Spark excels at distributing these operations across a cluster while abstracting away many of the underlying implementation details. Spark has been designed with a focus on scalability and efficiency. With Spark you can begin developing your solution on your laptop, using a small dataset, and then use that same code to process terabytes or even petabytes across a distributed cluster.

## The Big Data problem
A single machine can no longer process or even store all the data!"
Only solution is to distribute data over large clusters"

## Counting words
For really HUGE documents, the best solution is divide and conquer, to partition the documents and have several machines counting and suming up word results.
* The first step of counting all of the words is a Map: e.g. the document is partitioned and each partition is sent to a different machine, which counts all the words of that specific partition.
* Combining all those results is a Reduce: e.g. each machine sends all his values for "I" to a specific machine.
This is what Google wrote in 2004 in a research paper.
We can use Map-Reduce also for sorting: in the reduce phase, sending to the first machine all the words which occurs less than 3 times, to the second machine all the words which occurs less than 5 times, etc.

* What's hard about cluster computing?:
1. How to divide work across machines.
2. How to deal with failures: 1 server fails every 3 years. With 10k nodes, see 10 failures/day. Even worse and more common: stragglers (slow).

## Failures and slow tasks
Map Reduce deals with failures and slow tasks by re-launching the tasks on other machines. This functionality is enabled by the requirement that individual tasks in a Map Reduce job are idempotent and have no side effects. These two properties mean that given the same input, re-executing a task will always produce the same result and will not change other state. So, the results and end condition of the system are the same, whether a task is executed once or a thousand times.

## Map-Reduce and Disk I/O
* Every step that we perform (e.g. Map, Reduce) passes through hard drives.
* Disk I/O is very slow!!
* Using Map Reduce for complex jobs, interactive queries and online processing involves lots of disk I/O!

## Technology trends and an opportunity
The cost of memory has dropped A LOT: 1 cent/MB.
Opportunity:
* Keep more data in-memory instead of writing it out to slow disks and then having to read it.
* Spark: distributed execution engine, use memory instead of disk!!
** Use memory for in-data sharing.
* RDDs: Resilient Distributed Datasets.

* Spark framework:
** Core Apache Spark
** Spark SQL
** Spark Streaming
** MLib (machine learning)
** GraphX (graphical computing library)

Using memory instead of disks offers two huge benefits. The first benefit is that memory is much faster than disks. The time to read or write a value to memory is only a few nanoseconds, while the time to read or write is several milliseconds - that means memory is a million times faster than disks. The second benefit is that keeping intermediate results in memory means that they do not have to be converted into a format that can be stored on disks. The process of converting a memory object to a disk object is called serialization and the process of converting a disk object to a memory object is called deserialization. Serializing and deserializing objects is a very expensive and time consuming process. Keeping intermediate results in memory avoids this significant overhead.

Taken together, the faster access times and avoidance of serialization/deserialization overhead make Spark much faster than Map Reduce - up to 100 times faster!


## Python Spark (pySpark)
RDDs are a key concept.
A Spark program is two programs: A driver program and a workers program. 
* Worker programs run on cluster nodes or in local threads.
* RDDs are distributed across the workers.
* First of all, create a SparkContext: Tells Spark how and where to access a cluster
* Use SparkContext to create RDDs.

In Spark, datasets are represented as a list of entries, where the list is broken up into many different partitions that are each stored on a different machine. Each partition holds a unique subset of the entries in the list. Spark calls datasets that it stores "Resilient Distributed Datasets" (RDDs).

For more information about Apache Spark, you should refer to the online Spark Documentation. The documentation includes screencasts, training materials, and hands-on exercises. The Spark Programming Guide is anothe good starting point, and the pySpark API documentation is a great reference to use when writing Spark programs.

## Resilient Distributed Datasets
* It's the first abstraction in Spark.
* They are immutable once constructed. As a result, each transformation creates a new RDD.
* Data lineage: following the life-cycle of the data.
* Programmer specifies the number of a partitions for an RDD: the more partitions, the more parallelism.
* There are two types of operations: transformations (e.g. map, filter, distinct, flatMap, mapPartitions, mapPartitionsWithIndex) and actions (e.g. collect, count).
* Lazy evaluation: results are not computed right away. We are creating a recipe for creating a result.
* RDDs cannot be changed once they are created - they are immutable. You can create RDDs by applying transformations to existing RDDs and Spark automatically tracks how you create and manipulate RDDs (their lineage) so that it can reconstruct any data that is lost due to slow or failed machine. Operations on RDDs are performed in parallel.
* When you run map() on a dataset, a single stage of tasks is launched. A stage is a group of tasks that all perform the same computation, but on different input data. One task is launched for each partitition, as shown in the example below. A task is a unit of execution that runs on a single machine. When we run map(f) within a partition, a new task applies f to all of the entries in a particular partition, and outputs a new partition. In this example figure, the dataset is broken into four partitions, so four map() tasks are launched.


## Transformations
Spark Transformations use lazy evaluation, which means they are not immediately executed. Instead they can be thought of as a recipe for creating a result from an input dataset.

## Spark Actions
* Cause Spark to execute recipe to transform source.
* Mechanism for getting results out of Spark.
* Actions: reduce, take, collect, takeOrdered, first, take, top, takeOrdered, takeSample, countByValue, etc.
** Note that for the first() and take() actions, the elements that are returned depend on how the RDD is partitioned.
** The takeOrdered() action returns the first n elements of the RDD, using either their natural order or a custom comparator. The key advantage of using takeOrdered() instead of first() or take() is that takeOrdered() returns a deterministic result, while the other two actions may return differing results, depending on the number of partions or execution environment. takeOrdered() returns the list sorted in ascending order. The top() action is similar to takeOrdered() except that it returns the list in descending order.
** The reduce() action reduces the elements of a RDD to a single value by applying a function that takes two parameters and returns a single value. The function should be commutative and associative, as reduce() is applied at the partition level and then again to aggregate results from partitions. If these rules don't hold, the results from reduce() will be inconsistent. Reducing locally at partitions makes reduce() very efficient.
* Spark Actions are the mechanism for causing Spark to apply the specified set of transformations to the source data. They are the way that you extract the results out of Spark at the driver.

## Caching RDDs
* Every time you perform an action, ALL the data is reloaded from the partitions. 
* In order to avoid that, you can cache the RDDs. That way, you load from memory, not from disk (slide 27 from week2b).

## Spark program lifecycle
1. Create RDDs from external data or parallelize a collection in your driver program
2. Lazily transform them into new RDDs
3. cache() some RDDs for reuse
4. Perform actions to execute parallel computation and produce results

## Spark key-value RDDs
Key-value transformations:
* reduceByKey()
* sortByKey()
* groupByKey(): Be careful using groupByKey() as it can cause a lot of data movement across the network and create large Iterables at workers.

## pySpark closures
Spark automatically creates closures for:
* Functions that run on RDDs at workers
* Any global variables used by those workers

One closure per worker
* Sent for every task
* No communication between workers
* Changes to global variables at workers are not sent to driver

pySpark shared variables:
* Broadcast variables:
** Sent from the driver to the workers.
** Sent only once to each worker, not once per task.
** Keep read-only variables cached at workers.

In broadcasting, a call-sign is a unique designation for a transmitting station.


* Accumulators
* Variables that can only be “added” to by associative op
* Used to efficiently implement parallel counters and sums
* Only driver can read an accumulator’s value, not tasks

## Summary
So in summary, when you write a Spark program, use the master parameter to specify the number of workers.
When you create an RDD, you can specify the number of partitions for that RDD, and Spark will automatically create that RDD spread across the workers.
When you perform transformations and actions that use functions, Spark will automatically push a closure containing that function to the workers so that it can run at the workers.

## Lab 2
1. Spark tutorial
https://raw.githubusercontent.com/spark-mooc/mooc-setup/master/spark_tutorial_student.ipynb

Spark web UI: http://localhost:4040/jobs/

At a high level, every Spark application consists of a driver program that launches various parallel operations on executor Java Virtual Machines (JVMs) running either in a cluster or locally on the same machine. In Databricks Cloud, "Databricks Shell" is the driver program. When running locally, "PySparkShell" is the driver program. In all cases, this driver program contains the main loop for the program and creates distributed datasets on the cluster, then applies operations (transformations & actions) to those datasets.

xrange() only generates values as they are needed. This is different from the behavior of range() which generates the complete list upon execution. Because of this xrange() is more memory efficient than range(), especially for large ranges.

### Lambda
Python supports the use of small one-line anonymous functions that are not bound to a name at runtime. Borrowed from LISP, these lambda functions can be used wherever function objects are required. They are syntactically restricted to a single expression. Remember that lambda functions are a matter of style and using them is never required - semantically, they are just syntactic sugar for a normal function definition. You can always define a separate normal function instead, but using a lambda() function is an equivalent and more compact form of coding. Ideally you should consider using lambda functions where you want to encapsulate non-reusable code without littering your code with one-line functions.

### Glom
glom(self)
 |      Return an RDD created by coalescing all elements within each partition
 |      into a list.
 |      
 |      >>> rdd = sc.parallelize([1, 2, 3, 4], 2)
 |      >>> sorted(rdd.glom().collect())
 |      [[1, 2], [3, 4]]


### Repartition
repartition(self, numPartitions)
 |      Return a new RDD that has exactly numPartitions partitions.
 |      
 |      Can increase or decrease the level of parallelism in this RDD.
 |      Internally, this uses a shuffle to redistribute data.
 |      If you are decreasing the number of partitions in this RDD, consider
 |      using `coalesce`, which can avoid performing a shuffle.
 |      
 |      >>> rdd = sc.parallelize([1,2,3,4,5,6,7], 4)
 |      >>> sorted(rdd.glom().collect())
 |      [[1], [2, 3], [4, 5], [6, 7]]
 |      >>> len(rdd.repartition(2).glom().collect())
 |      2
 |      >>> len(rdd.repartition(10).glom().collect())
 |      10

groupByKey() and reduceByKey(): both operations operate on a pair RDD.
A pair RDD is an RDD where each element is a pair tuple (key, value). For example, sc.parallelize([('a', 1), ('a', 2), ('b', 1)]) would create a pair RDD where the keys are 'a', 'a', 'b' and the values are 1, 2, 1.
While both the groupByKey() and reduceByKey() transformations can often be used to solve the same problem and will produce the same answer, the reduceByKey() transformation works much better for large distributed datasets. This is because Spark knows it can combine output with a common key on each partition before shuffling (redistributing) the data across nodes. Only use groupByKey() if the operation would not benefit from reducing the data before the shuffle occurs.

* Different ways to sum by key
print pairRDD.groupByKey().map(lambda (k, v): (k, sum(v))).collect()
* Using mapValues, which is recommended when they key doesn't change
print pairRDD.groupByKey().mapValues(lambda x: sum(x)).collect()

Better than groupByKey(), use combineByKey() or foldByKey().

## Caching RDDs
* For efficiency Spark keeps your RDDs in memory. By keeping the contents in memory, Spark can quickly access the data. However, memory is limited, so if you try to keep too many RDDs in memory, Spark will automatically delete RDDs from memory to make space for new RDDs. If you later refer to one of the RDDs, Spark will automatically recreate the RDD for you, but that takes time.
* So, if you plan to use an RDD more than once, then you should tell Spark to cache that RDD. You can use the cache() operation to keep the RDD in memory. However, if you cache too many RDDs and Spark runs out of memory, it will delete the least recently used (LRU) RDD first. Again, the RDD will be automatically recreated when accessed.
* You can check if an RDD is cached by using the is_cached attribute, and you can see your cached RDD in the "Storage" section of the Spark web UI. If you click on the RDD's name, you can see more information about where the RDD is stored.
* Spark automatically manages the RDDs cached in memory and will save them to disk if it runs out of memory. For efficiency, once you are finished using an RDD, you can optionally tell Spark to stop caching it in memory by using the RDD's unpersist() method to inform Spark that you no longer need the RDD in memory.
* You can see the set of transformations that were applied to create an RDD by using the toDebugString() method, which will provide storage information, and you can directly query the current storage information for an RDD using the getStorageLevel() operation.

## How Python is Executed in Spark 
Internally, Spark executes using a Java Virtual Machine (JVM). pySpark runs Python code in a JVM using Py4J. Py4J enables Python programs running in a Python interpreter to dynamically access Java objects in a Java Virtual Machine. Methods are called as if the Java objects resided in the Python interpreter and Java collections can be accessed through standard Python collection methods. Py4J also enables Java programs to call back Python objects.

## Moving toward expert style 
* As you are learning Spark, I recommend that you write your code in the form:
RDD.transformation1()
RDD.action1()
RDD.transformation2()
RDD.action2()
* Using this style will make debugging your code much easier as it makes errors easier to localize - errors in your transformations will occur when the next action is executed.
* Once you become more experienced with Spark, you can write your code with the form:
RDD.transformation1().transformation2().action()


2. Word count
https://raw.githubusercontent.com/spark-mooc/mooc-setup/master/ML_lab2_word_count_student.ipynb

* Regular expressions resources:
https://developers.google.com/edu/python/regular-expressions
https://regex101.com/#python
https://docs.python.org/2/library/re.html