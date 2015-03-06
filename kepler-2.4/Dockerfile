
FROM dockerfile/java:oracle-java6

MAINTAINER Daniel Crawl <danielcrawl@gmail.com>

RUN apt-get update && \
    apt-get install -y tightvncserver xterm fluxbox

RUN mkdir ~/.vnc && \
    echo '' | tightvncpasswd -f > ~/.vnc/passwd && \
    chmod 600 ~/.vnc/passwd && \
    echo '#!/bin/sh' > ~/.vnc/xstartup && \
    echo 'fluxbox &' >> ~/.vnc/xstartup && \
    echo 'xterm -e /kepler/kepler-2.4/kepler.sh &' >> ~/.vnc/xstartup && \
    chmod 755 ~/.vnc/xstartup

WORKDIR /kepler
RUN curl https://code.kepler-project.org/code/kepler/releases/installers/2.4/kepler-2.4-linux.tar.gz | tar xzf -

EXPOSE 5900
ENV USER root
CMD tightvncserver :0 -depth 16 -geometry 1800x1050 && tail -f ~/.vnc/*.log
