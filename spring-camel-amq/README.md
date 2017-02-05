# spring-camel-amq

This demo runs in a Java standalone container, using Spring with Apache Camel.

This example is implemented using solely a Spring XML file (there is no custom Java code).
The source code is provided in the following XML file `src/main/resources/META-INF/spring/camel-context.xml`.

This example uses a timer to trigger the publishing of a message to an ActiveMQ broker over the SSL protocol.

This demo is based on the [spring-camel](https://github.com/fabric8-quickstarts/spring-camel) quickstart.

### Running the example locally

Edit `resources/application.properties` and update the broker URL in `activemq.broker.url` depending on your environment, e.g.:

    tcp://localhost:61616
    ssl://broker-amq-tcp-ssl-myproject.127.0.0.1.xip.io:443  # in OpenShift

Build and run the example using

    mvn clean compile exec:java

If you wish to connect to a broker securely using TLS, you can pass the location of your Java truststore, e.g.:

    mvn clean compile exec:java -Djavax.net.ssl.trustStore=/Users/jmills/keystores/client.ts -Djavax.net.ssl.trustStorePassword=changeit

### Running the example in your local OpenShift cluster

If you are running OpenShift locally (e.g. through `oc cluster up`, or the CDK), then the example can be built and deployed using a single goal:

    mvn -Pf8-local-deploy \
        -Dfabric8.env.ACTIVEMQ_BROKER_URL=ssl://broker-amq-tcp-ssl:61617

When the example runs in OpenShift, you can use the OpenShift client tool to inspect the status.

To list all the running pods:

    oc get pods

Then find the name of the pod that runs this quickstart, and output the logs from the running pods with:

    oc logs <name of pod>

You can also use the OpenShift [web console](https://docs.openshift.com/enterprise/3.1/getting_started/developers/developers_console.html#tutorial-video) to manage the
running pods, and view logs and much more.

When you want to terminate, you can use the following, which will delete the ReplicationController and stop the pod:

    oc delete rc spring-camel-amq

### Running the example using OpenShift S2I template

The example can also be built and run using the included S2I template quickstart-template.json.

Before you start, make sure you have installed the Fuse Integration Services image streams:

    oc create  -f https://raw.githubusercontent.com/jboss-fuse/application-templates/master/fis-image-streams.json -n openshift

Use the template file to create an OpenShift application template, by executing the command:

    oc create -f quickstart-template.json

The template expects to find a secret named `my-secret`, which is where the truststore should be located to enable your client to accept the broker's SSL certificate. To create the secret (where `client.ts` is the filename of your truststore):

    oc secrets new my-secret <client.ts>

Then create the new app from the console, optionally setting:

- `MAVEN_MIRROR_URL` - You can use this parameter in the template to set a Maven mirror URL where Maven should look for artifacts.
- `JAVA_OPTIONS` - if connecting to a broker over SSL, you will need to set the path to your truststore (mounted in the secret) - e.g. `-Djavax.net.ssl.trustStore=/etc/secret-volume/client.ts -Djavax.net.ssl.trustStorePassword=changeit`
- `ACTIVEMQ_BROKER_URL` - Set this to the connection URL for your broker, e.g. `failover://ssl://yourbroker-amq-tcp-ssl.yourproject.svc.cluster.local:61617`
- `ACTIVEMQ_BROKER_USERNAME` and `ACTIVEMQ_BROKER_PASSWORD` - set these for broker authentication.
