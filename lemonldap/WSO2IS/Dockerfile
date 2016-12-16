# Dockerfile for WSO2IS

# Start from Debian Jessie
FROM isim/docker-wso2is
MAINTAINER Tanguy Le Berre

# Update system
RUN apt-get -y update && apt-get -y dist-upgrade

#  Additionnal packages
RUN apt-get -y install vim ack-grep