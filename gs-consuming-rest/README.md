# gs-consuming-rest
Consuming a RESTful Web Service

### References
* https://spring.io/guides/gs/consuming-rest/

### Directions

##### Build a VM and connect
* [AWS](../docs/aws-vm.md)

##### Install JDK 1.8
* [JDK 1.8 Install](../docs/jdk-1.8-install.md)

##### Install Gradle
* [Gradle Install](../docs/gradle-install.md)

##### Create a project directory
	mkdir ~/gs-consuming-rest
	cd ~/gs-consuming-rest

##### Create the directory structure
	mkdir -p src/main/java/hello

##### Create a Gradle build file
	vim build.gradle
```gradle
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
    baseName = 'gs-consuming-rest'
    version =  '0.1.0'
}

repositories {
    mavenCentral()
}

sourceCompatibility = 1.8
targetCompatibility = 1.8

dependencies {
    compile("org.springframework.boot:spring-boot-starter")
    compile("org.springframework:spring-web")
    compile("com.fasterxml.jackson.core:jackson-databind")
    testCompile("junit:junit")
}

task wrapper(type: Wrapper) {
    gradleVersion = '2.10'
}
```

##### Check the REST resource
	curl http://gturnquist-quoters.cfapps.io/api/random
```json
{"type":"success","value":{"id":11,"quote":"I have two hours today to build an app from scratch. @springboot to the rescue!"}}
```
##### Create a domain class 
	vim src/main/java/hello/Quote.java
```java
package hello;

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@JsonIgnoreProperties(ignoreUnknown = true)
public class Quote {

    private String type;
    private Value value;

    public Quote() {
    }

    public String getType() {
        return type;
    }

    public void setType(String type) {
        this.type = type;
    }

    public Value getValue() {
        return value;
    }

    public void setValue(Value value) {
        this.value = value;
    }

    @Override
    public String toString() {
        return "Quote{" +
                "type='" + type + '\'' +
                ", value=" + value +
                '}';
    }
}
```

##### Add an additional class to embed the inner quotation
	vim src/main/java/hello/Value.java
```java
package hello;

import com.fasterxml.jackson.annotation.JsonIgnoreProperties;

@JsonIgnoreProperties(ignoreUnknown = true)
public class Value {

    private Long id;
    private String quote;

    public Value() {
    }

    public Long getId() {
        return this.id;
    }

    public String getQuote() {
        return this.quote;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public void setQuote(String quote) {
        this.quote = quote;
    }

    @Override
    public String toString() {
        return "Value{" +
                "id=" + id +
                ", quote='" + quote + '\'' +
                '}';
    }
}
```
##### Write the Application class that uses RestTemplate to fetch the data from our Spring Boot quotation service
    vim src/main/java/hello/Application.java
```java
package hello;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.boot.CommandLineRunner;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.web.client.RestTemplate;

@SpringBootApplication
public class Application implements CommandLineRunner {

    private static final Logger log = LoggerFactory.getLogger(Application.class);

    public static void main(String args[]) {
        SpringApplication.run(Application.class);
    }

    @Override
    public void run(String... strings) throws Exception {
        RestTemplate restTemplate = new RestTemplate();
        Quote quote = restTemplate.getForObject("http://gturnquist-quoters.cfapps.io/api/random", Quote.class);
        log.info(quote.toString());
    }
}
```

##### Run the gradle wrapper task
	gradle wrapper
```
:wrapper

BUILD SUCCESSFUL

Total time: 7.344 secs
```
##### Build an executable JAR
	./gradlew build
```
:wrapper

BUILD SUCCESSFUL

Total time: 7.344 secs

This build could be faster, please consider using the Gradle Daemon: https://docs.gradle.org/2.10/userguide/gradle_daemon.html
[ec2-user@ip-172-31-36-185 gs-consuming-rest]$ ./gradlew build
:compileJava
:processResources UP-TO-DATE
:classes
:findMainClass
:jar
:bootRepackage
:assemble
:compileTestJava UP-TO-DATE
:processTestResources UP-TO-DATE
:testClasses UP-TO-DATE
:test UP-TO-DATE
:check UP-TO-DATE
:build

BUILD SUCCESSFUL

Total time: 10.252 secs
```
##### Run the JAR
	java -jar build/libs/gs-consuming-rest-0.1.0.jar
```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v1.3.2.RELEASE)

2016-02-19 11:10:34.618  INFO 21776 --- [           main] hello.Application                        : Starting Application on ip-172-31-36-185 with PID 21776 (/home/ec2-user/gs-consuming-rest/build/libs/gs-consuming-rest-0.1.0.jar started by ec2-user in /home/ec2-user/gs-consuming-rest)
2016-02-19 11:10:34.630  INFO 21776 --- [           main] hello.Application                        : No active profile set, falling back to default profiles: default
2016-02-19 11:10:34.736  INFO 21776 --- [           main] s.c.a.AnnotationConfigApplicationContext : Refreshing org.springframework.context.annotation.AnnotationConfigApplicationContext@689d68bc: startup date [Fri Feb 19 11:10:34 EST 2016]; root of context hierarchy
2016-02-19 11:10:36.374  INFO 21776 --- [           main] o.s.j.e.a.AnnotationMBeanExporter        : Registering beans for JMX exposure on startup
2016-02-19 11:10:36.751  INFO 21776 --- [           main] hello.Application                        : Quote{type='success', value=Value{id=9, quote='So easy it is to switch container in #springboot.'}}
2016-02-19 11:10:36.759  INFO 21776 --- [           main] hello.Application                        : Started Application in 2.837 seconds (JVM running for 3.509)
2016-02-19 11:10:36.763  INFO 21776 --- [       Thread-2] s.c.a.AnnotationConfigApplicationContext : Closing org.springframework.context.annotation.AnnotationConfigApplicationContext@689d68bc: startup date [Fri Feb 19 11:10:34 EST 2016]; root of context hierarchy
2016-02-19 11:10:36.767  INFO 21776 --- [       Thread-2] o.s.j.e.a.AnnotationMBeanExporter        : Unregistering JMX-exposed beans on shutdown
```

* * *
[INDEX](../README.md)
