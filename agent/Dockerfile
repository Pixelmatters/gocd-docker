FROM ubuntu:14.04
MAINTAINER Fabio Martins, fabio@wearepixelmatters.com

RUN apt-get update -y \
  && apt-get install -y --no-install-recommends \
  wget \
  curl \
  git \
  unzip \
  nodejs \
  openjdk-7-jre-headless \
  && rm -rf /var/lib/apt/lists/*

ENV JAVA_HOME /usr/lib/jvm/java-7-openjdk-amd64

RUN wget -O /tmp/gocd-agent.deb http://download.go.cd/gocd-deb/go-agent-15.1.0-1863.deb \
  && apt-get update -y \
  && sudo dpkg -i /tmp/gocd-agent.deb \
  && rm /tmp/gocd-agent.deb \
  && rm -rf /var/lib/apt/lists/*

RUN sed -r -i "s/^(GO_SERVER)=(.*)/\1=\$SERVER_PORT_8153_TCP_ADDR/g" /etc/default/go-agent

ADD autoregister.properties /var/lib/go-agent/config/autoregister.properties
RUN chown go /var/lib/go-agent/config/autoregister.properties

ENV DAEMON Y

#VOLUME ["/var/lib/go-agent"]

# TODO: Look at using spupervisord to keep the container running, or use something like kubernetes?
CMD touch /var/log/go-agent/go-agent.log \
  && /usr/share/go-agent/agent.sh \
  && tail -f /var/log/go-agent/go-agent.log
