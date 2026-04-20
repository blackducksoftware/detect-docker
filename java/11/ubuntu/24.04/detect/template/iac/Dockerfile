#FROM cgr.dev/blackduck.com/adoptium-jre:adoptium-openjdk-17-dev
FROM artifactory.tools.duckutil.net:5010/blackduck.com/adoptium-jre:adoptium-openjdk-17-dev

# Download Detect jar

ARG DETECT_SOURCE=<detectsource>

RUN wget -O /tmp/detect.jar --server-response ${DETECT_SOURCE}

# Define Docker Image entrypoint
ENTRYPOINT ["java", "-jar", "/tmp/detect.jar", "--detect.source.path=/source", "--detect.output.path=/output", "--detect.phone.home.passthrough.invoked.by.image=true"]
