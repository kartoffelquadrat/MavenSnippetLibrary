# Artifact Build

Often you want to run a software without first cloning or building the sources. In fact, almost any time you download a software from the internet you receive a precompiled release rather than source code.  
It is actually not so trivial to obtain such a standalone executable that comes a single file, with all its dependencies included.

Maven comes with powerful mechanisms to build your project to a wholesome *Java Archive* (JAR) file. In this case, all dependencies are included - however the customer still needs a Java Runtime Environment (JRE).

 > Note: It is also possible to include the JRE dependencies themselves. However, for simplicity this is not covered in this guide. Additional info on true platform specific [standalones e.g. Mac can be found here](https://simonkollross.de/posts/packaging-a-java-app-for-mac-with-maven).

## Creating a JAR with all dependencies

Once more this is a goal best achieved by help of targeted plugins. Similar to the [*Direct Run*](run) module we needed to provide a pointer to the desired launcher class, we need to tell the *jar-creating plugin* about the entry point to our code.

Below are three snippets for **Vanilla**, **Spring Boot** and **JavaFX** projects. There are many more plugins for further frameworks, but  this selection should give you a good illustration of the procedure.

 * Add the desired plugin to your ```pom.xml```
 * Then compile / build a self contained jar with: ```mvn clean package```
 * Finally run the produced jar: ```java -jar target/whatever.jar```

=== "Vanilla / No Frameworks"
     ```xml
            <plugin>
                <artifactId>maven-assembly-plugin</artifactId>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>single</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <archive>
                        <manifest>
                            <addClasspath>true</addClasspath>
                            <mainClass>eu.kartoffelquadrat.zoo.Launcher</mainClass>
                        </manifest>
                    </archive>
                    <descriptorRefs>
                        <descriptorRef>jar-with-dependencies</descriptorRef>
                    </descriptorRefs>
                    <finalName>AcquireBanker</finalName>
                    <appendAssemblyId>false</appendAssemblyId>
                </configuration>
            </plugin>
     ```

=== "Spring Boot"
     ```xml
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>
                    <mainClass>eu.kartoffelquadrat.zoo.RestLauncher</mainClass>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
     ```

=== "Java FX"
     ```xml
	  <plugin>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.4.0</version>
                <executions>
                    <execution>
                        <phase>package</phase>
                        <goals>
                            <goal>shade</goal>
                        </goals>
                        <configuration>
                            <transformers>
                                <transformer
                                        implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
                                    <mainClass>eu.kartoffelquadrat.javafxhelloworld.SuperLauncher
                                    </mainClass>
                                </transformer>
                            </transformers>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
     ```
