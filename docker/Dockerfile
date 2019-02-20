ARG BASE_IMAGE
FROM ${BASE_IMAGE}
WORKDIR $HOME
EXPOSE 8761
COPY *.jar $HOME/
USER root
RUN chmod 755 /home/omar && chmod 644 /home/omar/*.jar
USER 1001
CMD $HOME/run.sh