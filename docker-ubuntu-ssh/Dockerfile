############################################
# version : ubuntu-ssh
# desc : ubuntu14.04 �ϰ�װ��ssh
############################################
# ���ü̳���ubuntu14.04�ٷ�����
FROM ubuntu:14.04
 
# ������һЩ�����ߵĻ�����Ϣ
MAINTAINER zhaominghui<342015908@qq.com>
 
RUN rm -vf /var/lib/apt/lists/*
RUN apt-get update
RUN apt-get install -y openssh-server openssh-client vim wget curl sudo
 
#�����û� worker����������Ϊ worker, ����sudoȨ��
RUN useradd  -m worker
RUN echo "worker:worker" | chpasswd
RUN cd /etc/sudoers.d && touch nopasswdsudo && echo "worker ALL=(ALL) ALL" >> nopasswdsudo
#��worker�û���shell�ĳ�bash������ssh��¼ubuntu�������������в���ʾ�û�����Ŀ¼
RUN usermod -s /bin/bash worker
 
RUN echo "root:root" | chpasswd
 
#����ssh
RUN mkdir /var/run/sshd
 
RUN sed -i 's/PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile
 
EXPOSE 22
 
USER worker
RUN ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
RUN cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
 
USER root
RUN ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
RUN cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys