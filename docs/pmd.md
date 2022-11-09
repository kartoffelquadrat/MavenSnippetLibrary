# PMD

[PMD is a static source code analyzer](https://en.wikipedia.org/wiki/PMD_(software)) that can be highly configured to detect issues on a wide range, from poor coding, missing documentation to severe security or performance flaws.

PMD can also be [added to IntelliJ as graphical plugin](https://plugins.jetbrains.com/plugin/1137-pmdplugin).

## Configuration

Add the following plugin to your ```pom.xml```:

```xml
<!-- PMD static code analysis -->
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
	<artifactId>maven-pmd-plugin</artifactId>
	<version>3.13.0</version>
	<configuration>
	    <rulesets>
		<!-- full list: bestpractices, codestyle, design, documentation, errorprone, multithreading, performance-->
		<ruleset>/category/java/bestpractices.xml</ruleset>
	    </rulesets>
	    <!-- failOnViolation is actually true by default, but can be disabled -->
	    <failOnViolation>true</failOnViolation>
	    <!-- printFailingErrors is pretty useful -->
	    <printFailingErrors>true</printFailingErrors>
	    <linkXRef>false</linkXRef>
	</configuration>
	<executions>
	    <execution>
		<goals>
		    <goal>check</goal>
		</goals>
		<!-- Enforce the pmd:check goal is auto-executed during package phase-->
		<phase>package</phase>
	    </execution>
	</executions>
</plugin>
```

Invokation:

 * The plugin is configured to be automcatically executed on every ```mvn clean package```.
 * To run the code analysis without the full ```package``` phase, type: ```mvn clean pmd:check```.

## Tweaks

Above default configuration only tests for codestyle violations. To get the full range of feedback, enable the full ```rulesets``` list:

```xml
    <ruleset>/category/java/bestpractices.xml</ruleset>
    <ruleset>/category/java/codestyle.xml</ruleset>
    <ruleset>/category/java/design.xml</ruleset>
    <ruleset>/category/java/documentation.xml</ruleset>
    <ruleset>/category/java/errorprone.xml</ruleset>
    <ruleset>/category/java/multithreading.xml</ruleset>
    <ruleset>/category/java/performance.xml</ruleset>
```

