FROM openjdk:8-jre

# Update the base image OS and install wget and python
RUN apt-get update && apt-get install -y wget python less

# Presto version will be passed in at build time
ARG PRESTO_VERSION=0.261

# Set the URL to download
ARG PRESTO_BIN=https://repo1.maven.org/maven2/com/facebook/presto/presto-server/${PRESTO_VERSION}/presto-server-${PRESTO_VERSION}.tar.gz


# Download Presto and unpack it to /opt/presto
RUN wget --quiet ${PRESTO_BIN} && \
    mkdir -p /opt && \
    tar -xf presto-server-${PRESTO_VERSION}.tar.gz -C /opt && \
    rm presto-server-${PRESTO_VERSION}.tar.gz && \
    ln -s /opt/presto-server-${PRESTO_VERSION} /opt/presto

# Download the Presto CLI and put it in the image
RUN wget --quiet https://repo1.maven.org/maven2/com/facebook/presto/presto-cli/${PRESTO_VERSION}/presto-cli-${PRESTO_VERSION}-executable.jar && \
    mv presto-cli-${PRESTO_VERSION}-executable.jar /usr/local/bin/presto && \
    chmod +x /usr/local/bin/presto

# Copy configuration files on the host into the image
COPY etc /opt/presto/etc

# Specify the entrypoint to start
ENTRYPOINT /opt/presto/bin/launcher run
EXPOSE 8080