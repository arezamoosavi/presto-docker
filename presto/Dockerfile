FROM openjdk:8-jre

RUN apt-get update -y && apt-get install -y libzbar-dev bash gcc git libc-dev curl wget vim nano \
    && apt-get install -y iputils-ping telnet openssh-client net-tools \
    man unzip vim-tiny bc openssh-server thrift-compiler netcat sudo \
    && apt-get install -y build-essential \
    && apt-get autoremove -y \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

#install python3.7
RUN apt-get update && apt-get install -y software-properties-common && \
    apt-add-repository -r ppa:armagetronad-dev/ppa && \
    apt-get update && apt-get install -y python3.7 python3-pip \
    python3-distutils python3-setuptools && \
    python3.7 -m pip install pip --upgrade pip && \
    echo "alias python=python3.7" >> ~/.bashrc && \
    export PATH=${PATH}:/usr/bin/python3.7 && \
    /bin/bash -c "source ~/.bashrc" && \
    update-alternatives --install /usr/bin/python python /usr/bin/python3.7 1 && \
    update-alternatives --set python /usr/bin/python3.7


# Download Presto package
ARG PRESTO_VERSION=0.218
ENV PRESTO_PKG presto-server-$PRESTO_VERSION.tar.gz
ENV PRESTO_PKG_URL https://repo1.maven.org/maven2/com/facebook/presto/presto-server/$PRESTO_VERSION/$PRESTO_PKG
ENV PRESTO_CLI_JAR_URL https://repo1.maven.org/maven2/com/facebook/presto/presto-cli/$PRESTO_VERSION/presto-cli-$PRESTO_VERSION-executable.jar
# Use curl rather ADD <remote> to leverage RUN caching
# Let curl show progress bar to prevent Travis from thinking the job is stalled
RUN curl -o /$PRESTO_PKG $PRESTO_PKG_URL
RUN tar -zxf /$PRESTO_PKG

# Create directory for Presto data
RUN mkdir -p /var/lib/presto/data

# Add Presto configuration
WORKDIR /presto-server-$PRESTO_VERSION

RUN mkdir -p etc
ADD ./presto/etc etc/

# Download Presto CLI
RUN mkdir -p bin
RUN curl -o bin/presto-cli $PRESTO_CLI_JAR_URL
RUN chmod +x bin/presto-cli

CMD bin/launcher.py run
EXPOSE 8080