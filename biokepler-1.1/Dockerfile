FROM ubuntu:latest

MAINTAINER Daniel Crawl <danielcrawl@gmail.com>

# disable interactive functions
ENV DEBIAN_FRONTEND noninteractive

# set default java environment variable
ENV JAVA_HOME /usr/lib/jvm/java-7-oracle/

# install vnc server and R
# install software-properties-common for add-apt-repository used in
# next command
RUN apt-get update && \
    apt-get upgrade -y && \
    apt-get install -y software-properties-common curl tightvncserver xterm fluxbox r-base-core && \
    rm -rf /var/lib/apt/lists/*

# install java 7
RUN add-apt-repository ppa:webupd8team/java -y && \
    echo debconf shared/accepted-oracle-license-v1-1 select true |  debconf-set-selections && \
    echo debconf shared/accepted-oracle-license-v1-1 seen true |  debconf-set-selections && \
    apt-get update && \
    apt-get install -y --no-install-recommends oracle-java7-installer && \
    apt-get install -y --no-install-recommends oracle-java7-set-default && \
    rm -rf /var/cache/oracle-jdk7-installer && \
    rm -rf /var/lib/apt/lists/*

RUN mkdir ~/.vnc && \
    echo '' | tightvncpasswd -f > ~/.vnc/passwd && \
    chmod 600 ~/.vnc/passwd && \
    echo '#!/bin/sh' > ~/.vnc/xstartup && \
    echo 'fluxbox &' >> ~/.vnc/xstartup && \
    echo 'xterm -e /kepler/biokepler-1.1/kepler.sh &' >> ~/.vnc/xstartup && \
    chmod 755 ~/.vnc/xstartup

WORKDIR /kepler
# get bioKepler
RUN curl http://www.biokepler.org/files/downloads/biokepler-1.1.tar.gz | tar xzf -

# open vnc port
EXPOSE 5900

ENV USER root

# start vnc
CMD tightvncserver :0 -depth 16 -geometry 1800x1050 && tail -f ~/.vnc/*.log
