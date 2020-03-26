# c12e/alpine-miniconda
FROM ubuntu:14.04

MAINTAINER Brian Ray <brianhray@gmail.com>
# based on https://github.com/CognitiveScale/alpine-jupyter

# Install all OS dependencies for fully functional notebook server
RUN apt-get update && \
      apt-get -y install sudo

RUN apt-get -y install curl \
    bzip2 

# Java stuff
RUN apt-get -y install python-software-properties
RUN apt-get -y install software-properties-common
RUN add-apt-repository ppa:openjdk-r/ppa
RUN apt-get update
RUN apt-get -y install openjdk-8-jdk
RUN apt-get -y install unzip 

# Spark stuff
RUN curl -s http://d3kbcqa49mib13.cloudfront.net/spark-1.6.1-bin-hadoop2.6.tgz | tar -xz -C /usr/local/
RUN cd /usr/local && ln -s spark-1.6.1-bin-hadoop2.6 spark
ENV SPARK_HOME /usr/local/spark

# r stuff
RUN apt-get install -y --no-install-recommends  r-cran-rodbc 


# Configure environment
ENV SHELL=/bin/bash \
  NB_USER=docpyml \
  NB_UID=1000 

# Configure environment
ENV CONDA_DIR=/opt/conda CONDA_VER=4.8.2
ENV PATH=$CONDA_DIR/bin:$PATH SHELL=/bin/bash LANG=C.UTF-8

# # Install conda
RUN mkdir -p $CONDA_DIR && \
 echo export PATH=$CONDA_DIR/bin:'$PATH' > /etc/profile.d/conda.sh && \ 
 curl https://repo.continuum.io/miniconda/Miniconda3-py38_${CONDA_VER}-Linux-x86_64.sh -o mconda.sh && \
 /bin/bash mconda.sh -f -b -p $CONDA_DIR && \
 rm mconda.sh && \
 $CONDA_DIR/bin/conda install --yes conda==${CONDA_VER}
RUN conda update conda
RUN conda update --all

# Install Jupyter notebook

# Grant ownership over the conda dir and home dir, but stick the group as root.
# ALPINE Change to: RUN adduser -s /bin/bash -u $NB_UID -D $NB_USER && \
RUN useradd -s /bin/bash -u $NB_UID $NB_USER  && \
    mkdir -p /home/$NB_USER/work \
    && mkdir /home/$NB_USER/.jupyter \
    && mkdir /home/$NB_USER/.local \
    && mkdir -p $CONDA_DIR \
    && chown -R $NB_USER:users $CONDA_DIR \
    && chown -R $NB_USER:users /home/$NB_USER \
    && su $NB_USER -c "$CONDA_DIR/bin/conda install --yes -c conda-forge jupyterhub \
    terminado" \
    && su $NB_USER -c "$CONDA_DIR/bin/conda clean -yt"


# Add local files as late as possible to avoid cache busting
COPY  dev-requirements.txt /home/$NB_USER/.jupyter/
COPY optional-requirements.txt  /home/$NB_USER/.jupyter/
COPY requirements.txt /home/$NB_USER/.jupyter/
COPY start-notebook.sh /usr/local/bin/
RUN chown $NB_USER:users /usr/local/bin/start-notebook.sh
RUN chmod +x /usr/local/bin/start-notebook.sh
COPY jupyterhub_config.py /home/$NB_USER/.jupyter/
RUN chown -R $NB_USER:users /home/$NB_USER/.jupyter
COPY starter_notebooks /home/docpyml/work/

# make up to date
RUN pip install -U pip

# extra stuff
RUN conda install -y numpy
RUN mkdir -p /opt/conda/etc/fonts
RUN chmod -R 777 /opt/conda/etc/fonts
RUN conda install -y matplotlib
RUN conda install -y scikit-learn
RUN conda install -c conda-forge pystan
RUN conda install -c conda-forge xgboost
# RUN conda install -c pytorch pytorch
RUN conda install -c conda-forge cartopy

# Install R
RUN conda install --quiet --channel https://conda.anaconda.org/conda-forge --yes \
    'r-base=3.6.2' \
    'r-caret=6.0*' \
    'r-crayon=1.3*' \
    'r-devtools=2.2*' \
    'r-forecast=8.11*' \
    'r-hexbin=1.28*' \
    'r-htmltools=0.4*' \
    'r-htmlwidgets=1.5*' \
    'r-irkernel=1.1*' \
    'r-nycflights13=1.0*' \
    'r-plyr=1.8*' \
    'r-randomforest=4.6*' \
    'r-rcurl=1.98*' \
    'r-reshape2=1.4*' \
    'r-rmarkdown=2.1*' \
    'r-rodbc=1.3*' \
    'r-rsqlite=2.1*' \
    'r-shiny=1.4*' \
    'r-tidyverse=1.3*' \
    'unixodbc=2.3.*' 

RUN conda install --quiet --yes r-e1071

RUN conda install -c r r-irkernel

# [ TensorFlow ]
RUN apt-get -y install build-essential
#RUN conda install -y --channel https://conda.anaconda.org/conda-forge tensorflow

# Theano Family
#RUN conda install -y theano 
#RUN conda install -y --channel https://conda.anaconda.org/ijstokes lasagne
#RUN conda install -y --channel https://conda.anaconda.org/conda-forge keras

# more stuff
RUN pip install -r  /home/$NB_USER/.jupyter/dev-requirements.txt
RUN pip install -r  /home/$NB_USER/.jupyter/optional-requirements.txt
RUN pip install -r  /home/$NB_USER/.jupyter/requirements.txt

# Configure container startup
EXPOSE 8888
WORKDIR /home/$NB_USER/work
ENTRYPOINT ["/bin/bash"]
CMD ["start-notebook.sh"]
