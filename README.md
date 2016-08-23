# Python Machine Learning Package on collection of Docker Containers

By: Brian Ray <brianhray@gmail.com>

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
