# OMAR Eureka Server
https://github.com/ossimlabs/omar-eureka-server

## Purpose
The Eureka server provides a Spring Framework discovery client for all OMAR services. This discovery client is utilized for a variety of routing, metrics, and aggregation tasks.

## Dockerfile
```
FROM omar-base
RUN mkdir /usr/share/omar
COPY omar-eureka-server-1.0.0-SNAPSHOT.jar /usr/share/omar
RUN chown -R 1001:0 /usr/share/omar
RUN chown 1001:0 /usr/share/omar
RUN chmod -R g+rw /usr/share/omar
RUN find $HOME -type d -exec chmod g+x {} +
USER 1001
WORKDIR /usr/share/omar
VOLUME /dev/random /home /Users
EXPOSE 8761
CMD java -Xms256m -Xmx1024m -Dspring.profiles.active=production -Djava.security.egd=file:/dev/./urandom -Dserver.contextPath=/omar-eureka-server -jar /usr/share/omar/omar-eureka-server-1.0.0-SNAPSHOT.jar
```
Ref: [omar-base](../../../omar-base/docs/install-guide/omar-base/)

## JAR
[https://artifactory.ossim.io/artifactory/webapp/#/artifacts/browse/tree/General/omar-local/io/ossim/omar/apps/omar-eureka-server](https://artifactory.ossim.io/artifactory/webapp/#/artifacts/browse/tree/General/omar-local/io/ossim/omar/apps/omar-eureka-server)

## Installation in Openshift

**Assumption:** The omar-eureka-server docker image is pushed into the OpenShift server's internal docker registry and available to the project.

The OMAR Eureka Server can be deployed into OpenShift without any special volumes or secrets.

### Environment variables

|Variable|Value|
|------|------|
|SPRING_PROFILES_ACTIVE|Comma separated profile tags|

By default, no environment variables are required. If none are specified the SPRING_PROFILES_ACTIVE is empty.

### An Example DeploymentConfig

```yaml
apiVersion: v1
kind: DeploymentConfig
metadata:
  annotations:
    openshift.io/generated-by: OpenShiftNewApp
  creationTimestamp: null
  generation: 1
  labels:
    app: omar-openshift
  name: omar-eureka-server
spec:
  replicas: 1
  selector:
    app: omar-openshift
    deploymentconfig: omar-eureka-server
  strategy:
    activeDeadlineSeconds: 21600
    resources: {}
    rollingParams:
      intervalSeconds: 1
      maxSurge: 25%
      maxUnavailable: 25%
      timeoutSeconds: 600
      updatePeriodSeconds: 1
    type: Rolling
  template:
    metadata:
      annotations:
        openshift.io/generated-by: OpenShiftNewApp
      creationTimestamp: null
      labels:
        app: omar-openshift
        deploymentconfig: omar-eureka-server
    spec:
      containers:
      - env:
        - name: SPRING_PROFILES_ACTIVE
          value: production,dev
        image: 172.30.181.173:5000/o2/omar-eureka-server@sha256:ed05df0ff85eebca1b6d6f371944b17785331cb872cad74c15b8dcc9722173a9
        imagePullPolicy: Always
        livenessProbe:
          failureThreshold: 3
          initialDelaySeconds: 30
          periodSeconds: 10
          successThreshold: 1
          tcpSocket:
            port: 8761
          timeoutSeconds: 5
        name: omar-eureka-server
        ports:
        - containerPort: 8761
          protocol: TCP
        readinessProbe:
          failureThreshold: 3
          initialDelaySeconds: 15
          periodSeconds: 10
          successThreshold: 1
          tcpSocket:
            port: 8761
          timeoutSeconds: 5
        resources: {}
        terminationMessagePath: /dev/termination-log
        volumeMounts:
        - mountPath: /dev/random
          name: omar-eureka-server-2
      dnsPolicy: ClusterFirst
      restartPolicy: Always
      securityContext: {}
      terminationGracePeriodSeconds: 30
      volumes:
      - emptyDir: {}
        name: omar-eureka-server-2
  test: false
  triggers:
  - type: ConfigChange
  - imageChangeParams:
      automatic: true
      containerNames:
      - omar-eureka-server
      from:
        kind: ImageStreamTag
        name: omar-eureka-server:latest
        namespace: o2
    type: ImageChange
status:
  availableReplicas: 0
  latestVersion: 0
  observedGeneration: 0
  replicas: 0
  unavailableReplicas: 0
  updatedReplicas: 0
```
