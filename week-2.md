# Introduction to Apache Spark

## Interesting links
* http://aws.amazon.com/elasticmapreduce/
* http://people.csail.mit.edu/matei/papers/2010/hotcloud_spark.pdf

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
Using memory instead of disks offers two huge benefits. The first benefit is that memory is much faster than disks. The time to read or write a value to memory is only a few nanoseconds, while the time to read or write is several milliseconds - that means memory is a million times faster than disks. The second benefit is that keeping intermediate results in memory means that they do not have to be converted into a format that can be stored on disks. The process of converting a memory object to a disk object is called serialization and the process of converting a disk object to a memory object is called deserialization. Serializing and deserializing objects is a very expensive and time consuming process. Keeping intermediate results in memory avoids this significant overhead.

Taken together, the faster access times and avoidance of serialization/deserialization overhead make Spark much faster than Map Reduce - up to 100 times faster!