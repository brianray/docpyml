# c12e/alpine-miniconda
FROM ubuntu:12.04

MAINTAINER Brian Ray <brianhray@gmail.com>
# based on https://github.com/CognitiveScale/alpine-jupyter

# Install all OS dependencies for fully functional notebook server
RUN apt-get update && \
      apt-get -y install sudo

RUN apt-get -y install curl \
    bzip2 

# Configure environment
ENV SHELL=/bin/bash \
  NB_USER=docpyml \
  NB_UID=1000 

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
    && su $NB_USER -c "$CONDA_DIR/bin/conda install --yes \
    'notebook=4.0*' \
    terminado" \
    && su $NB_USER -c "$CONDA_DIR/bin/conda clean -yt"


# Add local files as late as possible to avoid cache busting
COPY start-notebook.sh /usr/local/bin/
RUN chown $NB_USER:users /usr/local/bin/start-notebook.sh
RUN chmod +x /usr/local/bin/start-notebook.sh
COPY jupyter_notebook_config.py /home/$NB_USER/.jupyter/
RUN chown -R $NB_USER:users /home/$NB_USER/.jupyter

# Configure container startup
EXPOSE 8888
WORKDIR /home/$NB_USER/work
ENTRYPOINT ["/bin/bash"]
CMD ["start-notebook.sh"]
