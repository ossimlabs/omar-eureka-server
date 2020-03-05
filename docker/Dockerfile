#ARG BASE_IMAGE
#FROM ${BASE_IMAGE}
#FROM adoptopenjdk/openjdk8:alpine-slim
ARG DOCKER_REGISTRY=nexus-docker-private-group.ossim.io
ARG BASE_IMAGE_TAG=release
FROM ${DOCKER_REGISTRY}/openjdk8:${BASE_IMAGE_TAG}

ENV HOME /home/omar
RUN adduser -D -h $HOME -s /sbin/nologin -u 1001 omar
COPY run.sh $HOME
RUN chown 1001:0 -R $HOME && \
    chmod 777 $HOME && \
    chmod 777 $HOME/*.sh
USER 1001

WORKDIR $HOME
EXPOSE 8761
COPY *.jar $HOME/
CMD ["/bin/sh", "/home/omar/run.sh"]
