# Health check flow
## How to package
1. open the command line
1. navigate to the project directory
1. run the commane 'mvn package'
1. a package will be created at target/maven-health-check-1.0.0-SNAPSHOT-flows.zip

### How to import into JFrog Artifactory
1. Open JFrog artifactory
1. Click deploy
1. Select a repository
1. Upload the maven-health-check-1.0.0-SNAPSHOT-flows.zip file
1. Check deploy as maven artifact
1. Enter au.gov.csc as the Group ID
1. Enter maven-health-check as the Artifact ID
1. Enter 1.0.0 as the Version
1. Enter zip as the type
1. Check generate default pom

## How to use
### Add the following to your .pom file
```
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-dependency-plugin</artifactId>
	<version>2.10</version>
	<executions>
		<execution>
			<id>unpack</id>
			<phase>initialize</phase>
			<goals>
				<goal>unpack</goal>
			</goals>
			<configuration>
				<artifactItems>
					<artifactItem>
						<groupId>au.gov.csc</groupId>
					    <artifactId>maven-health-check</artifactId>
					    <version>1.0.0</version>
					    <type>zip</type>
						<overWrite>true</overWrite>
						<outputDirectory>${basedir}/src/main/app</outputDirectory>
					</artifactItem>
				</artifactItems>
      			</configuration>
    			</execution>
	</executions>
</plugin>
```

### Add the following to your properties file within the mule application
```
app.id=maven-health-check

http.listener.ref=HTTP_Listener_Configuration

health.check.path=/health-check

health.check.generic.properties=system-a,system-b

health.check.generic.system-a.type=HTTP
health.check.generic.system-a.flow.name=global:/success-flow

health.check.generic.system-b.type=Oracle Database
health.check.generic.system-b.flow.name=global:/error-flow
```
