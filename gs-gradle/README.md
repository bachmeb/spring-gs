# gs-gradle
Building Java Projects with Gradle

### References
* https://spring.io/guides/gs/gradle/

##### Build a VM and connect
* [AWS](../docs/aws-vm.md)

##### Install JDK 1.8
* [JDK 1.8 Install](../docs/jdk-1.8-install.md)

##### Install Gradle
* [Gradle Install](../docs/gradle-install.md)

##### Create a project directory
    mkdir ~/gs-gradle
    cd ~/gs-gradle

##### Create the directory structure
    mkdir -p src/main/java/hello

##### Create the HelloWorld class
    vim src/main/java/hello/HelloWorld.java
```java
package hello;

public class HelloWorld {
  public static void main(String[] args) {
    Greeter greeter = new Greeter();
    System.out.println(greeter.sayHello());
  }
}
```

##### Create the Greeter class
    vim src/main/java/hello/Greeter.java
```
package hello;

public class Greeter {
  public String sayHello() {
    return "Hello world!";
  }
}
```
##### See the Gradle tasks that are available
    gradle tasks
```

:tasks

------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Build Setup tasks
-----------------
init - Initializes a new Gradle build. [incubating]
wrapper - Generates Gradle wrapper files. [incubating]

Help tasks
----------
buildEnvironment - Displays all buildscript dependencies declared in root project 'gs-gradle'.
components - Displays the components produced by root project 'gs-gradle'. [incubating]
dependencies - Displays all dependencies declared in root project 'gs-gradle'.
dependencyInsight - Displays the insight into a specific dependency in root project 'gs-gradle'.
help - Displays a help message.
model - Displays the configuration model of root project 'gs-gradle'. [incubating]
projects - Displays the sub-projects of root project 'gs-gradle'.
properties - Displays the properties of root project 'gs-gradle'.
tasks - Displays the tasks runnable from root project 'gs-gradle'.

To see all tasks and more detail, run gradle tasks --all

To see more detail about a task, run gradle help --task <task>

BUILD SUCCESSFUL

Total time: 2.428 secs
```

##### Create a build.gradle file
    vim build.gradle
```
apply plugin: 'java'
```
##### Run the build task
    gradle build
```

:compileJava
:processResources UP-TO-DATE
:classes
:jar
:assemble
:compileTestJava UP-TO-DATE
:processTestResources UP-TO-DATE
:testClasses UP-TO-DATE
:test UP-TO-DATE
:check UP-TO-DATE
:build

BUILD SUCCESSFUL

Total time: 5.608 secs
```
##### Change HelloWorld.java to look like this
    vim src/main/java/hello/HelloWorld.java
```java
package hello;

import org.joda.time.LocalTime;

public class HelloWorld {
  public static void main(String[] args) {
    LocalTime currentTime = new LocalTime();
    System.out.println("The current local time is: " + currentTime);

    Greeter greeter = new Greeter();
    System.out.println(greeter.sayHello());
  }
}
```

##### Edit build.gradle
    vim build.gradle
* Add Maven Central as a source for 3rd party libraries
* Declare the 3rd party libraries
* Specify the name for the JAR artifact
* Add a Gradle wrapper task
```
repositories {
    mavenCentral()
}

sourceCompatibility = 1.8
targetCompatibility = 1.8

dependencies {
    compile "joda-time:joda-time:2.2"
}

jar {
    baseName = 'gs-gradle'
    version =  '0.1.0'
}

task wrapper(type: Wrapper) {
    gradleVersion = '2.10'
}

```
##### Read the gradle tasks again
    gradle tasks
```
:tasks

------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Build tasks
-----------
assemble - Assembles the outputs of this project.
build - Assembles and tests this project.
buildDependents - Assembles and tests this project and all projects that depend on it.
buildNeeded - Assembles and tests this project and all projects it depends on.
classes - Assembles main classes.
clean - Deletes the build directory.
jar - Assembles a jar archive containing the main classes.
testClasses - Assembles test classes.

Build Setup tasks
-----------------
init - Initializes a new Gradle build. [incubating]

Documentation tasks
-------------------
javadoc - Generates Javadoc API documentation for the main source code.

Help tasks
----------
buildEnvironment - Displays all buildscript dependencies declared in root project 'gs-gradle'.
components - Displays the components produced by root project 'gs-gradle'. [incubating]
dependencies - Displays all dependencies declared in root project 'gs-gradle'.
dependencyInsight - Displays the insight into a specific dependency in root project 'gs-gradle'.
help - Displays a help message.
model - Displays the configuration model of root project 'gs-gradle'. [incubating]
projects - Displays the sub-projects of root project 'gs-gradle'.
properties - Displays the properties of root project 'gs-gradle'.
tasks - Displays the tasks runnable from root project 'gs-gradle'.

Verification tasks
------------------
check - Runs all checks.
test - Runs the unit tests.

Other tasks
-----------
wrapper

Rules
-----
Pattern: clean<TaskName>: Cleans the output files of a task.
Pattern: build<ConfigurationName>: Assembles the artifacts of a configuration.
Pattern: upload<ConfigurationName>: Assembles and uploads the artifacts belonging to a configuration.

To see all tasks and more detail, run gradle tasks --all

To see more detail about a task, run gradle help --task <task>

BUILD SUCCESSFUL

Total time: 5.356 secs
```
##### Initialize the wrapper scripts
    gradle wrapper
```
:wrapper

BUILD SUCCESSFUL

Total time: 5.173 secs
```

##### Confirm that the gradle-wrapper.jar and gradle-wrapper.properties files have been added to the gradle/wrapper directory
    ls -lahR

##### Run the wrapper script to perform the build task
    ./gradlew build

##### Edit build.gradle
    vim build.gradle
* Add gradle's application plugin to make the code runnable
```
apply plugin: 'application'

mainClassName = 'hello.HelloWorld'
```
##### Run the app
    ./gradlew run
```
:compileJava
:processResources UP-TO-DATE
:classes
:run
The current local time is: 10:26:07.254
Hello world!

BUILD SUCCESSFUL

Total time: 6.733 secs
```
* * *
[INDEX](../README.md)
