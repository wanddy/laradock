FROM ubuntu:latest

ARG JDK_VERSION=8
ARG DATAGEAR_VERSION=1.6.1

ENV JDK_VERSION ${JDK_VERSION}
ENV DATAGEAR_VERSION ${DATAGEAR_VERSION}

LABEL maintainer="geek.snail@qq.com"

# Install "software-properties-common" (for the "add-apt-repository")
RUN apt-get update && apt-get install -y \
    software-properties-common

# Add the "JAVA" ppa
RUN add-apt-repository -y \
    ppa:webupd8team/java

# Install OpenJDK-8
RUN apt-get update && \
    apt-get install -y openjdk-8-jdk && \
    apt-get install -y ant && \
    apt-get clean

# Fix certificate issues
RUN apt-get update && \
    apt-get install ca-certificates-java && \
    apt-get clean && \
    update-ca-certificates -f

# Setup JAVA_HOME -- useful for docker commandline
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
RUN export JAVA_HOME


# Install maven
RUN apt-get update \
    &&   apt-get install -y git maven \
    &&   apt-get clean \
    && rm -rf /var/lib/apt/lists/*  \
    && rm -rf /var/www/datagear \
    && git clone https://gitee.com/datagear/datagear.git /var/www/datagear \
    && cd /var/www/datagear \
    && git fetch --all --tags \
    && git checkout tags/v$DATAGEAR_VERSION -b v$DATAGEAR_VERSION \
    && mvn clean package -DskipTests 

WORKDIR /var/www/datagear/datagear-webappembd/target/datagear-$DATAGEAR_VERSION/
RUN rm -rf start.sh
COPY start.sh ./
RUN chmod +x start.sh
ENTRYPOINT ["sh", "start.sh"]
     
EXPOSE 50401

