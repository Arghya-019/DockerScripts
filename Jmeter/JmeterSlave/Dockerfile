FROM cloudbees/java-build-tools

USER root

ARG JENKINS_REMOTING_VERSION=3.5


#Installing Jmeter 3.0 ...:
# Set Jmeter Home
ENV JMETER_HOME /jmeter/apache-jmeter-3.0/

# Add Jmeter to the Path
ENV PATH $JMETER_HOME/bin:$PATH

ENV JMETER_VERSION 3.0
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
      && wget https://archive.apache.org/dist/jmeter/binaries/apache-jmeter-3.0.tgz \
      && tar -xzf apache-jmeter-3.0.tgz \
      && rm apache-jmeter-3.0.tgz
	  
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

#Downloading and installing docker client 
RUN curl -fsSLO https://get.docker.com/builds/Linux/x86_64/docker-17.03.0-ce.tgz && tar --strip-components=1 -xvzf docker-17.03.0-ce.tgz -C /usr/local/bin

# See https://github.com/jenkinsci/docker-slave/blob/2.62/Dockerfile#L32
RUN curl --create-dirs -sSLo /usr/share/jenkins/slave.jar https://repo.jenkins-ci.org/public/org/jenkins-ci/main/remoting/$JENKINS_REMOTING_VERSION/remoting-$JENKINS_REMOTING_VERSION.jar \
  && chmod 755 /usr/share/jenkins \
  && chmod 644 /usr/share/jenkins/slave.jar

COPY jenkins-slave /usr/local/bin/jenkins-slave

#testing 
COPY LoadTesting.jmx /home/jenkins/

RUN chmod a+rwx /home/jenkins
#making docker executable
RUN chmod +x /usr/local/bin/docker

WORKDIR /home/jenkins
USER jenkins

ENTRYPOINT ["/opt/bin/entry_point.sh", "/usr/local/bin/jenkins-slave"]
