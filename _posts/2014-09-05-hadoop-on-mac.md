---
layout: post
title:  "Hadoop on Mac"
date:   2014-09-05 20:30:29
categories: hadoop
---

# Hadoop on Mac

## Installing Hadoop on Mac OS X 10.9.4 

Below are the steps to install and configure Hadoop 2.5 using Homebrew.


### Hadoop Prerequisites

 - Java Version 1.7
 
```
$ java -version
java version "1.7.0_60"
Java(TM) SE Runtime Environment (build 1.7.0_60-b19)
Java HotSpot(TM) 64-Bit Server VM (build 24.60-b09, mixed mode)
```

 - Ruby & Homebrew
 
 ```
 $ ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```

- SSH & Romote Login

Hadoop requires SSH so you'll need to enable it. First, make sure Remote Login under System Preferences -> Sharing is checked (this enables SSH on Mac OS X). Then generate and authorize SSH keys:

```
$ ssh-keygen -t rsa -P ""
$ cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
```

### Hadoop Installation

Homebrew (0.9.5) installs Hadoop v2.5 so the steps below are specific to that version of Hadoop.

 - SSH into localhost
 
 To begin the installation you'll need to SSH into your workstation. Open up console and fire up the ssh client.
 
 ```
 $ ssh localhost
 ```
 
 - Install Hadoop 
 
 ```
 $ brew install hadoop
 ```
 
 - Create HDFS directory:
 
 ```
mkdir -p ~/hadoop/data/namenode
mkdir -p ~/hadoop/data/datanode
```

### Hadoop Configuration

Homebrew will install Hadoop to /usr/local/Cellar/hadoop/2.5.0 directory.
 
 - The important config files are:
 	- hadoop-env.sh
 	- mapred-env.sh
 	- yarn-env.sh
 	- core-site.xml
 	- hdfs-site.xml
 	- mapred-site.xml
 	- yarn-site.xml
 	
 	The files are in /usr/local/Cellar/hadoop/2.5.0/libexec/etc/hadoop
 	
 - hadoop-env.sh
 
 ```
 #export HADOOP_OPTS="$HADOOP_OPTS -Djava.net.preferIPv4Stack=true"
export HADOOP_OPTS="${HADOOP_OPTS} -Djava.security.krb5.realm= -Djava.security.krb5.kdc="
export HADOOP_OPTS="${HADOOP_OPTS} -Djava.security.krb5.conf=/dev/null"
```

- yarn-env.sh

```
YARN_OPTS="$YARN_OPTS -Djava.security.krb5.realm=OX.AC.UK -Djava.security.krb5.kdc=kdc0.ox.ac.uk:kdc1.ox.ac.uk"
```

- core-site.xml

```
<configuration>
 <property>
   <name>hadoop.tmp.dir</name>
   <value>/tmp/hadoop-${user.name}</value>
   <description>A base for other temporary directories.</description>
 </property> 
<property>
   <name>fs.default.name</name>
   <value>hdfs://localhost:9000</value>
 </property>
</configuration>
```
- hdfs-site.xml

```
<configuration>
 <property>
     <name>dfs.replication</name>
     <value>1</value>
 </property>
 <property>
  <name>dfs.namenode.name.dir</name>
  <value>${user.home}/hadoop/data/namenode</value>
</property>

<property>
  <name>dfs.datanode.data.dir</name>
  <value>${user.home}/hadoop/data/datanode</value>
</property
</configuration>
```

- mapred-site.xml

cp /usr/local/Cellar/hadoop/2.5.0/libexec/etc/hadoop/mapred-site.xml.template /usr/local/Cellar/hadoop/2.5.0/libexec/etc/hadoop/mapred-site.xml

```
<configuration>
  <property>
    <name>mapred.job.tracker</name>
    <value>localhost:9001</value>
  </property>
  <property>
  <name>mapreduce.framework.name</name>
  <value>yarn</value>
</property>
</configuration>
```

- yarn-site.xml

```
<configuration>
        <property>
                <name>yarn.resourcemanager.address</name>
                <value>localhost:8032</value>
        </property>

        <property>
                <name>yarn.nodemanager.aux-services</name>
                <value>mapreduce_shuffle</value>
        </property>
</configuration>
```

### Running Hadoop

- Formatting HDFS

```
hadoop namenode -format
```

- Running Hadoop

Execute the following commands (please note the start-all.sh command has been deprecated):


```
$ cd /usr/local/Cellar/hadoop/2.5.0/libexec/sbin
$ ./start-dfs.sh
$ ./start-yarn.sh
		
```
Or

```
echo "export PATH=$PATH:/usr/local/Cellar/hadoop/2.5.0/libexec/bin:/usr/local/Cellar/hadoop/2.5.0/libexec/sbin" >> ~/.bashrc

source ~/.bashrc
```

If you get the warning:

```
WARN util.NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
```

It is because you are running on 64bit but Hadoop native library is 32bit. This is not a big issue. 

- Check status

```
jps
```

Expected output

```
82139 DataNode
82382 ResourceManager
82056 NameNode
82244 SecondaryNameNode
83980 Jps
82474 NodeManager
```

- Access web interfaces:
	- Cluser status: http://localhost:8088
	- HDFS status: http://localhost:50070
    - Secondary NameNode status: http://localhost:50090

- Testing Hadoop

Hadoop ships with a few sample MapReduce jobs that you can run to see whether your HDP instance is running and can accept jobs. Here's a sample MapReduce job to calculate the value of PI:

```
hadoop jar /usr/local/Cellar/hadoop/2.5.0/libexec/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.5.0.jar pi 2 5
Number of Maps  = 2
Samples per Map = 5
...
Job Finished in 17.468 seconds
Estimated value of Pi is 3.60000000000
```

- Scalding Test

[Scalding-tutorial](https://github.com/Cascading/scalding-tutorial)

```
$ git clone git://github.com/Cascading/scalding-tutorial.git
$ cd scalding-tutorial
$ sbt assembly
$ yarn jar target/scalding-tutorial-0.8.11.jar Tutorial0 --local
```

- Stop Hadoop

```
stop-dfs.sh && stop-yarn.sh
```



### Reference

 * [Hadoop Single Node](http://hadoop.apache.org/docs/r2.4.0/hadoop-project-dist/hadoop-common/SingleCluster.html)