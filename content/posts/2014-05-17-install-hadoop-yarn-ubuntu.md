---
title: 'Install Hadoop/YARN 2.4.0 on Ubuntu (VirtualBox)'
date: 2014-05-17T00:00:00-07:00
tags: ["hadoop", "yarn"]
summary: "A step-by-step approach to install Hadoop and Yarn on Ubuntu"
---

This article describes the step-by-step approach to install Hadoop/YARN 2.4.0 on Ubuntu and its derivatives (LinuxMint, Kubuntu etc.). I personally use a virtual machine for testing out different big data softwares (Hadoop, Spark, Hive, etc.) and I’ve used LinuxMint 16 on VirtualBox 4.3.10 for the purpose of this blog post.


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

Install OpenSSH Server
----------------------

```
$ sudo apt-get install openssh-server
$ ssh-keygen -t rsa
```

Hit enter on all prompts i.e. accept all defaults including “no passphrase”. Next, to prevent password prompts, add the public key of this machine to the authorized keys folder (Hadoop services use ssh to talk among themselves even on a single node cluster).

```
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

SSH to localhost to test ssh server and also save localhost in the list of known hosts. Next time when you ssh to `localhost`, there won't be any prompts.

```
$ ssh localhost
```

Download Hadoop
---------------

Note 1: You should use a mirror URL from the official downloads page
Note 2: `parambirs` is my user name as well as group name on the ubuntu machine. Please replace this with your own user/group name

```
$ cd Downloads/
$ wget http://apache.claz.org/hadoop/common/hadoop-2.4.0/hadoop-2.4.0.tar.gz
$ tar zxvf hadoop-2.2.0.tar.gz
$ sudo mv hadoop-2.2.0 /usr/local/
$ cd /usr/local
$ sudo ln -s hadoop-2.2.0 hadoop
$ sudo chown -R parambirs:parambirs hadoop-2.2.0
$ sudo chown -R parambirs:parambirs hadoop
```

Environment Configuration
-------------------------

```
$ cd ~
$ vim .bashrc
```

Add the following to the end of .bashrc file

```sh
#Hadoop variables
export JAVA_HOME=/usr/lib/jvm/jdk/
export HADOOP_INSTALL=/usr/local/hadoop
export PATH=$PATH:$HADOOP_INSTALL/bin
export PATH=$PATH:$HADOOP_INSTALL/sbin
export HADOOP_MAPRED_HOME=$HADOOP_INSTALL
export HADOOP_COMMON_HOME=$HADOOP_INSTALL
export HADOOP_HDFS_HOME=$HADOOP_INSTALL
export YARN_HOME=$HADOOP_INSTALL
```

Modify `hadoop-env.sh`:

```
$ cd /usr/local/hadoop/etc/hadoop
$ vim hadoop-env.sh
```

```
#modify JAVA_HOME
export JAVA_HOME=/usr/lib/jvm/jdk/
```

Verify hadoop installation
--------------------------

```
$ source ~/.bashrc (refresh shell to reflect the configuration changes we’ve made)
$ hadoop version
Hadoop 2.4.0
Subversion http://svn.apache.org/repos/asf/hadoop/common -r 1583262
Compiled by jenkins on 2014-03-31T08:29Z
Compiled with protoc 2.5.0
From source with checksum 375b2832a6641759c6eaf6e3e998147
This command was run using /usr/local/hadoop-2.4.0/share/hadoop/common/hadoop-common-2.4.0.jar
```

Hadoop Configuration
--------------------

```
$ cd ~
$ mkdir -p mydata/hdfs/namenode
$ mkdir -p mydata/hdfs/datanode
```

### core-site.xml

```
$ cd /usr/local/hadoop/etc/hadoop/
$ vim core-site.xml
```

Add the following between the `<configuration></configuration>` elements

```xml
<property>
 <name>fs.default.name</name>
 <value>hdfs://localhost:9000</value>
</property>
```

### yarn-site.xml

```
$ vim yarn-site.xml
```

Add the following between the `<configuration></configuration>` elements

```xml
<property>
 <name>yarn.nodemanager.aux-services</name>
 <value>mapreduce_shuffle</value>
</property>
<property>
 <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
 <value>org.apache.hadoop.mapred.ShuffleHandler</value>
</property>
```
### mapred-site.xml

```
$ cp mapred-site.xml.template mapred-site.xml
$ vim mapred-site.xml
```

Add the following between the `<configuration></configuration>` elements

```xml
<property>
 <name>mapreduce.framework.name</name>
 <value>yarn</value>
</property>
```

### hdfs-site.xml

```
$ vim hdfs-site.xml
```

Add the following between the `<configuration></configuration>` elements. Replace `/home/parambirs` with your own home directory.

```xml
<property>
 <name>dfs.replication</name>
 <value>1</value>
 </property>
 <property>
 <name>dfs.namenode.name.dir</name>
 <value>file:/home/parambirs/mydata/hdfs/namenode</value>
 </property>
 <property>
 <name>dfs.datanode.data.dir</name>
 <value>file:/home/parambirs/mydata/hdfs/datanode</value>
 </property>
```

Running Hadoop
--------------

Format the namenode:

```
$ hdfs namenode -format
```

Start hadoop:

```
$ start-dfs.sh
$ start-yarn.sh
```

Verify all services are running:

```
$ jps
5037 SecondaryNameNode
4690 NameNode
5166 ResourceManager
4777 DataNode
5261 NodeManager
5293 Jps
```

Check web interfaces of different services:

- Namenode: http://localhost:50070
- YARN: http://localhost:8088


### Run a hadoop example MR job

```
$ cd /usr/local/hadoop
$ hadoop jar ./share/hadoop/mapreduce/hadoop-mapreduce-examples-2.4.0.jar pi 2 5
```

References
----------

- http://codesfusion.blogspot.in/2013/10/setup-hadoop-2x-220-on-ubuntu.html
