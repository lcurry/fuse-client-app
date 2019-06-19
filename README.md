# Multi-Datacenter Reference Architecture :: Fuse AMQP Client Application

A Spring Boot application written with Red Hat Fuse components that acts as an AMQP message producer and consumer.

## Building the application

Build the application:

`mvn clean install`

## Creating a container image with s2i

Get the latest Fuse image streams:

`oc apply -f https://raw.githubusercontent.com/jboss-fuse/application-templates/application-templates-2.1.fuse-730065-redhat-00002/fis-image-streams.json -n openshift`

Create a binary s2i build configuration:

`oc process -f openshift/build.yml -o yaml | oc apply -f -`

Source application artifact and run image build:

`oc start-build fuse-client-app --from-file=target/fuse-client-app-1.0-SNAPSHOT.jar --follow`

## Deploying the application image

Apply the application template:

`oc process -f openshift/application.yml -o yaml | oc apply -f -`

Rollout the latest deployment

`oc rollout latest dc/fuse-client-app`

Follow the progress of the deployment

`oc rollout status dc/fuse-client-app --watch`

## Integration Testing

The example includes a [fabric8 arquillian](https://github.com/fabric8io/fabric8/tree/v2.2.170.redhat/components/fabric8-arquillian) OpenShift Integration Test. 
Once the container image has been built and deployed in OpenShift, the integration test can be run with:

`mvn test -Dtest=*KT`

The test is disabled by default and has to be enabled using `-Dtest`. 