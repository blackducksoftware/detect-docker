FROM artifactory.tools.duckutil.net:5010/blackduck.com/chainguard-base:latest

# Update, upgrade, and add required packages
RUN apk --no-cache update && \
    apk --no-cache upgrade && \
    apk --no-cache add bash curl

# Required for standalone nuget inspector
# Note: gcompat is not needed - Wolfi uses glibc natively (Alpine uses musl, which needed gcompat)
RUN apk --no-cache add libstdc++ icu

# Java, use headless as it brings in less dependencies
RUN apk --no-cache add openjdk-11-jre

ENV BDS_JAVA_HOME=/usr/lib/jvm/java-11-openjdk
ENV PATH="${BDS_JAVA_HOME}/bin:${PATH}"

ARG DETECT_SOURCE=https://repo.blackduck.com/bds-integrations-release/com/blackduck/integration/detect/11.5.0/detect-11.5.0.jar

RUN if [ $(curl --silent -L -w '%{http_code}' -o /detect.jar --create-dirs ${DETECT_SOURCE}) != "200" ]; then echo "Unable to download Detect jar from ${DETECT_SOURCE}"; exit 1; fi

ENTRYPOINT ["java", "-jar", "/detect.jar", "--detect.source.path=/source", "--detect.output.path=/output", "--detect.phone.home.passthrough.invoked.by.image=true"]
