# Run

This one depends on the frameworks you are using:

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
     ---No Plugins Needed---
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