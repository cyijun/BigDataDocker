FROM ubuntu:jammy
LABEL maintainer="Chen Yijun"

ARG BUILD_DATE
ARG BUILD_VERSION

LABEL org.label-schema.build-date=$BUILD_DATE
LABEL org.label-schema.version=$BUILD_VERSION

WORKDIR /opt

COPY requirements.txt /opt

RUN ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone && \
    apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y apt-transport-https ca-certificates wget ssh rsync openjdk-8-jdk build-essential && \
    wget -q https://archive.apache.org/dist/spark/spark-3.1.1/spark-3.1.1-bin-hadoop2.7.tgz && \
    wget -q https://archive.apache.org/dist/hadoop/common/hadoop-2.7.4/hadoop-2.7.4.tar.gz && \
    wget -q https://downloads.lightbend.com/scala/2.12.19/scala-2.12.19.deb && \
    dpkg -i scala-2.12.19.deb && \
    tar -xvf spark-3.1.1-bin-hadoop2.7.tgz && \
    tar -xvf hadoop-2.7.4.tar.gz && \
    rm spark-3.1.1-bin-hadoop2.7.tgz hadoop-2.7.4.tar.gz scala-2.12.19.deb && \
    useradd --home /app -M app -K UID_MIN=10000 -K GID_MIN=10000 -s /bin/bash && \
    mkdir /app && \
    chown app:app -R /app && \
    chown app:app -R /opt/hadoop-2.7.4 && \
    chown app:app -R /opt/spark-3.1.1-bin-hadoop2.7 && \
    apt-get clean

ENV JAVA_HOME="/usr/lib/jvm/java-8-openjdk-amd64"
ENV SCALA_HOME="/usr/share/scala"
ENV HADOOP_HOME="/opt/hadoop-2.7.4"
ENV SPARK_HOME="/opt/spark-3.1.1-bin-hadoop2.7"
ENV LD_LIBRARY_PATH="/opt/hadoop-2.7.4/lib/native"
ENV HADOOP_COMMON_LIB_NATIVE_DIR="${HADOOP_HOME}/lib/native"
ENV HADOOP_OPTS="-Djava.library.path=${HADOOP_COMMON_LIB_NATIVE_DIR}"
ENV PATH="${PATH}:${SPARK_HOME}/bin:${SPARK_HOME}/sbin:${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin:/app/.local/bin"

USER app
WORKDIR /app

ENV PATH="${PATH}:/app/miniconda3/bin"
RUN wget -q https://repo.anaconda.com/miniconda/Miniconda3-py39_24.4.0-0-Linux-x86_64.sh && \
    bash ./Miniconda3-py39_24.4.0-0-Linux-x86_64.sh -b && \
    rm -rf ./Miniconda3-py39_24.4.0-0-Linux-x86_64.sh && \
    conda init bash
ENV PYSPARK_PYTHON=/app/miniconda3/bin/python
ENV PYSPARK_DRIVER_PYTHON=/app/miniconda3/bin/python
RUN pip install -r /opt/requirements.txt && \
    pip cache purge