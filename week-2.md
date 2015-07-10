# Introduction to Apache Spark

## Interesting links
* http://aws.amazon.com/elasticmapreduce/
* http://people.csail.mit.edu/matei/papers/2010/hotcloud_spark.pdf
* https://databricks.com/blog/2014/11/05/spark-officially-sets-a-new-record-in-large-scale-sorting.html
* MLlib: Machine Learning in Apache Spark: http://arxiv.org/pdf/1505.06807.pdf

* Spark documentation: https://spark.apache.org/documentation.html
* Spark Programming guide: https://spark.apache.org/docs/latest/programming-guide.html


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

For more information about Apache Spark, you should refer to the online Spark Documentation. The documentation includes screencasts, training materials, and hands-on exercises. The Spark Programming Guide is anothe good starting point, and the pySpark API documentation is a great reference to use when writing Spark programs.

## Resilient Distributed Datasets
* It's the first abstraction in Spark.
* They are immutable once constructed.
* Data lineage: following the life-cycle of the data.
* Programmer specifies the number of a partitions for an RDD: the more partitions, the more parallelism.
* There are two types of operations: transformations (e.g. map, filter, distinct, flatMap) and actions (e.g. collect, count).
* Lazy evaluation: results are not computed right away. We are creating a recipe for creating a result.
* RDDs cannot be changed once they are created - they are immutable. You can create RDDs by applying transformations to existing RDDs and Spark automatically tracks how you create and manipulate RDDs (their lineage) so that it can reconstruct any data that is lost due to slow or failed machine. Operations on RDDs are performed in parallel.

## Transformations
Spark Transformations use lazy evaluation, which means they are not immediately executed. Instead they can be thought of as a recipe for creating a result from an input dataset.

## Spark Actions
* Cause Spark to execute recipe to transform source.
* Mechanism for getting results out of Spark.
* Actions: reduce, take, collect, takeOrdered.
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

2. Word count
https://raw.githubusercontent.com/spark-mooc/mooc-setup/master/ML_lab2_word_count_student.ipynb