FROM earthquakesan/hadoop-spark:1.0.0
MAINTAINER Ivan Ermilov <ivan.s.ermilov@gmail.com>

#modified by @brianray

RUN apt-get -y install curl \
    bzip2 

# Configure environment
ENV CONDA_DIR=/opt/conda CONDA_VER=4.0.5
ENV PATH=$CONDA_DIR/bin:$PATH SHELL=/bin/bash LANG=C.UTF-8

# Install conda
RUN mkdir -p $CONDA_DIR && \
    echo export PATH=$CONDA_DIR/bin:'$PATH' > /etc/profile.d/conda.sh && \
    curl https://repo.continuum.io/miniconda/Miniconda3-${CONDA_VER}-Linux-x86_64.sh  -o mconda.sh && \
    /bin/bash mconda.sh -f -b -p $CONDA_DIR && \
    rm mconda.sh && \
    $CONDA_DIR/bin/conda install --yes conda==${CONDA_VER}

RUN ln -s /opt/conda/bin/python /usr/bin/python
RUN conda install -y numpy


ADD spark-worker-entrypoint.sh /spark-worker-entrypoint.sh
RUN chmod a+x /spark-worker-entrypoint.sh



ENTRYPOINT ["/spark-worker-entrypoint.sh"]