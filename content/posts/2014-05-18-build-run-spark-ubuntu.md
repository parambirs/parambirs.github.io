---
title: 'Building and running Spark 1.0 on Ubuntu'
date: 2014-05-18T00:00:00-07:00
tags: ["spark"]
summary: "Step-by-step approach for building and running Spark."
---

This article describes a step-by-step approach to build and run Apache Spark 1.0.0-SNAPSHOT. I personally use a virtual machine for testing out different big data softwares (Hadoop, Spark, Hive, etc.) and I’ve used LinuxMint 16 on VirtualBox 4.3.10 for the purpose of this blog post.

Install JDK 7
-------------

```
$ sudo add-apt-repository ppa:webupd8team/java
$ sudo apt-get update
$ sudo apt-get install oracle-java7-installer
```

Verify the Java installation:

```
$ java -version
java version "1.7.0_55"
Java(TM) SE Runtime Environment (build 1.7.0_55-b13)
Java HotSpot(TM) 64-Bit Server VM (build 24.55-b03, mixed mode)
```

Create a symlink for easier configuration later:

```
$ cd /usr/lib/jvm/
$ sudo ln -s java-7-oracle jdk
```

Download Spark
--------------

Note: `parambirs` is my user name as well as group name on the ubuntu machine. Please replace this with your own user/group name

```
$ cd ~/Downloads
$ git clone https://github.com/apache/spark.git
$ sudo mv spark /usr/local
$ cd /usr/local
$ sudo chown -R parambirs:parambirs spark
```

Build
-----

```
$ cd /usr/local/spark
$ sbt/sbt clean assembly
```

Run an Example
--------------

```
$ cd /usr/local/spark
$ ./bin/run-example org.apache.spark.examples.SparkPi 
...
Pi is roughly 3.1399
...
```

Run Spark Shell
---------------

```
$ ./bin/spark-shell
```

Try out some commands in the spark shell

```sh
scala> val textFile=sc.textFile("README.md")
textFile: org.apache.spark.rdd.RDD[String] = MappedRDD[1] at textFile at <console>:12
scala> textFile.count
res0: Long = 126
scala> textFile.filter(_.contains("the")).count
res1: Long = 28
scala> exit
```