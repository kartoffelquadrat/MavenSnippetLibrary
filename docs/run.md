# Run

The whole point of Maven is that you can build your software reliably, no matter the environment - this also includes settings where you have no graphical access, and maybe not even an IDE.  
The default way to compile is therefore by command line.

## Run Plugins and Commands

To run a program you mainly need to point to the launcher class.  
(There can also be programs with multiple launcher classes, but this corner case we deal with later.)

On top, the startup procedure is often also dependent on the framework or library you are using. Especially GUI and reflective libraries often overload the default javaagent and therefore bring their proper plugin.

### Plugins

Maven plugins, like [dependencies](dependencies/#the-classpath) are (once included in the ```pom.xml```) downloaded and added to the classpath. However plugins are commonly compile-time, not run-time dependencies, and merely target a modification of the default build and run process.

In the [default ```pomx.ml```](minimalpom/#template) you already see an empty  
```xml
        <plugins>
        </plugins>
```
section.

All xml snippets show in the following go directly between these tags.

### Run Plugin Samples

Below you find sample run-plugins for **Vanilla**, **Spring Boot** and **JavaFX** projects. There are many more, but this selection should give you a good illustration of the procedure.

> Note how every plugin also slightly alters the launch command! (Command after xml snippet)

=== "Vanilla / No Frameworks"
     ```
     <plugin>
                <groupId>org.codehaus.mojo</groupId>
                <artifactId>exec-maven-plugin</artifactId>
                <version>1.6.0</version>
                <executions>
                    <execution>
                        <goals>
                            <goal>java</goal>
                        </goals>
                    </execution>
                </executions>
                <configuration>
                    <mainClass>eu.kartoffelquadrat.printer.PrinterVsLogger</mainClass>
                </configuration>
     </plugin>
     ```
     Start with: ```mvn clean package exec:java```

=== "Spring Boot"
     ```
     ---No Plugins Needed: Launcher class is auto detected by annotation---
     ```
     Start with: ```mvn clean package spring-boot:run```

=== "Java FX"
     ```
     <plugin>
                <groupId>org.openjfx</groupId>
                <artifactId>javafx-maven-plugin</artifactId>
                <version>0.0.8</version>
                <configuration>
                    <mainClass>eu.kartoffelquadrat.javafxhelloworld.Launcher</mainClass>
                </configuration>
     </plugin>
     ```
     Start with: ```mvn clean package javafx:run```