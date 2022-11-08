# Build Best Practices

Just because a program seemingly works, that does not mean it is free of errors or well written code.  
Most codes agree that code gains significantly in quality, if:

 * Style-Checked
 * Documented
 * Tested

Below I show you which plugins you can integrate into your project to enforce this minimal quality on your code.

 > Note: Very likely your program will at first no longer run once you enable all below plugins. It can be tempting to just remove the plugins again as a "*quick fix*". However, keep in mind that you then only turn off the flashing warning lights, not the actual quality issues with your code.

## Checkstyle

 * First of all you will need a [checkstyle configuration file. I recommend the one by google](assets/google_checks.xml). The checkstyle file is simply a restrictive linter configuration that inherently knows about all naming and indentation conventions for java code.  
Download the ```google_checks.xml``` file and place it in your project's root directory, right next to the ```pom.xml```.

 * Next you need to enable the *Maven Checkstyle Plugin* in your ```pom.xml```.  
Paste the below snippet somewhere between your ```pom.xml```s ```<plugins>...</plugins>``` tags.

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

From now on, maven will invoke the checkstyle linter every single time you attempt to build or run your code.  
Note that your program will not start unless your code complies to all checkstyle rules. This even includes formatting.  
While this sounds tedious at first, this plugin is a guaranteed time and trouble saver - it also prevents anyone else from adding any poorly formatted or weirdly named code.


## JavaDoc Parameter Check

The JavaDoc plugin does no require an extra configuration file. Simply enable the *JavaDoc Plugin* in your ```pom.xml```.  
To do so, paste the below snippet somewhere between your ```pom.xml```s ```<plugins>...</plugins>``` tags.

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


This plugin does two things:

 * It first ensures **all** your methods are documented. That means unless a method ```@overrides``` a base class or interface, it must carry a javadoc description, explaining the inputs and outputs of your signature, including mention of potential exceptions.  
Here is a little example:  
```java
/**
 * Little demo method to count a given amount of sheep.
 * @param sheep as the amount of sheep to count.
 * @returns boolean indicating if any black sheep were encountered.
 * @throws IOException in case a sheep jumped a fence.
 */
public static boolean count(int sheep) throws IOException {
	[...]
}
```

 * If and only if all methods are fully documented, the build process continues and the plug generates a human readable HTML Java API documentation to the ```docs``` folder. Note that you can use this output as-is, e.g. for your GitHub pages project documentation, so everyone working with your codebase has a convenient access to user navigable documentation.  
Also, since this documentation is created on every build  or run, it automatically stays fully up to date.

 > **Note:** The above plugin is set to the strictest possible configuration. Even warnings are interpreted as a direct compile failure. (```failOnWarnings```:```true```). To good side is that you can be sure your codebase never has a single undocumented method parameter.

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
