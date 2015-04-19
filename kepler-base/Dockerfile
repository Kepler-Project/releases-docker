FROM ubuntu:latest

MAINTAINER Daniel Crawl <danielcrawl@gmail.com>

# install vnc server and R
# install software-properties-common for add-apt-repository used in
# next command
RUN apt-get update && \
    apt-get install -y \
        curl \
        fluxbox \
        r-base-core \
        software-properties-common \
        tightvncserver \
        xterm && \
    rm -rf /var/lib/apt/lists/*

# disable interactive functions
ENV DEBIAN_FRONTEND noninteractive

# set default java environment variable
ENV JAVA_HOME /usr/lib/jvm/java-7-oracle/

# install java 7
RUN add-apt-repository ppa:webupd8team/java -y && \
    echo debconf shared/accepted-oracle-license-v1-1 select true |  debconf-set-selections && \
    echo debconf shared/accepted-oracle-license-v1-1 seen true |  debconf-set-selections && \
    apt-get update && \
    apt-get install -y --no-install-recommends oracle-java7-installer && \
    apt-get install -y --no-install-recommends oracle-java7-set-default && \
    rm -rf /var/cache/oracle-jdk7-installer && \
    rm -rf /var/lib/apt/lists/*

# add kepler user
RUN useradd -d /kepler -s /bin/bash -m kepler

USER kepler
WORKDIR /kepler
ENV USER kepler
# tightvncserver needs $HOME
ENV HOME /kepler

RUN mkdir .vnc && \
    echo '' | tightvncpasswd -f > .vnc/passwd && \
    chmod 600 .vnc/passwd && \
    echo '#!/bin/sh' > .vnc/xstartup && \
    echo 'fluxbox &' >> .vnc/xstartup && \
    chmod 755 .vnc/xstartup

# open vnc port
EXPOSE 5900

# start vnc
CMD tightvncserver :0 -depth 16 -geometry 1800x1050 && tail -f ~/.vnc/*.log
