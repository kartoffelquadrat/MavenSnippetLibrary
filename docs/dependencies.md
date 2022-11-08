# Compile Time Dependencies

Most programmers at first are puzzled by the complexity of library integration. The habit of graphically dragging libraries at random points into your IDE unfortunately conceals the actual complexity of code dependencies.

## Motivation

While the drag and drop approach might work for a single developer and a simple project, it is not a reliable approach for fast workstation setup or even scenarios where no graphical access to the code exists (e.g. compiling on a server via SSH).

### The Classpath

What actually happens when you try to compile a piece of software (depending on a library call) is that the java compiler sets out for journey to localize the library you are referring to.  Unfortunately by just writing the import statement, or calling a library the compiler has no clue where to actually find the library byte code. What the [compiler actually needs is a ```classpath```](https://stackoverflow.com/a/2396759) reference, pin-pointing exactly to a valid location on disk. (The ```classpath``` is a bit like the ```PATH``` variable on your operating system. It indexes all the software installed, only in this case specifically for java libraries)

 * In principle you could manually download all libraries your project needs and manually alter the ```classpath``` every time you compile or run your software. When you drag and drop a library jar file on your IDE that process might happen implicitly.

 * The problem is that drag and drop only solves the issue for the very machine you are working on, not for any other developer who clones your project.

This is once more where maven comes to rescue. Maven has a dedicated part in the ```pom.xml``` file to indicate all dependencies textually. Since the ```pom.xml``` resides alongside the sources of your project, everyone who want to work with your project automatically gets all it's dependencies, stored between the ```<dependencies>...</dependencies>``` tags.

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

When Maven finds the above entry, it will run the following procedure:

 * Check if you already have the referenced dependency locally (maven has a local buffer in your homedirectory ```.m2``` folder)
 * If not yet found locally, maven contacts the official library servers and downloads the jar for you. It then stores it in your local buffer, so things are faster next time.
 * Once the dependency resolved, Maven adds the locally downloaded artifact to your ```CLASSPATH```. This means starting now, any occurence of the library reference can now be correctly resolved.

The great thing about this procedure is:  
If your pom.xml evolves, all developers have automatically the full list of all dependencies, and the right versions - without a single mouse click in IDE menus!

## Finding Libraries

At this point you might wonder how on earth you are supposed to figure out the correct dependency entry for each and any java library you might ever need.  
Luckily this part has been made very simple for you: If e.g. you are looking for the *Apache Commons IO* library, to conveniently load files from disk you can simply:

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

Now that maven has conveniently placed the commons-io library on your ```classpath``` you can directly access it from source code.  
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