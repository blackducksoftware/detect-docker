FROM ubuntu:24.04

# Update
RUN apt-get update

# Install wget
RUN apt-get -y install wget

# Java
RUN apt-get -y install default-jre-headless
ENV BDS_JAVA_HOME=/usr/lib/jvm/default-java

# Configure locale (charset encoding)
RUN apt-get -y install locales
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# Download Detect jar

ARG DETECT_SOURCE=https://repo.blackduck.com/bds-integrations-release/com/blackduck/integration/detect/10.3.0/detect-10.3.0.jar

RUN wget -O /detect.jar --server-response ${DETECT_SOURCE}

# Define Docker Image entrypoint
ENTRYPOINT ["java", "-jar", "/detect.jar", "--detect.source.path=/source", "--detect.output.path=/output", "--detect.phone.home.passthrough.invoked.by.image=true"]
