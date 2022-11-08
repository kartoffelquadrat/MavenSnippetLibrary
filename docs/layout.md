# Project Layout

You might be used to a discovery like project setup, where you just throw in you code files at random place into the project, then hopefully click a green triangle start button in your IDE and cross you fingers it will magically work.

While this may work for tiny projects or little scripting codebases that you hammer out in a day, bigger software projects often have a long list of requirements and likewise are expected to compile and run reliable no matter to platform or developer cloning the project.

This is where maven enters the game. Maven replaces the click based project configuration (where a developer sets up their project via IDE menus), by a central xml textual description, the ```pom.xml``.

The good news is: Once the ```pom.xml``` is set up, no one ever needs to touch a UI menu again.

## Files and Folders

The root of a maven project should roughly look like this:
![folderlayout](../captures/folderlayout.png)

Before we go into the details, note that there are two important entires at root level:

 * ```pom.xml``` which contains all project configuration. Almost everything presented on this webpage are snippets that extend this file.
 * A ```src``` folder, all your source code, tests, even resources go somewhere into that folder.

Depending on which configurations you add to your ```pom.xml```, you might have additional content on top level. But for a start these are the minimum requirement for your project at root level.

Next let's look at the nested content of the ```src``` folder. Everything that carries a red marker in the capture above must be in place **exactly as shown**. If you alter that structure, your project simply is not valid and there are zero guarantees for what happens when someone else attempts to build and run it.

 * Your java sources go into ```src/main/java```
 * Your java tests fo into ```src/test/java```
 * Your resource files go into ```src/main/resources```

## GroupId, ArtifactId, Packages

In the ```test/java``` and ```src/java``` folder you see subfolders: ```eu/kartoffelquadrat/printer```.

 * ```eu/kartoffelquadrat``` has a blue label. This on represents your ```groupId```. The ```groupId``` is specific to the developer (or team of developers) responsible for the project. By convention it is the inverted domain name of your affiliation. So for instance if you are a student at McGill you could use: ```ca/mcgill``` instead of ```eu/kartoffelquadrat```. It can also be longer than two segments, but it seems to have become an unwritten convention to use two segments.  
 > **Do not use ```eu/kartoffelquadrat``` for your projects.** That domain is owned by me, so you better not pretend to release software on my behalf. ;)
 *  ```liveprogramming``` is the ```artifactId```. It has a yellow label. It describes the specific purpose of your project. For instance if you are developing a board game *tic tac toe* client, it could be ```ticTacToeClient```.
 * Optionally you can create further subfolders for sub-packages (purple label). E.g. if you have an MVC structure you can place parallel folders for ```model```, ```view```, ```control``` under your ```artifactId```. Packages are optional for smaller projects, but for everything that has more than 5 classes, things can get very confusing without packages.