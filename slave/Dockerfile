FROM ubuntu:16.04

# Install required pkg
RUN apt-get update -y && \
    apt-get install apt-transport-https ca-certificates gnupg-agent software-properties-common openjdk-8-jre\
    curl python-dev python-setuptools gcc make libssl-dev -y && \
    easy_install pip jenkins-webapi

RUN curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg > /tmp/dkey; apt-key add /tmp/dkey && \
    add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") $(lsb_release -cs) stable" && \
    apt-get update -y && \
    apt-get -y install docker-ce
 
# Get docker-compose in the agent container
RUN curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose
RUN mkdir -p /home/jenkins /var/lib/jenkins
 
# Start-up script to attach the slave to the master
ADD slave.py /var/lib/jenkins/slave.py
 
WORKDIR /home/jenkins

ENV JENKINS_URL "http://jenkins"
ENV JENKINS_SLAVE_ADDRESS ""
ENV JENKINS_USER "admin"
ENV JENKINS_PASS "admin"
ENV SLAVE_NAME ""
ENV SLAVE_SECRET ""
ENV SLAVE_EXECUTORS "1"
ENV SLAVE_LABELS "docker"
ENV SLAVE_WORING_DIR ""
ENV CLEAN_WORKING_DIR "true"
 
CMD [ "python", "-u", "/var/lib/jenkins/slave.py" ]
