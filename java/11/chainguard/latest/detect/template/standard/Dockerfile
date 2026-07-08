#FROM artifactory.tools.duckutil.net:5002/blackducksoftware/blackduck-basejre-chainguard-dev:3.0.1
#
## Update, upgrade, and add required packages
#RUN apk --no-cache update && \
#    apk --no-cache upgrade && \
#    apk --no-cache add bash
#
## Required for standalone nuget inspector
## Note: gcompat is not needed - Wolfi uses glibc natively (Alpine uses musl, which needed gcompat)
#RUN apk --no-cache add libstdc++ icu
#
## Java, use headless as it brings in less dependencies
#RUN apk --no-cache add openjdk-11-jre
#
#ENV BDS_JAVA_HOME=/usr/lib/jvm/java-11-openjdk
#ENV PATH="${BDS_JAVA_HOME}/bin:${PATH}"
#
#ARG DETECT_SOURCE=<detectsource>
#
#RUN wget -O /detect.jar --server-response ${DETECT_SOURCE}
#
#ENTRYPOINT ["java", "-jar", "/detect.jar", "--detect.source.path=/source", "--detect.output.path=/output", "--detect.phone.home.passthrough.invoked.by.image=true"]

# --- Multi Stage build to reduce image size ---

# ============================================================
# Stage 1: Builder (dev image with shell, apk, wget)
# ============================================================
FROM artifactory.tools.duckutil.net:5002/blackducksoftware/blackduck-basejre-chainguard-dev:3.0.1 AS builder

# Update, upgrade, and add required packages
RUN apk --no-cache update && \
    apk --no-cache upgrade && \
    apk --no-cache add bash

# Required for standalone nuget inspector
RUN apk --no-cache add libstdc++ icu

# Java
RUN apk --no-cache add openjdk-11-jre

ARG DETECT_SOURCE=<detectsource>

RUN wget -O /detect.jar --server-response ${DETECT_SOURCE}

# Prepare a clean /runtime directory with only what we need
RUN mkdir -p /runtime/usr/bin /runtime/bin /runtime/usr/lib /runtime/lib && \
    cp /usr/bin/bash /runtime/usr/bin/bash && \
    ln -s /usr/bin/bash /runtime/bin/sh && \
    ln -s /usr/bin/bash /runtime/bin/bash && \
    cp -r /usr/lib/jvm /runtime/usr/lib/jvm && \
    cp /usr/lib/libstdc++* /runtime/usr/lib/ && \
    cp /usr/lib/libicu* /runtime/usr/lib/ && \
    cp /usr/lib/libreadline* /runtime/usr/lib/ && \
    cp /usr/lib/libncurses* /runtime/usr/lib/ && \
    cp /usr/lib/libgcc_s* /runtime/usr/lib/ && \
    cp /detect.jar /runtime/detect.jar

# ============================================================
# Stage 2: Final (non-dev image, no busybox, no apk)
# ============================================================
FROM artifactory.tools.duckutil.net:5002/blackducksoftware/blackduck-basejre-chainguard:3.0.1

# Copy runtime files from builder
COPY --from=builder /runtime/usr/bin/bash /usr/bin/bash
COPY --from=builder /runtime/bin/sh /bin/sh
COPY --from=builder /runtime/bin/bash /bin/bash
COPY --from=builder /runtime/usr/lib/jvm /usr/lib/jvm
COPY --from=builder /runtime/usr/lib/libstdc++* /usr/lib/
COPY --from=builder /runtime/usr/lib/libicu* /usr/lib/
COPY --from=builder /runtime/usr/lib/libreadline* /usr/lib/
COPY --from=builder /runtime/usr/lib/libncurses* /usr/lib/
COPY --from=builder /runtime/usr/lib/libgcc_s* /usr/lib/
COPY --from=builder /runtime/detect.jar /detect.jar

ENV BDS_JAVA_HOME=/usr/lib/jvm/java-11-openjdk
ENV PATH="${BDS_JAVA_HOME}/bin:${PATH}"

ENTRYPOINT ["java", "-jar", "/detect.jar", "--detect.source.path=/source", "--detect.output.path=/output", "--detect.phone.home.passthrough.invoked.by.image=true"]
