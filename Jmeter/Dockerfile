# credits to : rdpanek & vinsdocker

#Base image for JmeterServer
#use ubuntu 14.04

FROM ubuntu:14.04
MAINTAINER arghya


# env

# Set Jmeter Home
ENV JMETER_HOME /jmeter/apache-jmeter-2.13/

# Add Jmeter to the Path
ENV PATH $JMETER_HOME/bin:$PATH

ENV JMETER_VERSION 2.13
ENV PLUGINS_VERSION 1.2.0
ENV PLUGINS_PATH $JMETER_HOME/plugins




# Install wget & JRE
RUN apt-get clean && \
	apt-get update && \
	apt-get -qy install \
				wget \
				default-jre-headless \
				telnet \
				iputils-ping \
				unzip

# Install jmeter
RUN   mkdir /jmeter \
      && cd /jmeter/ \
      && wget https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-2.13.tgz \
      && tar -xzf apache-jmeter-2.13.tgz \
      && rm apache-jmeter-2.13.tgz
	  
# Install dependencies
# - JMeterPlugins-Standard 1.2.0
# - JMeterPlugins-Extras 1.2.0
# - JMeterPlugins-ExtrasLibs 1.2.0
RUN apt-get install -y --force-yes unzip

# Install JMeterPlugins-ExtrasLibs
RUN mkdir -p $PLUGINS_PATH && \
    wget -q http://jmeter-plugins.org/downloads/file/JMeterPlugins-ExtrasLibs-$PLUGINS_VERSION.zip && \
    unzip -o -d $PLUGINS_PATH JMeterPlugins-ExtrasLibs-$PLUGINS_VERSION.zip && \
    wget -q http://jmeter-plugins.org/downloads/file/JMeterPlugins-Extras-$PLUGINS_VERSION.zip && \
    unzip -o -d $PLUGINS_PATH JMeterPlugins-Extras-$PLUGINS_VERSION.zip && \
    wget -q http://jmeter-plugins.org/downloads/file/JMeterPlugins-Standard-$PLUGINS_VERSION.zip && \
    unzip -o -d $PLUGINS_PATH JMeterPlugins-Standard-$PLUGINS_VERSION.zip

# Copy plugins to jmeter enviroment
RUN cp $PLUGINS_PATH/lib/*.jar $JMETER_HOME/lib/
RUN cp $PLUGINS_PATH/lib/ext/*.jar $JMETER_HOME/lib/ext/	  
	  
	  



