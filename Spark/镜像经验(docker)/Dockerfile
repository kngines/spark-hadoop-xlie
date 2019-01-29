FROM sequenceiq/hadoop-docker:2.6.0
MAINTAINER kngines

#update to java 8
ADD jdk-8u25-linux-x64.tar.gz /usr/java/
RUN cd /usr/java && rm -f latest && ln -s ./jdk1.8.0_25 latest && rm -rf jdk1.7.0_51
#adjust yarn-site.xml
RUN rm -f /usr/local/hadoop/etc/hadoop/yarn-site.xml
ADD yarn-site.xml /usr/local/hadoop/etc/hadoop/

#support for Hadoop 2.6.0
ADD ./spark-2.3.2-bin-hadoop2.6.tgz /usr/local/
RUN cd /usr/local && ln -s spark-2.3.2-bin-hadoop2.6 spark
ENV SPARK_HOME /usr/local/spark
RUN mkdir $SPARK_HOME/yarn-remote-client
ADD yarn-remote-client $SPARK_HOME/yarn-remote-client

RUN $BOOTSTRAP && $HADOOP_PREFIX/bin/hadoop dfsadmin -safemode leave && $HADOOP_PREFIX/bin/hdfs dfs -put $SPARK_HOME-2.3.2-bin-hadoop2.6/jars /spark && $HADOOP_PREFIX/bin/hdfs dfs -put $SPARK_HOME-2.3.2-bin-hadoop2.6/examples/jars /spark

ENV YARN_CONF_DIR $HADOOP_PREFIX/etc/hadoop
ENV PATH $PATH:$SPARK_HOME/bin:$HADOOP_PREFIX/bin
# update boot script
COPY bootstrap.sh /etc/bootstrap.sh
RUN chown root.root /etc/bootstrap.sh
RUN chmod 700 /etc/bootstrap.sh

#install R
RUN rpm -ivh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
RUN yum -y install R

ENTRYPOINT ["/etc/bootstrap.sh"]
