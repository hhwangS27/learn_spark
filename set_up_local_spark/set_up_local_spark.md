# Install JDK

Spark only needs JRE, but we install JDK which contains JRE instead.

```
sudo apt install openjdk-11-jre-headless
```

In `.bashrc`

```
#set java environment value
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export PATH=$PATH:$JAVA_HOME/bin
```


# Install Spark

If you run spark on your own computer, there is no need of Hadoop.
Hadoop is for cluster which offers YARN and HDFS.  

Just download the spark file, extract it and config as below:

In `.bashrc`

```
#set spark environment value
export SPARK_HOME=/home/honghuiw/spark-2.4.0-bin-hadoop2.7
export PATH=$PATH:$SPARK_HOME/bin
export PYSPARK_PYTHON=python3
export PYSPARK_DRIVER_PYTHON=jupyter-notebook
```

# Run pyspark

when start pyspark, it will start a python deamon process using PythonRDD.
It fork a new python worker when never a new task come and there is not enought worker.


