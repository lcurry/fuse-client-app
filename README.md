# Fuse AMQP Client Application

A Spring Boot application written with Red Hat Fuse components that acts as an AMQP message producer and consumer.


## Configure for you environment 

Edit the configuration file 
```
./src/main/resources/application.properties
```
Edit the following properties in configuration:

amqp.producer.uri
amqp.consumer.uri

```
# amqp producer properties
amqp.producer.enabled=true
amqp.producer.uri=amqp://iof-amq-0-svc.iof.svc.cluster.local:61617
# Comment above line and uncoment below 
# This URL will come from the corresponding Openshift route. To view in console got to 
# Networking->Routes Select the URL under "Location" for the route with "\<your namespace\>-amq-0-svc-rte
# amqp.producer.uri=amqps://iof-amq-0-svc-rte-iof.apps.cluster-gj7vv.gj7vv.sandbox1370.opentlc.com:443?transport.verifyHost=false

amqp.producer.user=admin
amqp.producer.password=admin
amqp.producer.address=simple.amqp.test

# amqp consumer properties
amqp.consumer.enabled=true
amqp.consumer.uri=amqp://iof-amq-0-svc.iof.svc.cluster.local:61617
# Comment above line and uncoment below 
# This URL will come from the corresponding Openshift route. To view in console got to 
# Networking->Routes Select the URL under "Location" for the route with "\<your namespace\>-amq-0-svc-rte
# amqp.consumer.uri=amqps://iof-amq-0-svc-rte-iof.apps.cluster-gj7vv.gj7vv.sandbox1370.opentlc.com:443?transport.verifyHost=false


```


## Building the application

To build the application run the following from the root of this project:

`mvn clean install`

## Copy down the Keystore file 

From the Openshift console Go to the namespace where the broker is deployed. 
Look under secrets. 
Download the secret for amq broker. 
'iof-amq-secret' 
Download the file associated with 'broker.ks'
There is a link "Save File" for key 'broker.ks' in the Openshift console under secret -> Data (near the bottom)
Copy this file into your current working directory (the root of this project)


```
cp ~/Downloads/broker.ks . 
```

## Run the application 

To run the application 

```
java  -Djavax.net.ssl.trustStore=./broker.ks -Djavax.net.ssl.trustStorePassword=changeit -jar target/fuse-amq-client-1.0-SNAPSHOT.jar

```
