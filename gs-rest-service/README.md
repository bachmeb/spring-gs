# gs-rest-service
Building a RESTful Web Service

### References
* https://spring.io/guides/gs/rest-service/

##### Build a VM and connect
* [AWS](../docs/aws-vm.md)

##### Install JDK 1.8
* [JDK 1.8 Install](../docs/jdk-1.8-install.md)

##### Install Gradle
* [Gradle Install](../docs/gradle-install.md)

##### Create a project directory
    mkdir ~/gs-rest-service
    cd ~/gs-rest-service

##### Create the directory structure
    mkdir -p src/main/java/hello 

##### Make the gradle.build file
    vim build.gradle
```
buildscript {
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath("org.springframework.boot:spring-boot-gradle-plugin:1.3.2.RELEASE")
    }
}

apply plugin: 'java'
apply plugin: 'eclipse'
apply plugin: 'idea'
apply plugin: 'spring-boot'

jar {
    baseName = 'gs-rest-service'
    version =  '0.1.0'
}

repositories {
    mavenCentral()
}

sourceCompatibility = 1.8
targetCompatibility = 1.8

dependencies {
    compile("org.springframework.boot:spring-boot-starter-web")
    testCompile("junit:junit")
}

task wrapper(type: Wrapper) {
    gradleVersion = '2.10'
}
```
##### Create a resource representation class
    vim src/main/java/hello/Greeting.java
```java
package hello;

public class Greeting {

    private final long id;
    private final String content;

    public Greeting(long id, String content) {
        this.id = id;
        this.content = content;
    }

    public long getId() {
        return id;
    }

    public String getContent() {
        return content;
    }
}
```
##### Create a resource controller
    vim src/main/java/hello/GreetingController.java
```java
package hello;

import java.util.concurrent.atomic.AtomicLong;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

@RestController
public class GreetingController {

    private static final String template = "Hello, %s!";
    private final AtomicLong counter = new AtomicLong();

    @RequestMapping("/greeting")
    public Greeting greeting(@RequestParam(value="name", defaultValue="World") String name) {
        return new Greeting(counter.incrementAndGet(),
                            String.format(template, name));
    }
}
```
##### Make the application executable
    vim src/main/java/hello/Application.java
```java
package hello;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```
##### Build an executable JAR
    ./gradlew build
```
```
##### Run the JAR file
    java -jar build/libs/gs-rest-service-0.1.0.jar
```
```
##### Test the service
* Go to http://localhost:8080/greeting
```
{"id":1,"content":"Hello, World!"}
```
* Provide a name query string parameter with http://localhost:8080/greeting?name=Brian  
```
{"id":2,"content":"Hello, Brian!"}
```

* * *
[INDEX](../README.md)
