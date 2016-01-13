---
title: Reading Spark Souce Code in IntelliJ IDEA
date: 2016-01-09 19:20:43
tags: spark
---

It's a good choice to read spark souce code in IntelliJ IDEA. This tutorial introduces how to do it.

### Get spark repository
1. Fork [apache spark](https://github.com/apache/spark) project to your Github account
2. Clone spark to local:
  
  ```bash
	$ git clone git@github.com:username/spark.git
	$ cd spark/
	```
3. Add apache spark remote (to keep up-to-date with apache spark repo):
	
	```bash
	$ git remote add apache https://github.com/apache/spark.git
	# check remote accounts
	$ git remote -v
	```
4. Sync repo with apache spark:
	
	```bash
	# Fetch the branches and their respective commits from the apache repo
	$ git fetch apache
	# Update codes
	$ git pull apache master
	```
5. Push new updates to your own github account repo:
	
	```bash
	$ git push origin master
	```
6. Create new develop branch for developing: 
	
	```bash
	$ git checkout -b develop
	```
7. Push develop branch to your github repo: 
	
	```bash
	$ git push -u origin develop
	```

### Built spark in Intellij IDEA 15
1. Install [IntelliJ IDEA 15](https://www.jetbrains.com/idea/download/) as well as [IDEA Scala Plugin](https://plugins.jetbrains.com/plugin/?id=1347)
2. Make sure your are in your own develop branch:

	```bash
	$ git checkout develop
	```
3. Open spark project in IDEA (directly open **pom.xml** file)
Menu -> File -> **Open** -> {spark}/**pom.xml** 
4. Modify `java.version` to your java version inside **pom.xml**

	```bash
	# pom.xml
	<java.version>1.8</java.version>
	```
5. Build spark by sbt

	```bash
	$ build/sbt assembly
	```
6. Validating spark is built successfully

	```bash
	$ ./bin/spark-shell
	```

### Reading spark codes
It's better to read or change codes on your develop branch and sync with apache spark repo inside master branch. So normally, you can update your develop branch by following commands:

```bash
$ git checkout master
$ git pull apache master
$ git checkout develop
$ git merge master
```
	
Useful IDEA Shortcuts:

	command + o : search classes
	command + b : go to implementation
	command + [ : go back to the previous location
	shift + command + F : search files 

Several important classes:

	SparkContext.scala 
	DAGScheduler.scala
	TaskSchedulerImpl.scala
	BlockManager.scala
	

------------------------------
Ref: Building Spark: http://spark.apache.org/docs/latest/building-spark.html


