############################################
# version : ubuntu-hadoop3
# desc : ubuntu上安装的ssh，java, hadoop
############################################
FROM centos7-ssh

# 下面是一些创建者的基本信息
MAINTAINER zhaominghui<342015908qq.com>

#为hadoop集群提供dns服务
RUN yum -y install bind bind-chroot

#安装和配置java环境
#ADD http://mirrors.linuxeye.com/jdk/jdk-8u192-linux-x64.tar.gz /usr/local/ 
COPY jdk-8u192-linux-x64.tar.gz /usr/local/
RUN cd /usr/local && tar -zxvf jdk-8u192-linux-x64.tar.gz && rm -f jdk-8u192-linux-x64.tar.gz

ENV JAVA_HOME /usr/local/jdk1.8.0_192
ENV CLASSPATH ${JAVA_HOME}/lib/dt.jar:$JAVA_HOME/lib/tools.jar
ENV PATH $PATH:${JAVA_HOME}/bin

#安装和配置 hadoop
RUN groupadd hadoop
RUN useradd -m hadoop -g hadoop
RUN echo "hadoop:hadoop" | chpasswd
 
#ADD http://mirrors.shu.edu.cn/apache/hadoop/common/hadoop-3.0.3/hadoop-3.0.3.tar.gz  /usr/local/
COPY hadoop-3.0.3.tar.gz /usr/local/
RUN cd /usr/local && tar -zxvf hadoop-3.0.3.tar.gz && rm -f hadoop-3.0.3.tar.gz
RUN chown -R hadoop:hadoop /usr/local/hadoop-3.0.3
RUN cd /usr/local && ln -s ./hadoop-3.0.3 hadoop
 
ENV HADOOP_PREFIX /usr/local/hadoop
ENV HADOOP_HOME /usr/local/hadoop
ENV HADOOP_COMMON_HOME /usr/local/hadoop
ENV HADOOP_HDFS_HOME /usr/local/hadoop
ENV HADOOP_MAPRED_HOME /usr/local/hadoop
ENV HADOOP_YARN_HOME /usr/local/hadoop
ENV HADOOP_CONF_DIR /usr/local/hadoop/etc/hadoop
ENV PATH ${HADOOP_HOME}/bin:${HADOOP_HOME}/sbin:$PATH

#ADD http://mirrors.tuna.tsinghua.edu.cn/apache/hbase/hbase-1.2.9/hbase-1.2.9-bin.tar.gz /usr/local/
COPY hbase-1.2.9-bin.tar.gz /usr/local/
RUN cd /usr/local && tar -zxvf hbase-1.2.9-bin.tar.gz && rm -f hbase-1.2.9-bin.tar.gz
RUN chown -R hadoop:hadoop /usr/local/hbase-1.2.9
RUN cd /usr/local && ln -s ./hbase-1.2.9 hbase

ENV HBASE_HOME /usr/local/hbase
ENV PATH ${HBASE_HOME}/bin:$PATH

RUN echo "hadoop ALL= NOPASSWD: ALL" >> /etc/sudoers

#免密ssh
USER hadoop
RUN ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
RUN cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys

ADD hadoop-env.sh $HADOOP_HOME/etc/hadoop/ 
ADD core-site.xml $HADOOP_HOME/etc/hadoop/ 
ADD hdfs-site.xml $HADOOP_HOME/etc/hadoop/ 
ADD mapred-site.xml $HADOOP_HOME/etc/hadoop/ 
ADD yarn-site.xml $HADOOP_HOME/etc/hadoop/ 

ADD hbase-env.sh $HBASE_HOME/conf/
ADD hbase-site.xml $HBASE_HOME/conf/
ADD regionservers $HBASE_HOME/conf/

USER hadoop
RUN sudo chown -R hadoop:hadoop $HADOOP_HOME/etc/hadoop
RUN sudo chown -R hadoop:hadoop $HBASE_HOME/conf
RUN sudo chown -R hadoop:hadoop /home/hadoop

