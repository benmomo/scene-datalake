FROM ubuntu:24.04

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get -y update
RUN apt-get -y upgrade
RUN apt-get -y install dialog apt-utils
RUN apt-get -y update --fix-missing
RUN apt-get -y install openjdk-8-jdk wget curl

# Avoid Tomcat slow start
RUN apt-get -y install haveged
#RUN apt-get -y install tomcat8 tomcat8-admin


# Useful libraries
RUN apt-get -y install net-tools git zip unzip dos2unix nano iputils-ping

# Remove jdk-11 (probably in the upgrade process it is installed. Just keep  JDK 8 version)
RUN apt-get remove openjdk-11-jre-headless


RUN mkdir /usr/local/tomcat



COPY ./apache-tomcat-9.0.91.tar.gz /tmp
RUN cd /tmp && tar xvfz apache-tomcat-9.0.91.tar.gz
RUN cp -Rv /tmp/apache-tomcat-9.0.91/* /usr/local/tomcat/



EXPOSE 8080
CMD /usr/local/tomcat/bin/catalina.sh run

COPY ./webvowl.war /usr/local/tomcat/webapps/

