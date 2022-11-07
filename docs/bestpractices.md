# Build Best Practices

...

All those are ```plugins``` / go into ```plugins``` section.

## Checkstyle without Complaints

Link to google checks file

Code snipped:

```xml
    <plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-checkstyle-plugin</artifactId>
	<version>3.2.0</version>
	<configuration>
	    <configLocation>google_checks.xml</configLocation>
	    <consoleOutput>true</consoleOutput>
	    <violationSeverity>warning</violationSeverity>
	    <failOnViolation>true</failOnViolation>
	    <failsOnError>true</failsOnError>
	    <linkXRef>false</linkXRef>
	</configuration>
	<executions>
	    <execution>
		<id>validate</id>
		<phase>validate</phase>
		<goals>
		    <goal>check</goal>
		</goals>
	    </execution>
	</executions>
    </plugin>
```

## JavaDoc Parameter Check

```xml
    <plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-javadoc-plugin</artifactId>
	<version>3.4.0</version>
	<configuration>
	    <source>8</source>
	    <javadocExecutable>${java.home}/bin/javadoc</javadocExecutable>
	    <reportOutputDirectory>${project.reporting.outputDirectory}/docs
	    </reportOutputDirectory>
	    <destDir>docs</destDir>
	</configuration>
	<executions>
	    <execution>
		<id>attach-javadocs</id>
		<goals>
		    <goal>jar</goal>
		</goals>
		<configuration>
		    <failOnWarnings>true</failOnWarnings>
		</configuration>
	    </execution>
	</executions>
    </plugin>
```

Generates to top level ```docs``` folder.

The ```failOnWarnings```:```true``` entry ensures your program does not compile unless all parameters are documented.

## Unit Tests Passing

dependency:
```xml
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.10</version>
        <scope>test</scope>
    </dependency>
```

plugin:
```xml
    <plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-surefire-plugin</artifactId>
	<version>2.22.2</version>
    </plugin>
```

Refuses build anless all unit tests defines in ```src/test/java``` pass.

Sample unit test:  

```java
package eu.kartoffelquadrat.whatever;

import org.junit.Test;

/**
 * Simple demo test...
 *
 * @author Maximilian Schiedermeier
 */
public class PrinterVsLoggerTest {

  @Test
  public void testPrinter() {
    AssertTrue(...);
  }

}
```
