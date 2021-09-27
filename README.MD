# Amido stacks modules, examples and archetypes generator
## Folder structure

### Archetypes
Contains the generated archetypes from the examples folders

### Build
Azure Devops and Jenkins build pipelines
### Deploy
Azure and Kubernetes deploy pipelines
### Examples
Different examples build using the available stacks modules

#### POM Structure
- pom-stacks-examples.xml
  - pom-stacks-examples-simple.xml
  - pom-stacks-examples-basic.xml

#### POM Properties
Default properties
``` xml
<properties>
    <!-- Root folder for building the examples -->
    <main.basedir>./</main.basedir>
    <!-- Path to the stacks modules code -->
    <source.basedir>${main.basedir}stacks</source.basedir>
    <!-- Path to the build scripts -->
    <build.basedir>${main.basedir}build</build.basedir>
    <!-- Path to the deploy scripts -->
    <deploy.basedir>${main.basedir}deploy</deploy.basedir>
    <!-- Path to the API tests  -->
    <apiTests.basedir>${main.basedir}tests/api-tests</apiTests.basedir>
    <!-- Path to the Karate API tests -->
    <apiTestKarate.basedir>${main.basedir}tests/api-tests-karate</apiTestKarate.basedir>
    <!-- Root path to the output folder -->
    <output.basedir>./</output.basedir>
    <!-- Destination path for the Java source files (If we want to generate an archertype based on this example, this folder must not be named Java) -->
    <output.directory>./source</output.directory>
</properties>
```
``` xml 
<properties>
    <!-- Point the build to the project root folder -->
    <main.basedir>../../</main.basedir>
    <!-- Folder to store the example files -->
    <output.basedir>./stacks-api-cqrs</output.basedir>
</properties>
```
### Stacks
Modules to implement the different stacks capabilities.

The stacks capabilities have been defined as maven modules
- stacks-base
  - api
  - service
    - basic
    - cqrs
  - repository
    - cosmosdb
    
### Tests
Implementations for the API testing

## How to create a new Example
1. Add a new file pom-stacks-examples-<example-name>.xml. You can copy one of the existing files and replace the artifactId and output.basedir properties with the right values
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <artifactId>stacks-api-examples</artifactId>
        <groupId>org.example</groupId>
        <version>1.0-SNAPSHOT</version>
        <relativePath>./pom-stacks-examples.xml</relativePath>
    </parent>

    <artifactId>stacks-api-example-[example-name]</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <main.basedir>../../</main.basedir>
        <output.basedir>./[EXAMPLE_FOLDER_NAME]</output.basedir>
    </properties>

    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <artifactId>maven-resources-plugin</artifactId>
                    <version>2.6</version>

                    <executions>
                        <execution>
                            <id>copy-java-resources</id>
                            <phase>process-resources</phase>
                            <goals>
                                <goal>copy-resources</goal>
                            </goals>
                            <configuration>
                                <outputDirectory>${output.basedir}/source</outputDirectory>
                                <resources>
                                    <!-- Each resource references to a specific stack module, remove and include as needed -->
                                    <resource>
                                        <directory>${source.basedir}/api</directory>
                                        <filtering>true</filtering>
                                        <excludes>
                                            <exclude>pom.xml</exclude>
                                            <exclude>/target/*</exclude>
                                        </excludes>
                                    </resource>
                                    <resource>
                                        <directory>${source.basedir}/service/basic</directory>
                                        <filtering>true</filtering>
                                        <excludes>
                                            <exclude>pom.xml</exclude>
                                            <exclude>/target</exclude>
                                            <exclude>/target/*</exclude>
                                        </excludes>
                                    </resource>
                                    <resource>
                                        <directory>${source.basedir}/service/cqrs</directory>
                                        <filtering>true</filtering>
                                        <excludes>
                                            <exclude>pom.xml</exclude>
                                            <exclude>/target</exclude>
                                            <exclude>/target/*</exclude>
                                        </excludes>
                                    </resource>
                                    <resource>
                                        <directory>${basedir}</directory>
                                        <includes>
                                            <include>pom.xml</include>
                                        </includes>
                                        <filtering>true</filtering>
                                    </resource>
                                </resources>
                            </configuration>
                        </execution>
                    </executions>
                </plugin>

                <plugin>
                    <artifactId>maven-archetype-plugin</artifactId>
                    <version>3.2.0</version>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>

</project>
```
2. Create a new folder under the examples folder. Name has to be the same than in the output.basedir property
3. Inside the new folder copy 
   1. archetype.properties
   2. pom-archetype.xml
   
```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
   <parent>
   <artifactId><NEW_ARCHETYPE_ARTIFACT_ID></artifactId>
   <groupId>org.guillermo</groupId>
   <version>1.0.0-SNAPSHOT</version>
   <relativePath>../pom-stacks-archetypes.xml</relativePath>
   </parent>
   <artifactId>stacks-api-example-simple</artifactId>
    <version>1.0.0-SNAPSHOT</version>
    <modelVersion>4.0.0</modelVersion>

    <packaging>maven-archetype</packaging>

    <properties>
        <archetype.output.directory><NEW_ARCHETYPE_NAME></archetype.output.directory>
    </properties>

    <build>
        <extensions>
            <extension>
                <groupId>org.apache.maven.archetype</groupId>
                <artifactId>archetype-packaging</artifactId>
                <version>3.2.0</version>
            </extension>
        </extensions>

        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-archetype-plugin</artifactId>
                    <version>3.2.0</version>
                    <configuration>
                        <outputDirectory>../../archetypes/${archetype.output.directory}</outputDirectory>
                    </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
   </project>
   ```
4. From the Examples folder, run the command below to copy all required files in the example folder
``` shell
mvn -f ./pom-stacks-examples-<example-name>.xml process-resources
```

## How to create a new archetype based on an existing example
1. Navigate to the folder containing the example we want to use
2. Review that the configuration for the atchetype.properties and pom-archetype.xml files
3. Run the command
``` shell
mvn -f pom-archetype.xml archetype:create-from-project -DpropertyFile='./archetype.properties'
```
4 A new child folder in the archetype should be added to the project
