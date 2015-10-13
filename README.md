# WildFly Configuration API

This projects breaks down into three parts:

- API Generator (`generator` module)
- Generated WildFly configuration API (`api` module)
- Runtime dependencies for both the `generator` and `api` modules.

The `generator` module provides facilities to create a Java API which represents
the Wildfly management model (or parts of it).

The configuration API contains the object model created by the generator. Client
such as WildFly Swarm may use this generated API to configure a running instance
of WildFly via a fluent Java API.

The Javadocs for the generated API can be found at:
http://wildfly-swarm.github.io/wildfly-config-api/

## Generating the API

The API is created from the Wildfly management model as part of the maven build.
It requires an active Wildfly instance to access the meta data and execute the
integration tests.

1. Start a stock Wildfly 10 distribution in standalone full ha mode
`./bin/standalone.sh -server-config=standalone-full-ha.xml`
2. Update the `*-config.json` to reflect your setup
3. Perform a regular maven build `mvn clean install`

If all goes well, you be able to access the generated sources at
`api/target/generated-sources`.

## Working with the config API

### Maven dependencies

Clients typically depend on the API library itself, as well as the `runtime`
library to marshal between Java configuration instances and the JBoss DMR
`ModelNode` instances.

```
<dependency>
	<groupId>org.wildfly</groupId>
	<artifactId>api</artifactId>
	<version>0.3.0</version>
</dependency>
<dependency>
	<groupId>org.wildfly</groupId>
	<artifactId>runtime</artifactId>
	<version>0.3.0</version>
</dependency>
```

### API usage

**Java to DMR**

To generate DMR from Java simply instantiate the object model and run it through the `EntityAdapter<T>`:

```
DataSource dataSource = ...;
EntityAdapter<DataSource> entityAdapter = new EntityAdapter<>(DataSource.class);
ModelNode modelNode = entityAdapter.fromEntity(dataSource);

```

**DMR to Java**

Creating object instances work
in a similar way, by using the `EntityAdapter<T>`:

```
ModelNode modelNode = ...;
EntityAdapter<DataSource> entityAdapter = new EntityAdapter<>(DataSource.class);
DataSource dataSource = entityAdapter.fromDMR(payload);
```

The `IntegrationTestCase.java` contains more examples.


## Status and limitations

This is pretty much work in it's early stages. Use cases covered by the test cases seem to work, but we didn't test plenty of scenarios.

But the project as it is should give you an idea where the config-lib is heading and what it might be used for.


## Contributions


Feel free to create ticket right here on github, fork the code and send pull request or join us on the Wildfly IRC channels and mailing lists.
