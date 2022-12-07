# Compile Time Dependencies

Most programmers at first are puzzled by the complexity of library integration, and the technical debt that comes along. The habit of graphically dragging libraries at random points into your IDE unfortunately conceals the actual complexity and significance of dependencies.

## Motivation

While the drag and drop approach might work for a single developer and a simple project, it is not a reliable approach for project setup or even scenarios where no graphical access to the code exists (e.g. compiling on a server via SSH).

### The Classpath

What actually happens when you compile a piece of software that uses a library, is that the java compiler sets out for journey to localize that library on your system.

 * Unfortunately just writing the import statement, or calling a library is insufficient - the compiler has no clue where to actually find the library's byte code.
 * What the [compiler actually needs is a ```classpath```](https://stackoverflow.com/a/2396759) reference: That is to say a reference, pin-pointing exactly to a location on your disk.
 * The java ```classpath``` is an environment variably, listing all the places on your disk with potentially relevant libraries. You can think of the ```classpath``` a bit like the Operating System's ```PATH``` variable: It references all the software (respectively java libraries) installed, only in this case specifically for java libraries)

When you drag a JAR into your IDE, under the hood the software performs a classpath extension. This is convenient if you are the only one coding. However, there are good reasons to keep track of all dependencies in a textual, structured way.

#### Why now just download everything yourself?

 * In principle you could manually download all libraries your project needs and manually alter the ```classpath```. You could keep donwloading JARs and run the drag and drop approach for every developer who uses your code.

 * In the best case you have a good software documentation that tells developers the complete list and where to get these JARs. Also you ensure the documentation is always up to date.

 * Most likely you will eventually forget to list a dependency. Or one of the JARs is no longer available for download, or people will just refuse to download the long list of dependencies manually and dragging and dropping them every time they set up a new machine.

 * There might also be a security issue in one of the libraries, but since you do not specify your dependencies in a standardized form auto-notifiers such as [GitHubs Dependabot](https://github.com/dependabot) are not going to tell you.

This is once more where maven comes to rescue. Maven has a dedicated part in the ```pom.xml``` file to indicate all dependencies textually. Since the ```pom.xml``` resides alongside the sources of your project, everyone who wants to work with your project automatically gets all dependencies, specified between the ```<dependencies>...</dependencies>``` tags.  
Nobody needs to manually download the decencies. Maven contacts official servers to download cryptographically signed versions of your dependencies, and adds them automatically to your classpath.

### Mavens Dependency Model

The Maven ```pom.xml``` only stores a unique description of your dependencies, not the referenced libraries themselves. Luckily! Otherwise your repository would rapidly exceed a tolerable size.

A dependency entry might look like this:  

```xml
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.11.0</version>
</dependency>
```

When Maven spots the above ```pom.xml``` entry, it will run the following procedure:

 * Check if you already have the referenced dependency locally (maven has a local buffer in your homedirectory ```.m2``` folder - It can be safely deleted if you run out of space.)
 * If not yet found locally, maven contacts the official library servers and downloads the jar for you. It then stores it in your local buffer, so things are faster next time.
 * Once the dependency resolved, Maven adds the locally downloaded artifact to your ```CLASSPATH```. This means starting now, any occurrence of the library reference can now be correctly resolved.

The great thing about this procedure is:  
If your pom.xml evolves, all developers have automatically the full list of all dependencies, and the right versions - without a single mouse click in IDE menus!

## Finding Libraries

At this point you might how you can figure out the correct dependency entry for each and any java library you might ever need.  
Luckily this part has been made very simple for you. Most libraries are indexed in the official repos:  
If e.g. you are looking for the *Apache Commons IO* library, to conveniently load files from disk you can simply:

 * Visit the maven central, a world wide collection of most public libraries: [https://mvnrepository.com/](https://mvnrepository.com/)
 * Search for the needed artifact: Type e.g.[Apache Commons IO](https://mvnrepository.com/artifact/commons-io/commons-io)
 * Click the version number:  Usually you want [the most recent one](https://mvnrepository.com/artifact/commons-io/commons-io/2.11.0)
 * Copy and paste the dependency block into your ```pom.xml```  
```xml
<dependency>
    <groupId>commons-io</groupId>
    <artifactId>commons-io</artifactId>
    <version>2.11.0</version>
</dependency>
```

### Sample Library Use

Now that maven has conveniently placed the ```commons-io``` library on your ```classpath``` you can directly access it from source code.  
(Don't forget to actually import the library!)

```java
package eu.kartoffelquadrat.liveprogramming;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;
import org.apache.commons.io.IOUtils;

public class Launcher {

  public static void main(String[] args) throws IOException {

    // Program prints its own code...
    FileInputStream fisTargetFile = new FileInputStream(new File(
        "/Users/schieder/Desktop/LiveProgramming/src/main/java/eu/kartoffelquadrat/liveprogramming/Launcher.java"));
    String targetFileStr = IOUtils.toString(fisTargetFile, "UTF-8");
    System.out.println(targetFileStr);
  }
}
```