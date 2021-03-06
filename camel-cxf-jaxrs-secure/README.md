Camel Secure CXF JAX-RS Example
-------------------------------

This example demonstrates using the camel-cxf component with the WildFly Camel Subsystem to produce and consume JAX-RS
REST services secured by an Elytron Security Domain. Elytron is a new security framework available since WildFly 10.

In this example, a Camel route takes a message payload from a direct endpoint and passes it on to a CXF producer
endpoint. The producer uses the payload to pass arguments to a CXF JAX-RS REST service secured by BASIC HTTP
authentication.

Prerequisites
-------------

* Maven
* An application server with the wildfly-camel subsystem installed

Running the example
-------------------

To run the example.

1. Use the add-user script to create a new server application user and group

    For Linux:

    ${JBOSS_HOME}/bin/add-user.sh -a -u testRsUser -p testRsPassword1+ -g testRsRole

    For Windows:

    %JBOSS_HOME%\bin\add-user.bat -a -u testRsUser -p testRsPassword1+ -g testRsRole

2. Start the application server in standalone mode:

    For Linux:

    ${JBOSS_HOME}/bin/standalone.sh -c standalone-full.xml

    For Windows:

    %JBOSS_HOME%\bin\standalone.bat -c standalone-full.xml

3. Set the Security Domain using a JBoss CLI script.

    For Linux:

    ${JBOSS_HOME}/bin/jboss-cli.sh --connect --file=configure-basic-security-rs.cli

    For Windows:

    %JBOSS_HOME%\bin\jboss-cli.bat --connect --file=configure-basic-security-rs.cli

4. Study `jboss-web-xml` and `web.xml` files in `webapp/WEB-INF` directory of this project. They
set the application security domain, security roles and constraints.

5. Build and deploy the project `mvn install -Pdeploy`

6. Browse to http://localhost:8080/example-camel-cxf-jaxrs-secure

From the 'Send A Greeting' web form, enter a 'name' into the text field and press the 'send' button. You'll then
see a simple greeting message displayed on the UI.

So what just happened there?

`CamelCxfRsServlet` handles the POST request from the web UI. It retrieves the name form parameter value and constructs an
object array. This object array will be the message payload that is sent to the `direct:start` endpoint. A `ProducerTemplate`
sends the message payload to Camel. `The direct:start` endpoint passes the object array to a `cxfrs:bean` REST service producer.
The REST service response is used by `CamelCxfRsServlet` to display the greeting on the web UI.

The full Camel route can be seen in `src/main/webapp/WEB-INF/cxfrs-camel-context.xml`.

## Undeploy

To undeploy the example run `mvn clean -Pdeploy`.

## Learn more

Additional camel-cxf documentation can be found at the [WildFly Camel User Guide](http://wildfly-extras.github.io/wildfly-camel/#_jax_rs
) site.
