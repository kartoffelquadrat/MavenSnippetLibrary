# Artifact Build

Often you want to run a software without first cloning the sources. In fact, when you think about it, that happens almost any time you download a software from the internet. Rarely you first want to bother with compiling the software or downloading all it's dependencies.  
This is why software is commonly shipped as a bundled *release*, containing all dependencies.

Maven supports a build to a wholesome *Java Archive* (JAR) file. All dependencies are then included, however the customer still needs a Java Runtime Environment (JRE).

## Creating a JAR with all dependencies

One more this is achieved by help of plugins. Similar to the [*Direct Run*](run) module you need to provide a reference to the launcher class, so the JAR contains a meta file with instructions on where to find the "entry point".

Below are snippets for **Vanilla**, **Spring Boot** and **JavaFX** projects. There are many more, but this selection should give you a good illustration of the procedure.

In any case the procedure is the same:

 * First compile / build a self contained jar with: ```mvn clean package```
 * Then run the produced jar: ```java -jar target/whatever.jar```

=== "Vanilla / No Frameworks"
     ```
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
                            <mainClass>eu.kartoffelquadrat.zoo.Launcher/mainClass>
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
     ```
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
     ```
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
