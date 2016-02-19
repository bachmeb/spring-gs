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

##### Create a build.gradle file
    vim build.gradle
```
apply plugin: 'java'
```
##### Run the build task
    gradle build

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
    gradleVersion = '2.3'
}

```
##### Initialize the wrapper scripts
    gradle wrapper

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

* * *
[INDEX](../README.md)
