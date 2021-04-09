FROM jenkins:2.60.3
USER root
ARG DOCKER_CLIENT=docker-19.03.3.tgz
RUN cd /tmp/ \
    && apt-get update \
    && apt-get install -y apt-transport-https \
    && apt-get install -y sudo \
    && curl -sSL -O https://download.docker.com/linux/static/stable/x86_64/${DOCKER_CLIENT} \
    && tar zxf ${DOCKER_CLIENT} \
    && mkdir -p /usr/local/bin \
    && mv ./docker/docker /usr/local/bin \
    && chmod +x /usr/local/bin/docker \
    && rm -rf /tmp/*
RUN echo "jenkins ALL=NOPASSWD: ALL" >> /etc/sudoers
RUN curl -L https://github.com/docker/compose/releases/download/1.29.0/docker-compose-Linux-x86_64 > /usr/local/bin/docker-compose; \
    chmod +x /usr/local/bin/docker-compose
USER jenkins
COPY plugins.txt /usr/share/jenkins/plugins.txt
RUN /usr/local/bin/plugins.sh /usr/share/jenkins/plugins.txt
USER root
RUN sed -i -e 's/jdk.certpath.disabledAlgorithms=MD2, RSA keySize < 1024/jdk.certpath.disabledAlgorithms=MD2, RSA keySize < 512/' /docker-java-home/jre/lib/security/java.security
USER jenkins
