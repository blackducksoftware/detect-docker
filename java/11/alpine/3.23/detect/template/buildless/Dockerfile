FROM alpine:3.23

# Update, upgrade, and add required packages
RUN apk --no-cache update && \
    apk --no-cache upgrade && \
    apk --no-cache add bash curl

# Java, use headless as it brings in less dependencies
RUN apk --no-cache add openjdk11-jre-headless

ENV BDS_JAVA_HOME=/usr/lib/jvm/java-11-openjdk

# Download Detect jar

ARG DETECT_SOURCE=<detectsource>

RUN if [ $(curl --silent -L -w '%{http_code}' -o /detect.jar --create-dirs ${DETECT_SOURCE}) != "200" ]; then echo "Unable to download Detect jar from ${DETECT_SOURCE}"; exit 1; fi

# Define Docker Image entrypoint
ENTRYPOINT ["java", "-jar", "/detect.jar", "--detect.source.path=/source", "--detect.output.path=/output", "--detect.phone.home.passthrough.invoked.by.image=true", "--detect.accuracy.required=NONE"]
