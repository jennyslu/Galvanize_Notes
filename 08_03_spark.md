# Morning Lecture

## RDD vs DataFrame

DataFrame is more structured than tables in RDD. So how do we talk to structured table with a language like Python?

Scala is language of implementation for Spark. Scala runs in JVM.

However you can talk to Spark in any of these four languages (includes both strongly type and untyped):
 - R
 - Python
 - Scala
 - Java

## Hadoop ecosystem

 - Oozie: for workflow
 - Pig: older scripting
 - Hive: allows SQL-like queries
 - Hbase: can speed up Hive with columnar store

## MapReduce

Easy to abuse and have sprawling and unfocused and unoptimized workflow.

## Resilient Distributed Datasets (RDD)

Unit of data in Spark

Resilient is the opposite of fragile. It is also the opposite of unrecoverable.

Immutable. Why? Fixed and unchanging storage requirements. You can count on it being there.

## RDD operations

```python
# these operations are NOT independent
# groupBy will not work without join before
rdd1.join(rdd2).groupBy(...).filter(...)
.map()
.filter()
.select_one()
# ETC.
```

## SparkContext

SparkContext is enty into Spark

```python
import pyspark as ps #creates sparkcontext
type(sc) #sc is created for you and it's up and running and you can talk to spark

rdd.count() # best to do this first
rdd.collect() #very dangerous; might flood your machine
```

RDD is a list of tuples

## DataFrames

DataFrames can be thought of as RDDs with schema

Everything is __not__ an RDD (anymore).

__Schemas__
 - __Pros__
  * column names instead of positions
   - these are more reliable than indices
  * enable queries using SQL and DataFrame syntax
  * make data more structured, which can catch errors earlier
  * enable columnar optimization: differing compression and reference by column
   - This becomes more important as moving data across distributed servers becomes major part of execution cost
  * reduce overhead of boxing and unboxing data of unknown types

 - __Cons__
  * programmatic overhead
  * restriction on flexibility of data
