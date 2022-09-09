FROM alpine:latest

MAINTAINER Raja Beemi <beemi.raja@gmail.com>

# setup jmeter version to use
# Arguments
ARG JMETER_DOWNLOAD_HOST="https://archive.apache.org"
ARG JMETER_PLUGINS_DOWNLOAD_URL="https://repo1.maven.org/maven2/kg/apc"
ARG JMETER_PLUGINS_DOWNLOAD_URL_2="https://repo1.maven.org/maven2/com"
ARG JMETER_VERSION="5.5"
ARG JMETER_PLUGINS_MANAGER_VERSION="1.4"
ARG JMETER_PLUGIN_CASUTG="2.9"
ARG JMETER_PLUGIN_CMN="0.6"
ARG JMETER_PLUGIN_TST="2.5"
ARG JMETER_PLUGIN_RANDOM_CSV="0.6"
ARG JMETER_PLUGIN_PROMETHEUS="0.6.0"
ARG JMETER_PLUGIN_AUTO_STOP="0.1"

# Environment
ENV JMETER_HOME /jmeter/apache-jmeter-${JMETER_VERSION}
ENV JMETER_BIN ${JMETER_HOME}/bin
ENV JMETER_PLUGINS_LIB_FOLDER ${JMETER_HOME}/lib
ENV JMETER_PLUGINS_EXT_FOLDER ${JMETER_PLUGINS_LIB_FOLDER}/ext/

# Add Jmeter to the Path
ENV PATH $JMETER_BIN:$PATH
# JMeter ports
ENV JMETER_MASTER_PORT=60000
ENV JMETER_WORKER_RMI_LOCAL_PORT=50000
ENV JMETER_WORKER_SERVER_PORT=1099

EXPOSE ${JMETER_MASTER_PORT} ${JMETER_WORKER_RMI_LOCAL_PORT} ${JMETER_WORKER_SERVER_PORT}

# Install some library's
RUN apk update \
    && apk add --no-cache wget \
    && apk add --no-cache zip \
    && apk add --no-cache openjdk8

# Install Everything.
RUN echo 'Installing JMETER'
RUN mkdir /jmeter \
    && cd /jmeter/ \
    && wget ${JMETER_DOWNLOAD_HOST}/dist/jmeter/binaries/apache-jmeter-${JMETER_VERSION}.tgz \
    && tar -xvzf apache-jmeter-${JMETER_VERSION}.tgz \
    && rm -rf apache-jmeter-${JMETER_VERSION}.tgz

# Install jmeter lib and dependency jar
RUN echo 'Installing JMETER Plugins'
RUN wget -P ${JMETER_PLUGINS_EXT_FOLDER}  ${JMETER_PLUGINS_DOWNLOAD_URL}/jmeter-plugins-manager/${JMETER_PLUGINS_MANAGER_VERSION}/jmeter-plugins-manager-${JMETER_PLUGINS_MANAGER_VERSION}.jar \
    && wget -P ${JMETER_PLUGINS_EXT_FOLDER}  ${JMETER_PLUGINS_DOWNLOAD_URL}/jmeter-plugins-casutg/${JMETER_PLUGIN_CASUTG}/jmeter-plugins-casutg-${JMETER_PLUGIN_CASUTG}.jar \
    && wget -P ${JMETER_PLUGINS_LIB_FOLDER}  ${JMETER_PLUGINS_DOWNLOAD_URL}/jmeter-plugins-cmn-jmeter/${JMETER_PLUGIN_CMN}/jmeter-plugins-cmn-jmeter-${JMETER_PLUGIN_CMN}.jar \
    && wget -P ${JMETER_PLUGINS_EXT_FOLDER}  ${JMETER_PLUGINS_DOWNLOAD_URL}/jmeter-plugins-tst/${JMETER_PLUGIN_TST}/jmeter-plugins-tst-${JMETER_PLUGIN_TST}.jar \
    && wget -P ${JMETER_PLUGINS_EXT_FOLDER} ${JMETER_PLUGINS_DOWNLOAD_URL_2}/github/johrstrom/jmeter-prometheus-plugin/${JMETER_PLUGIN_PROMETHEUS}/jmeter-prometheus-plugin-${JMETER_PLUGIN_PROMETHEUS}.jar \
    && wget -P ${JMETER_PLUGINS_LIB_FOLDER} ${JMETER_PLUGINS_DOWNLOAD_URL_2}/blazemeter/jmeter-plugins-random-csv-data-set/${JMETER_PLUGIN_RANDOM_CSV}/jmeter-plugins-random-csv-data-set-${JMETER_PLUGIN_RANDOM_CSV}.jar \
    && wget -P ${JMETER_PLUGINS_EXT_FOLDER} ${JMETER_PLUGINS_DOWNLOAD_URL}/jmeter-plugins-autostop/${JMETER_PLUGIN_AUTO_STOP}/jmeter-plugins-autostop-${JMETER_PLUGIN_AUTO_STOP}.jar

# Copy all JMeter property files to JMETER_BIN
ADD properties/*.properties ${JMETER_BIN}/

WORKDIR $JMETER_BIN

