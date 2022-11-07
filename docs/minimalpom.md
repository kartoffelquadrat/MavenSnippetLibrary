# Miminal Pom

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
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