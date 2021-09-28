FROM jenkins/inbound-agent as builder
FROM maven:3.8.1-openjdk-8-slim as maven-builder
# Java Image Tag('11-jdk' or '8-jdk')
FROM openjdk:11-jdk as java-builder
# Base Image
FROM centos:centos8

# User Attributes
ARG user=jenkins
ARG group=jenkins
ARG uid=1000
ARG gid=1000
ARG MAINTAINER="CI Team"
ARG JENKINS_AGENT_VERSION="v2021.9"
# Java Version (openjdk-8 for 8-jdk or openjdk-11 for 11-jdk)
ARG JAVA_VERSION="openjdk-11"

COPY --from=builder /usr/local/bin/jenkins-slave /usr/local/bin/jenkins-agent
COPY --from=builder /usr/share/jenkins/agent.jar /usr/share/jenkins/agent.jar
COPY --from=maven-builder /usr/share/maven /usr/share/maven
COPY --from=java-builder /usr/local/${JAVA_VERSION} /usr/local/openjdk

# Labels
LABEL MAINTAINER=$MAINTAINER \
      JENKINS_AGENT_VERSION=$JENKINS_AGENT_VERSION

RUN groupadd -g ${gid} ${group} \
    && useradd -u ${uid} -g ${gid} -s /bin/bash ${user} \
    && groupadd -g 1950 docker \
    && usermod -aG docker ${user} \
    && yum install -y \
       yum-utils \
       unzip jq which \
       wget curl ca-certificates rsync \
    && yum-config-manager \
        --add-repo \
        https://download.docker.com/linux/centos/docker-ce.repo \
    && yum install -y docker-ce-cli \
    && yum clean all \
    && rm -rf /var/cache/yum \
    && curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64-2.0.30.zip" -o "awscliv2.zip" \
    && unzip awscliv2.zip \
    && ./aws/install \
    && aws --version \
    && rm -rf aws awscliv2.zip \
    && chmod +x /usr/local/bin/jenkins-agent \
    && ln -s /usr/local/bin/jenkins-agent /usr/local/bin/jenkins-slave \
    && chmod 644 /usr/share/jenkins/agent.jar \
    && ln -sf /usr/share/jenkins/agent.jar /usr/share/jenkins/slave.jar

USER ${user}
ENV JAVA_HOME /usr/local/openjdk
ENV M2_HOME /usr/share/maven
ENV MAVEN_CONFIG "/${user}/.m2"
ENV PATH="$M2_HOME/bin:${PATH}"
ENV PATH="$JAVA_HOME/bin:${PATH}"

ENTRYPOINT ["jenkins-agent"]
