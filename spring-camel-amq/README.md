# Java Camel Spring QuickStart

TODO: Update this document.

This quickstarts run in a Java standalone container, using Spring with Apache Camel.

This example is implemented using solely a Spring XML file (there is no custom Java code).
The source code is provided in the following XML file `src/main/resources/META-INF/spring/camel-context.xml`,
which can be viewed from [github](https://github.com/fabric8io/ipaas-quickstarts/blob/master/quickstart/java/camel-spring/src/main/resources/META-INF/spring/camel-context.xml).

This example uses a timer to trigger a message every 5th second that is routed using a content based router, based on
the message is regarded as high priority or not.


### Running the example locally

Edit `resources/application.properties` and update the broker URL in `activemq.broker.url` depending on your environment, e.g.:

    tcp://localhost:61616
    ssl://broker-amq-tcp-ssl-myproject.127.0.0.1.xip.io:443  # in OpenShift

Build and run the example using

    mvn clean compile exec:java

If you wish to connect to a broker securely using TLS, you can pass the location of your Java truststore, e.g.:

    mvn clean compile exec:java -Djavax.net.ssl.trustStore=/Users/jmills/keystores/client.ts -Djavax.net.ssl.trustStorePassword=changeit

### Running the example in your local OpenShift

If you are running OpenShift locally (e.g. through `oc cluster up`, or the CDK)), then the example can be built and deployed using a single goal:

    mvn -Pf8-local-deploy \
        -Dfabric8.env.ACTIVEMQ_BROKER_URL=ssl://broker-amq-tcp-ssl:61617

When the example runs in OpenShift, you can use the OpenShift client tool to inspect the status

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

The application can be run directly by first editing the template file and populating S2I build parameters, including the required parameter GIT_REPO and then executing the command:

    oc new-app -f quickstart-template.json

Alternatively the template file can be used to create an OpenShift application template by executing the command:

    oc create -f quickstart-template.json


### More details

You can find more details about running this [quickstart](http://fabric8.io/guide/quickstarts/running.html) on the website. This also includes instructions how to change the Docker image user and registry.

