# Copyright (c) 2012-2016 Codenvy, S.A.
# All rights reserved. This program and the accompanying materials
# are made available under the terms of the Eclipse Public License v1.0
# which accompanies this distribution, and is available at
# http://www.eclipse.org/legal/epl-v10.html
# Contributors:
# Codenvy, S.A. - initial API and implementation

FROM ubuntu
EXPOSE 4403 8000 8080 8443 9876 22
RUN apt-get update && \
    apt-get -y install sudo openssh-server procps wget unzip mc curl subversion software-properties-common python-software-properties && \
    mkdir /var/run/sshd && \
    sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd && \
    echo "%sudo ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers && \
    useradd -u 1000 -G users,sudo -d /home/user --shell /bin/bash -m user && \
    echo "secret\nsecret" | passwd user && \
    add-apt-repository ppa:git-core/ppa && \
    apt-get update && \
    sudo apt-get install git -y && \
    apt-get clean && \
    apt-get -y autoremove && \
    rm -rf /var/lib/apt/lists/*

USER user

LABEL che:server:8080:ref=tomcat7 che:server:8443:ref=tomcat7 che:server:8443:protocol=https che:server:8080:protocol=http che:server:8000:ref=tomcat7-debug che:server:8000:protocol=http che:server:9876:ref=codeserver che:server:9876:protocol=http


ENV JAVA_VERSION=8u45 \
    JAVA_VERSION_PREFIX=1.8.0_45 \
    TOMCAT_HOME=/home/user/tomcat8

ENV JAVA_HOME=/opt/jdk$JAVA_VERSION_PREFIX 

ENV PATH=$JAVA_HOME/bin:$PATH

RUN mkdir /home/user/cbuild /home/user/tomcat7 && \
  wget \
  --no-cookies \
  --no-check-certificate \
  --header "Cookie: oraclelicense=accept-securebackup-cookie" \
  -qO- \
  "http://download.oracle.com/otn-pub/java/jdk/$JAVA_VERSION-b14/jdk-$JAVA_VERSION-linux-x64.tar.gz" | sudo tar -zx -C /opt/ 
ENV TERM xterm

RUN wget -qO- "http://archive.apache.org/dist/tomcat/tomcat-7/v7.0.10/bin/apache-tomcat-7.0.10.tar.gz" | tar -zx --strip-components=1 -C /home/user/tomcat7 && \
    rm -rf /home/user/tomcat7/webapps/*


ENV LANG C.UTF-8
RUN echo "export JAVA_HOME=/opt/jdk$JAVA_VERSION_PREFIX\nexport TOMCAT_HOME=/home/user/tomcat7\nexport PATH=$JAVA_HOME/bin:$PATH" >> /home/user/.bashrc

WORKDIR /projects

CMD sudo /usr/sbin/sshd -D && \
    tail -f /dev/null
