# Minimal Pom, Hello World

Once you have decided on your ```groupId```, ```artifactId``` and also set up the required folder structure, the next step is to create a minimal ```pom.xml``` file.  
You can use below template as starting point, although **you will need to make some minor changes**.

## Pom Template

Create the ```pom.xml``` at top level of your project, then paste in the below template content:

```xml
<?xml version="1.0"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>eu.kartoffelquadrat</groupId>
  <artifactId>projectname</artifactId>
  <packaging>jar</packaging>
  <version>1.0</version>
  <name>printer</name>
  <url>http://maven.apache.org</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.target>1.8</maven.compiler.target>
    <maven.compiler.source>1.8</maven.compiler.source>
    <build.name>ProjectNameInCamelCase</build.name>
  </properties>

  <!-- main developer -->
  <developers>
    <developer>
      <name>Maximilian Schiedermeier</name>
      <email>maximilian.schiedermeier@mcgill.ca</email>
      <organization>kartoffelquadrat.eu</organization>
      <organizationUrl>https://github.com/kartoffelquadrat</organizationUrl>
    </developer>
  </developers>

  <!-- legal -->
  <licenses>
    <license>
      <name>MIT License</name>
      <url>http://www.opensource.org/licenses/mit-license.php</url>
      <distribution>repo</distribution>
    </license>
  </licenses>

  <dependencies>
  </dependencies>

  <build>
    <!-- Override default name of generated artifacts -->
    <finalName>${build.name}</finalName>
    <plugins>
    </plugins>
  </build>
</project>
```

### Changes

Next go over below list and make sure to updated the indicated fields:

 * Replace the ```groupId``` tag content by something that [describes your group](layout/#groupid-artifactid-packages).
 * Replace the ```artifactId``` tag content by something that [describes your project purpose](layout/#groupid-artifactid-packages).
 * Update the ```name``` tag to a single human readable word, describing your product.
 * Update the ```build.name``` variable to a camelCase description of your product, as you want the generated executable file to be named.
 * **Update the developer name and info! Do not use my name!**
 * Choose a license, or remove the license block if you want to reserve all rights.

## Hello World

Next create a file Launcher.java in the lowest level of your ```src/main/java/...``` folder with the following content:  

```java
package eu.kartoffelquadrat.liveprogramming;

public class Launcher {

  public static void main(String[] args) {

    System.out.println("Hello, World!");
  }
}
```

 > **Note**: Make sure the package matches your ```groupid``` + ```artifactid``` + any other subpackages you have in your folder structure!
