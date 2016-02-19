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

##### Add a source for 3rd party libraries to build.gradle
    vim build.gradle
```
repositories {
    mavenCentral()
}
```
* * *
[INDEX](../README.md)
