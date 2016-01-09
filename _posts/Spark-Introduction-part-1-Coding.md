---
title: Spark Introduction part 1 Coding
date: 2016-01-09 17:22:11
tags: spark
---

## Basic Functions
```scala
sc.parallelize(List(1,2,3,4,5,6)).map(_ * 2).filter(_ > 5).collect()
*** res: Array[Int] = Array(6, 8, 10, 12) ***

val rdd = sc.parallelize(List(1,2,3,4,5,6,7,8,9,10))
rdd.reduce(_+_)
*** res: Int = 55 ***
```

### union & intersection & join & lookup

```scala
val rdd1 = sc.parallelize(List(("a", 1), ("a", 2), ("b", 1), ("b", 3)))
val rdd2 = sc.parallelize(List(("a", 3), ("a", 4), ("b", 1), ("b", 2)))

val unionRDD = rdd1.union(rdd2)
unionRDD.collect() 
*** res: Array((a,1), (a,2), (b,1), (b,3), (a,3), (a,4), (b,1), (b,2)) ***

val intersectionRDD = rdd1.intersection(rdd2)
intersectionRDD.collect() 
*** res: Array[(String, Int)] = Array((b,1)) ***

val joinRDD = rdd1.join(rdd2)
joinRDD.collect()
*** res: Array[(String, (Int, Int))] = Array((a,(1,3)), (a,(1,4)), (a,(2,3)), (a,(2,4)), (b,(1,1)), (b,(1,2)), (b,(3,1)), (b,(3,2))) ***

rdd1.lookup("a")
*** res: Seq[Int] = WrappedArray(1, 2) *** 

unionRDD.lookup("a")
*** res: Seq[Int] = WrappedArray(1, 2, 3, 4) ***

joinRDD.lookup("a")
*** res: Seq[(Int, Int)] = ArrayBuffer((1,3), (1,4), (2,3), (2,4)) ***
```

<!-- more -->

### chars count example

```scala
val rdd = sc.textFile("/Users/tony/spark/spark-xiaoxiang-v1/chapter-01/char.data")

val charCount = rdd.flatMap(_.split(" "))
                   .map(char => (char.toLowerCase, 1))
                   .reduceByKey(_+_)
charCount.collect()

charCount.saveAsTextFile("/Users/tony/spark/spark-xiaoxiang-v1/chapter-01/result")

val charCountSort = rdd.flatMap(_.split(" "))
                       .map(char => (char.toLowerCase, 1))
                       .reduceByKey(_+_)
                       .map( p => (p._2, p._1) )
                       .sortByKey(false)
                       .map( p => (p._2, p._1) )
charCountSort.collect()
```


## Cluster Programming
/sbin: start or stop spark cluster
/bin:  start programs like spark-shell

### Cluster configuration
==scp these same configurations to all the cluster nodes==

```bash
spark-env.sh
-----------------export JAVA_HOME=export SPARK_MASTER_IP=localhostexport SPARK_WORKER_CORES=export SPARK_WORKER_INSTANCES=export SPARK_WORKER_MEMORY=export SPARK_MASTER_PORT=export SPARK_JAVA_OPTS="-verbose:gc -XX:-PrintGCDetails”
# if set up on server, uncomment the below line
# export SPARK_PUBLIC_DNS=ec2-54-179-156-156.ap-southeast-1.compute.amazonaws.comslaves
----------xx.xx.xx.2xx.xx.xx.3xx.xx.xx.4xx.xx.xx.5

spark-defaults.conf 
--------------------
spark.eventLog.enabled           true
spark.eventLog.dir               /tmp/spark-events
spark.history.fs.logDirectory    /tmp/spark-log-directory

spark.master spark://server:7077 
spark.local.dir /data/tmp_spark_dir/ 
spark.executor.memory 10g
```

### Run spark-shell on cluster

```bash
MASTER=local[4] ADD_JARS=code.jar ./spark-shell

MASTER=spark://localhost:7077 ./spark-shell
```

```bash
hdfs dfs -getmerge /path result
wc -l result
head result
tail result
```




