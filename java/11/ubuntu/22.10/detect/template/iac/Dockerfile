#FROM cgr.dev/blackduck.com/adoptium-jre:adoptium-openjdk-17-dev
FROM artifactory.tools.duckutil.net:5010/blackduck.com/adoptium-jre:adoptium-openjdk-17-dev AS downloader

# Download Detect jar
ARG DETECT_SOURCE=<detectsource>
RUN wget -O /tmp/detect.jar --server-response ${DETECT_SOURCE}

# For the final built image use a non-dev release so we remove various tool vulnerabilities
# from the final image.
FROM artifactory.tools.duckutil.net:5010/blackduck.com/adoptium-jre:adoptium-openjdk-17
COPY --from=downloader /tmp/detect.jar /tmp/detect.jar

# Define Docker Image entrypoint
ENTRYPOINT ["java", "-jar", "/tmp/detect.jar", "--detect.source.path=/source", "--detect.output.path=/output", "--detect.phone.home.passthrough.invoked.by.image=true"]
