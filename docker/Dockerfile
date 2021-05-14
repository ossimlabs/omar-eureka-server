ARG BASE_IMAGE
FROM ${BASE_IMAGE}

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
