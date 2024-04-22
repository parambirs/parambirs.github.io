---
title: 'Writing and Running a Standalone App with Spark 1.0 and YARN 2.4.0'
date: 2014-05-20T00:00:00-07:00
tags: ["spark", "yarn"]
summary: "A step-by-step guide to help you write, build and run a standalone Scala app on Spark 1.0"
---

This article is a step-by-step guide to help you write, build and run a standalone app on Spark 1.0 written in Scala. It assumes that you have a working installation of Spark 1.0. 

Install SBT
==============

We’ll use Scala SBT (Simple Build Tool) for building our standalone app. To install sbt, follow these steps.

Note: Replace parambirs:parambirs with your username:groupname combination

```
$ cd ~/Downloads
$ wget http://dl.bintray.com/sbt/native-packages/sbt/0.13.2/sbt-0.13.2.tgz
$ tar zxvf sbt-0.13.2.tgz 
$ sudo mv sbt /usr/local
$ cd /usr/local
$ sudo chown -R parambirs:parambirs sbt
$ vim ~/.bashrc
```

Add the following at the end of the file

```
export PATH=$PATH:/usr/local/sbt/bin
```

Refresh bash environment to reflect sbt addition to path

```
$ source ~/.bashrc
```

Download some sample data for our app and add it to hdfs

```
$ cd ~/Downloads
$ wget http://www.gutenberg.org/ebooks/1342.txt.utf-8
$ hadoop dfs -put 1342.txt.utf-8 /user/parambirs/pride.txt
$ hadoop dfs -ls /user/parambirs
-rw-r--r-- 1 parambirs supergroup 717569 2014-05-20 13:56 /user/parambirs/pride.txt
```

Write a simple app
==================

```
$ cd ~
$ mkdir -p SimpleApp/src/main/scala
$ cd SimpleApp
$ vim src/main/scala/SimpleApp.scala
```

Add the following code to `SimpleApp.scala` file. The code loads the text of “Pride and Prejudice” book by Jane Austen and calculates the number of lines that contain the letter “a” and “b”. It then prints this data to the console.

```scala
/*** SimpleApp.scala ***/
import org.apache.spark._

object SimpleApp {
  def main(args: Array[String]) {
    val logFile = "hdfs://localhost:9000/user/parambirs/pride.txt"
    val conf = new SparkConf().setAppName("Simple App")
    val sc = new SparkContext(conf)
    val logData = sc.textFile(logFile, 2).cache()
    val numAs = logData.filter(line => line.contains("a")).count()
    val numBs = logData.filter(line => line.contains("b")).count()
    println("Lines with a: %s, Lines with b: %s".format(numAs, numBs))
  }
}
```

Create sbt build file
---------------------

```
$ vim simple.sbt
```

Add the following content to the build file

```scala
name := "Simple Project"

version := "1.0"

scalaVersion := "2.10.4"

libraryDependencies += "org.apache.spark" %% "spark-core" % "1.0.0-SNAPSHOT"

resolvers += "Akka Repository" at "http://repo.akka.io/releases/"

libraryDependencies += "org.apache.hadoop" % "hadoop-client" % "2.4.0"
```

The final structure for the source folder should be like this

```
$ find .
./build.sbt
./src
./src/main
./src/main/scala
./src/main/scala/SimpleApp.scala
```

Building the App
================

Before we build our app, we need to publish spark-1.0 artifacts to local maven repository because our app has a dependency on it.

```
$ cd /usr/local/spark
$ sbt/sbt publish-local
```

Now we can build our app

```
$ cd ~/SimpleApp
$ sbt package
```

Running the app on YARN
=======================

We’ll use the yarn cluster mode (using `spark-submit`) to run our app. This sends our app as well as the spark assembly to the yarn cluster and the code is executed remotely.

```
$ cd /usr/local/spark
$ SPARK_JAR=./assembly/target/scala-2.10/spark-assembly-1.0.0-SNAPSHOT-hadoop2.4.0.jar HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop ./bin/spark-submit --master yarn --deploy-mode cluster --class SimpleApp --num-executors 3 --driver-memory 4g --executor-memory 2g --executor-cores 1 ~/SimpleApp/target/scala-2.10/simple-project_2.10-1.0.jar
```

Checking the output
===================

The output of the program is stored in yarn logs. On my machine, the application Id for this app was application_1400566266818_0007 and therefore, the output could be read using the following command

```
$ cat /usr/local/hadoop/logs/userlogs/application_1400566266818_0007/container_1400566266818_0007_01_000001/stdout
Lines with a: 10559, Lines with b: 5874
```
