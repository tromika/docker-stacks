# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
FROM jupyter/pyspark-notebook

MAINTAINER Jupyter Project <jupyter@googlegroups.com>

USER root

# RSpark config
ENV R_LIBS_USER $SPARK_HOME/R/lib

# R pre-requisites
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    fonts-dejavu \
    gfortran \
    gcc && apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN apt-get install -y --no-install-recommends openjdk-7-jre-headless && \
    apt-get clean

# Mesos dependencies
ENV VERSION 0.25.0
ENV PKG_RELEASE 0.2.70

RUN apt-get -y install curl

RUN \
  curl -s -O https://downloads.mesosphere.io/master/debian/8/mesos_${VERSION}-${PKG_RELEASE}.debian81_amd64.deb && \
  dpkg --unpack mesos_${VERSION}-${PKG_RELEASE}.debian81_amd64.deb && \
  apt-get install -f -y && \
  rm mesos_${VERSION}-${PKG_RELEASE}.debian81_amd64.deb && \
  apt-get clean

USER jovyan

# R packages
RUN conda config --add channels r && \
    conda install --quiet --yes \
    'r-base=3.2*' \
    'r-irkernel=0.5*' \
    'r-ggplot2=1.0*' \
    'r-rcurl=1.95*' && conda clean -tipsy

# Apache Toree kernel
RUN pip install toree==0.1.2
RUN jupyter toree install --user --kernel_name='Apache_Toree'
