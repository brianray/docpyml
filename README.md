# Python Machine Learning Package on collection of Docker Containers

By: Brian Ray <brianhray@gmail.com>

## Goal

This project's goal is to use docker containers to set up a network of services and workbenches commonly used by Data Scientists working on Machine Learning problems. It's currently marked as **experimental** and contributions are welcome. The Docker Compose file outlines a couple of the containers. They should be configured to work with eachother over the docpyml network you create on your docker VM.

List of Containers:

* docpyml-namenode: Hadoop NameNode. Keeps the directory tree of all files in the file system.
* docpyml-datanode1: Data Storage HadoopFileSystem
* docpyml-datanode2: Data Storage HadoopFileSystem
* docpyml-spark-master: Apache Spark Master
* spark-worker (<- may launch many): Spark Workers
* docpyml-sparknotebook: Preconfigured Spark Notebook
* docpyml-hdfsfb: HDFS FileBrowser from Cloudera Hue 
* docpyml-conda: Anaconda Python 3.5 with Jupyter Notebook, machine learning packages, pySpark preconfigured



## Install

To start the enviroment:

```
    docker network create docpyml
    docker-compose up -d
```

To scale up spark-workers:
```
    docker-compose scale spark-worker=3
```



## Credits

* [The Spark HDFS Workbench](http://www.big-data-europe.eu/scalable-sparkhdfs-workbench-using-docker/)
* [c12e jupyter](https://hub.docker.com/r/c12e/alpine-jupyter-minimal/)
* [pysparklines](https://github.com/RedKrieg/pysparklines)
* [docker-spark](https://github.com/earthquakesan/docker-spark/)
