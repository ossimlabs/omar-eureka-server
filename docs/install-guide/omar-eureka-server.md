# OMAR Eureka Server

## Purpose
The Eureka server provides a Spring Framework discovery client for all OMAR services. This discovery client is utilized for a variety of routing, metrics, and aggregation tasks.

## Installation in Openshift

**Assumption:** The omar-eureka-server docker image is pushed into the OpenShift server's internal docker registry and available to the project.

The OMAR Eureka Server can be deployed into OpenShift without any special volumes or secrets.

### Environment variables

|Variable|Value|
|------|------|
|SPRING_PROFILES_ACTIVE|Comma separated profile tags|

By default, no environment variables are required. If none are specified the SPRING_PROFILES_ACTIVE is empty.
