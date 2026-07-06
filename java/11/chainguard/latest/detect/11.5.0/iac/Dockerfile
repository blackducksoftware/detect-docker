# Use a full image so we have tools to setup
FROM artifactory.tools.duckutil.net:5002/blackducksoftware/blackduck-basejre-chainguard-dev:3.0.1-RC AS downloader

# Download Detect jar
ARG DETECT_SOURCE=https://repo.blackduck.com/bds-integrations-release/com/blackduck/integration/detect/11.5.0/detect-11.5.0.jar
RUN wget -O /tmp/detect.jar --server-response ${DETECT_SOURCE}

# For the final built image use a non-dev release so we remove various tool vulnerabilities
# from the final image.
#FROM artifactory.tools.duckutil.net:5002/blackducksoftware/blackduck-basejre-chainguard:3.0.1-RC
FROM artifactory.tools.duckutil.net:5002/blackducksoftware/blackduck-basejre-chainguard-dev:3.0.1-RC
COPY --from=downloader /tmp/detect.jar /tmp/detect.jar

# Define Docker Image entrypoint
ENTRYPOINT ["java", "-jar", "/tmp/detect.jar", "--detect.source.path=/source", "--detect.output.path=/output", "--detect.phone.home.passthrough.invoked.by.image=true"]
