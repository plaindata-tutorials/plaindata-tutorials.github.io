---
title: "PySpark Performance Optimization"
layout: post
---

Good clean read is set up with readability first in mind. Whatever you want to communicate here can be read easily, and without distraction. Of course, it’s fully responsive, which means people can read it naturally on any phone, or tablet. Write it in markdown in index.md and get a beautifully published piece.

In this post I summarize techniques to optimize the performance of PySpark code.

Subscribe to my **newsletter** if you appreciate the brevity of my writing.


### Table of Contents
1. [Intro](#Intro)
1. [Sources](#Sources)
2. [Overview](#Overview)
3. [Techniques](#third-example)
4. [Fourth Example](#fourth-examplehttpwwwfourthexamplecom)

# Intro 

Performance Tuning or Optimization is somewhat of a dark art as there is no one size fits all solution to Spark performance problems but there are some best practices and techniques that can guide you in finding a solution that works in your particular case.

## Sources

The techniques in this post are compiled mainly from four books:
- <i class="fa fa-book fa-fw"></i> Spark: The Definitive Guide (2018)
    - _The_ authorative book on Spark, written by its original creators
    - Chapter 19 (Performance Tuning)
- <i class="fa fa-book fa-fw"></i> Learning Spark, 2nd Edition (2020) ([link](https://pages.databricks.com/rs/094-YMS-629/images/LearningSpark2.0.pdf))
    - The widely referenced and more modern version of the former, praised by the original creators of Spark
    - Chapter 7 (Optimizing and Tuning Spark Applications)
- <i class="fa fa-book fa-fw"></i> Data Analytics with Spark Using Python
    - Chaper 11 (Advanced Programming Using the Spark API)

Some techniques are mentioned in several of these books while others are only mentioned in one of them respectively. 

Other books I read but there wasn't any additional knowledge to gain in terms of performance optimization:
- <i class="fa fa-book fa-fw"></i> Frank Kane's Taming Big Data with Apache Spark and Python
- <i class="fa fa-book fa-fw"></i> PySpark Recipes - A Problem-Solution Approach with PySpark2

## Overview

This table gives an overview of the techniques. 

In the next section below are more details about each of them.

| Area | Direct/indirect| Technique | When to apply | Example Code |
| -------- | -------- | -------- | -------- | -------- |
| Design Choices | Indirect | Avoiding UDFs | When using Pyspark (instead of Spark's Scala API)
| Cluster Configurations | Indirect | Dynamic Allocation | Multiple Spark jobs running on the same cluster at the same time
| Storage (read performance) | Indirect | Using a columnar file format (e.g. Parquet) for file storage | Always
| Text     | Text     | Text     |
| Text     | Text     | Text     |
| Text     | Text     | Text     |
| Text     | Text     | Text     |
| Text     | Text     | Text     |

# Techniques

## Avoiding User Defined Functions (UDFs)

**Avoid UDFs when writing Pyspark code**. While all of Spark's Structured APIs (Python, Scala, R, Java) are generally equivalent in terms of performance and stability, that's only true as long as you avoid UDFs. 

The big problem is that using Python UDFs forces Spark to send all data to a single Python context on a single node to apply the UDF. This leads to several problems:
1. Transferring the data from other nodes to the one holding the Python context is expensive. And afterwards the data will be distributed across the nodes again. Having a Python UDF in your code leads to these two very expensive operations.
2. Not only does the data have to travel over the network with its limited bandwidth, it will also have to be serialized and deserialized when it is transferred from JVM contexts to the Python context and back to the JVM contexts. This uses up ypur compute resources.
3. It also defeats the purpose of using a distributed processing library and a cluster when your data ends up being processed on a single machine while the other machines are sitting basically idle.

In case you need custom transformations for your data, you should use Scala. As a JVM language it doesn't suffer from the Python context problem. When you're bound to using Pyspark, you should stick to using Spark's built-in transformations instead of using UDFs.

**Side note:** Don't let a clean-coder tell you that UDFs are superior because they are unit-testable. You can write functions for your Pyspark application but they should be limited to calling the built-in transformations.

## Using Dynamic Allocation

**Dynamic Allocation** is a mechanism to dynamically adjust the amount of resources used by the application. This is particularly useful when there are multiple applications running on the same cluster. The resources occupied by each individual application are freed and requested as needed. In contrast Spark applications that don't using Dynamic Allocation will request a fixed amount of resources for their entire runtime.

To use Dynamic Allocation, set `spark.dynamicAllocation.enabled` to `True`. 

In the [Spark Documentation](https://spark.apache.org/docs/latest/configuration.html#dynamic-allocation) you will find further parameters to adjust the specifics of how the dynamic allocation is performed.

## Using a columnar file format

Columnar file formats, by far the most popular one being `Parquet` files, are superior for big data file storage compared to e.g. CSV or JSON. The gist of it is:

- Often you don't need all columns of a dataset for any particular data transformation. Imagine a dataset with 100 columns but you only need 1 of them. Columnar file formats allow you to only parse the parts of the file that are needed. This saves you in multiple ways:
    - Less CPU spent on parsing files
    - Less data being loaded into RAM

As a bonus, Parquet files and similar file formats contain their own schema. That's not the case with CSVs and JSONs. This helps with ensuring that data is interpreted in the intended way. Imagine a Boolean column but the information is encoded as 1 and 0 or some ID that is all numeric but should be treated as a String. Without a schema there's some degree of ambiguity about the data type of each column.

## Partitioning



## Caching
mentioned in Learning Spark
- cache()
- cacheTable()
- clearCache()

## Persisting
mentioned in Learning Spark
- persist()

## Correct Partitioning

- getNumPartitions()
- partitionBy()

## Determining the optimal number of partitions
- DatAnalytics (p. 123)
- Rule of thumb: 
    - 2x the number of cores in your cluster as a starting point
    - subject to experimentation, keep increasing the number until the performance degrades

## Repartitioning DataFrames

- repartition()
- coalesce()
- repartitionAndSortWithinPartitions()

## Broadcast Hash Join
mentioned in Learning Spark

## Shared variables

### Broadcast Variables
mentioned in SparkDefinitiveGuide, DataAnalytics
sc.broadcast(variables) --> uses peer-to-peer replication to all nodes

Makes joins between a small and a big dataset more efficient by eleminating the need for a shuffle

These are serialized objects (efficiently read) 

Exist once per worker, not per task 

- unpersist() - removes variable from memory
    - can be blocking (bloing=True) or non-blocking
- Spark Configuration settings relatedd to Broadcast Variables
    - spark.broadcast.compress
        - Default = True (recommended)
    - spark.broadcast.factory
    - spark.broadcast.blockSize
    - spark.broadcast.port

### Accumulators

my_acc = sc.accumulator(0)

- Can be updated, unlike broadcast variables. They are always numeric (integer or float) and can be incremented.
- Updated once at the end of a task
- DataAnalytics (p 116) has example code for usage

Typically used for counting something.

## Shuffle Sort Merge Join
mentioned in LearningSpark

## Configurations
mentioned in LearningSpark

### setting A

### setting B

### setting C

## Indirect Performance Enhancements
defined in SparkDefinitiveGuide

## Direct Performance Enhancements
defined in SparkDefinitiveGuide


## Data Layout
- (p. 88-114 GlueBook)

### Storage format (faster read)

- parquet
- partitioned data storahe, hive style (Simplify EMR p. 373)

## Avoid Python UDFs (my own)

## Adaptive Query Execution

## General Tips
- Spark UI Tabs

## Avoid wide transformations
These trigger a shuffle which is an operation that physically sends data over the network between nodes in the cluster. This is considered an expesive operation that should be avoided whenever possible.
Examples of wide transformations:
- ff
- ss
- ff


## Python specific considerations

- serialization/deserialization
- UDFs

---
###### tags: `Pyspark` `Spark`

---
---
---

# POST 2) Bonus Tip: Do you need Spark? 

The best optimization technique for distributed computing is avoiding it when not needed. Read **this post** to learn why. The gist of it is that distributed computing is costly in terms of both development effort and infrastructure cost. Yes, even with the help of Spark and even in the cloud (often especially in the cloud).

Distributed computing comes at a cost:
- **Working with Spark is hard**. This means:
    - Higher development cost 
    - Longer development time
    - Lower number of qualified developers
    - Highly paid engineers will spend more time to implement the same data transformations in Pyspark vs. using single-machine libraries such as Polars
- **Compute clusters are expensive** 
    - This is true even, and often especially, in the cloud. Short example: processing even the tiniest dataframe in AWS Glue costs at least 0.40 USD per job run while it's virtually free on AWS Lambda
        - No, you're not saving yourself development time by being "consistent". Process small data jobs with small data technologies even when you already got big data technologies in place for big data jobs.
- **Mistakes are expensive** and can escalate quickly:
    - What if an engineer makes a mistake and spins up 100 expensive machines to run an in-efficiently implemented Pyspark-Job for hours? Your entire project's budget might be blown away instantly and you end up wishing you didn't go the Spark route. I'm mentioning this because I saw it happening.

What I want to say is this: Make sure you actually need the terrabyte scale power of distributed computing when you're using Spark. Because in many cases you don't and in these cases you're better off avoiding distributed processing. That's the case whenever the data you're processing fits into memory. 

Say you've got a dataset that's 5000 GB (5 TB) per year but it's distributed evenly over time and you're only processing one day at a time. This means you're actually just processing around 13.4 GB at a time. Your data easily fits into memory of a single machine with 32 GB RAM for example and there are lots of optimization techniques for single machines to exhaust as well.

---
---
---

# POST 3) Spark performance myths
- Scala is better than Python
    - "Spark's Strucutred APIs are consistent across languages in terms of speed and stability" (p. 316 SparkDefinitive)
        - Caveat: UDFs are more performant in Scala
            - "We find that using Python for the majority of the application, and porting some of it to Scala or writing specific UDFs in Scala as your application evolves, is a powerful technique - it allows for a nice balance between overall usability, maintainability, and performance" (p. 317 SparkDefinitive)
- SQL API is slow
    - "Across all languages, DataFrames, Datasets and SQL are equivalent in speed"
- ##
