FROM jenkins/jenkins:lts
USER root
RUN curl -SL https://github.com/docker/compose/releases/download/v2.25.0/docker-compose-linux-x86_64 \
    -o /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose
USER jenkins
