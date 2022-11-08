# Build Best Practices

Just because a program seemingly works, it does not mean it is free of errors or well written.
However, while Tests, documentation and style checks are by themselves not a guarantee for good code, the are generally considered a non-optional prerequisite.

Below I show you which plugins you can integrate into your project to enforce this minimal quality on your code.

 > Note: Very likely your program will at first no longer run once you enable these plugins. You might be tempted to say **Nah, does not work - I'll remove this BLEEP**. However, keep in mind that you then only fix the symptoms, not the actual code issues you just were told about.  
Better: take an hour or two, carefully read through the warnings you received, then change your code so the warnings no longer appear.


## Checkstyle

 * First of all you will need a [checkstyle configuration file. I recommend the one by google](assets/google_checks.xml). Place it in your project's root directory, right next to the ```pom.xml```.

 * Next you need to enable the *Maven Checkstyle Plugin* in your ```pom.xml```. Paste the below snippet somewhere between your ```pom.xml```s ```<plugins>...</plugins>``` tags.

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

### Effect

Your program will not start unless your code complies to all checkstyle rules. This even includes formatting.  
You really do not want to have two developers in your team who use different auto-formats and produce massive merge commits on every commit. This plugin can be a real and trouble time-saver.


## JavaDoc Parameter Check

Enable the *JavaDoc Plugin* in your ```pom.xml```. Paste the below snippet somewhere between your ```pom.xml```s ```<plugins>...</plugins>``` tags.

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

### Effect

Generates to top level ```docs``` folder, using the JavaDoc in your source code.

This one is very strict. The ```failOnWarnings```:```true``` entry ensures your program does not compile unless all parameters are documented. So if you forgot a single javadoc parameter documentation you cannot run your code.

This may sound tedious, but it actually ensures your codebase does not grow and uncontrolled while getting harder and harder to understand.

## Unit Tests Passing

Enforce the passing of all *Unit Tests* via your ```pom.xml```. Unlike the previous, this one is enabled via a **plugin** in combination with a **test scoped dependency**. Paste the below snippet somewhere between your ```pom.xml```s respective ```<dependencies>...</dependencies>``` and ```<plugins>...</plugins>``` tags.

Dependency snippet:
```xml
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.10</version>
        <scope>test</scope>
    </dependency>
```

Plugin snippet:
```xml
    <plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-surefire-plugin</artifactId>
	<version>2.22.2</version>
    </plugin>
```

### Effect

Refuses build unless all unit tests defines in ```src/test/java``` pass.

Here is how to create a sample unit test:

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
