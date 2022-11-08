# Run

The whole point of Maven is that you can build your software reliably, no matter the environment - this also includes settings where you have no graphical access, and maybe not even an IDE.  
The default way to compile a maven project is therefore by command line.

## Run Plugins and Commands

To run a program you need to have your ```pom.xml``` point to the desired launcher class.  
There can also be programs with multiple launcher classes, but this corner case we deal with later.

On top, the startup procedure is often also dependent on the framework or library you are using. Especially GUI and reflective libraries tend to [overload the default ```javaagent```](https://www.baeldung.com/java-instrumentation#:~:text=In%20general%2C%20a%20java%20agent,javaagent%20parameter%20at%20JVM%20startup) and therefore also bring their own plugins that need to be added to the ```pom.xml```.

### Plugins

Maven plugins, like [dependencies](dependencies/#the-classpath) are (once included in the ```pom.xml```) downloaded and added to the .m2 directory and classpath. However plugins are commonly compile-time dependencies, not run-time dependencies like libraries. Plugins merely target a modification of the default build and run process.

In the [default ```pom.xml```](minimalpom/#template) you already see an empty section for plugins:  
```xml
        <plugins>
        </plugins>
```

All xml snippets shown in the following go directly within these tags.

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
     Start your program with: ```mvn clean package exec:java```

=== "Spring Boot"
     ```
     ---No Plugins Needed: Launcher class is auto detected by annotation---
     ```
      Start your program with: ```mvn clean package spring-boot:run```

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
      Start your program with: ```mvn clean package javafx:run```