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
##### Run the gradle wrapper task
    gradle wrapper
```
Download https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-gradle-plugin/1.3.2.RELEASE/spring-boot-gradle-plugin-1.3.2.RELEASE.pom
Download https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-tools/1.3.2.RELEASE/spring-boot-tools-1.3.2.RELEASE.pom
Download https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-parent/1.3.2.RELEASE/spring-boot-parent-1.3.2.RELEASE.pom
Download https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-dependencies/1.3.2.RELEASE/spring-boot-dependencies-1.3.2.RELEASE.pom
Download https://repo1.maven.org/maven2/org/springframework/spring-framework-bom/4.2.4.RELEASE/spring-framework-bom-4.2.4.RELEASE.pom
Download https://repo1.maven.org/maven2/org/springframework/data/spring-data-releasetrain/Gosling-SR2A/spring-data-releasetrain-Gosling-SR2A.pom
Download https://repo1.maven.org/maven2/org/springframework/data/build/spring-data-build/1.7.3.RELEASE/spring-data-build-1.7.3.RELEASE.pom
Download https://repo1.maven.org/maven2/org/springframework/integration/spring-integration-bom/4.2.4.RELEASE/spring-integration-bom-4.2.4.RELEASE.pom
Download https://repo1.maven.org/maven2/org/springframework/security/spring-security-bom/4.0.3.RELEASE/spring-security-bom-4.0.3.RELEASE.pom
Download https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-loader-tools/1.3.2.RELEASE/spring-boot-loader-tools-1.3.2.RELEASE.pom
Download https://repo1.maven.org/maven2/io/spring/gradle/dependency-management-plugin/0.5.4.RELEASE/dependency-management-plugin-0.5.4.RELEASE.pom
Download https://repo1.maven.org/maven2/org/springframework/spring-core/4.2.4.RELEASE/spring-core-4.2.4.RELEASE.pom
Download https://repo1.maven.org/maven2/commons-logging/commons-logging/1.2/commons-logging-1.2.pom
Download https://repo1.maven.org/maven2/org/apache/commons/commons-parent/34/commons-parent-34.pom
Download https://repo1.maven.org/maven2/org/apache/apache/13/apache-13.pom
Download https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-gradle-plugin/1.3.2.RELEASE/spring-boot-gradle-plugin-1.3.2.RELEASE.jar
Download https://repo1.maven.org/maven2/org/springframework/boot/spring-boot-loader-tools/1.3.2.RELEASE/spring-boot-loader-tools-1.3.2.RELEASE.jar
Download https://repo1.maven.org/maven2/io/spring/gradle/dependency-management-plugin/0.5.4.RELEASE/dependency-management-plugin-0.5.4.RELEASE.jar
Download https://repo1.maven.org/maven2/org/springframework/spring-core/4.2.4.RELEASE/spring-core-4.2.4.RELEASE.jar
Download https://repo1.maven.org/maven2/commons-logging/commons-logging/1.2/commons-logging-1.2.jar
:wrapper

BUILD SUCCESSFUL

Total time: 10.273 secs
```
    
##### Run the application
    ./gradlew build
```
:compileJava UP-TO-DATE
:processResources UP-TO-DATE
:classes UP-TO-DATE
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

Total time: 9.174 secs
```
##### Run the JAR file
    java -jar build/libs/gs-rest-service-0.1.0.jar
```
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v1.3.2.RELEASE)

2016-02-19 10:50:24.711  INFO 21652 --- [           main] hello.Application                        : Starting Application on ip-172-31-36-185 with PID 21652 (/home/ec2-user/gs-rest-service/build/libs/gs-rest-service-0.1.0.jar started by ec2-user in /home/ec2-user/gs-rest-service)
2016-02-19 10:50:24.736  INFO 21652 --- [           main] hello.Application                        : No active profile set, falling back to default profiles: default
2016-02-19 10:50:24.881  INFO 21652 --- [           main] ationConfigEmbeddedWebApplicationContext : Refreshing org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContext@4066d194: startup date [Fri Feb 19 10:50:24 EST 2016]; root of context hierarchy
2016-02-19 10:50:26.192  INFO 21652 --- [           main] o.s.b.f.s.DefaultListableBeanFactory     : Overriding bean definition for bean 'beanNameViewResolver' with a different definition: replacing [Root bean: class [null]; scope=; abstract=false; lazyInit=false; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=org.springframework.boot.autoconfigure.web.ErrorMvcAutoConfiguration$WhitelabelErrorViewConfiguration; factoryMethodName=beanNameViewResolver; initMethodName=null; destroyMethodName=(inferred); defined in class path resource [org/springframework/boot/autoconfigure/web/ErrorMvcAutoConfiguration$WhitelabelErrorViewConfiguration.class]] with [Root bean: class [null]; scope=; abstract=false; lazyInit=false; autowireMode=3; dependencyCheck=0; autowireCandidate=true; primary=false; factoryBeanName=org.springframework.boot.autoconfigure.web.WebMvcAutoConfiguration$WebMvcAutoConfigurationAdapter; factoryMethodName=beanNameViewResolver; initMethodName=null; destroyMethodName=(inferred); defined in class path resource [org/springframework/boot/autoconfigure/web/WebMvcAutoConfiguration$WebMvcAutoConfigurationAdapter.class]]
2016-02-19 10:50:27.380  INFO 21652 --- [           main] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat initialized with port(s): 8080 (http)
2016-02-19 10:50:27.400  INFO 21652 --- [           main] o.apache.catalina.core.StandardService   : Starting service Tomcat
2016-02-19 10:50:27.401  INFO 21652 --- [           main] org.apache.catalina.core.StandardEngine  : Starting Servlet Engine: Apache Tomcat/8.0.30
2016-02-19 10:50:27.557  INFO 21652 --- [ost-startStop-1] o.a.c.c.C.[Tomcat].[localhost].[/]       : Initializing Spring embedded WebApplicationContext
2016-02-19 10:50:27.557  INFO 21652 --- [ost-startStop-1] o.s.web.context.ContextLoader            : Root WebApplicationContext: initialization completed in 2691 ms
2016-02-19 10:50:28.197  INFO 21652 --- [ost-startStop-1] o.s.b.c.e.ServletRegistrationBean        : Mapping servlet: 'dispatcherServlet' to [/]
2016-02-19 10:50:28.211  INFO 21652 --- [ost-startStop-1] o.s.b.c.embedded.FilterRegistrationBean  : Mapping filter: 'characterEncodingFilter' to: [/*]
2016-02-19 10:50:28.212  INFO 21652 --- [ost-startStop-1] o.s.b.c.embedded.FilterRegistrationBean  : Mapping filter: 'hiddenHttpMethodFilter' to: [/*]
2016-02-19 10:50:28.212  INFO 21652 --- [ost-startStop-1] o.s.b.c.embedded.FilterRegistrationBean  : Mapping filter: 'httpPutFormContentFilter' to: [/*]
2016-02-19 10:50:28.212  INFO 21652 --- [ost-startStop-1] o.s.b.c.embedded.FilterRegistrationBean  : Mapping filter: 'requestContextFilter' to: [/*]
2016-02-19 10:50:28.766  INFO 21652 --- [           main] s.w.s.m.m.a.RequestMappingHandlerAdapter : Looking for @ControllerAdvice: org.springframework.boot.context.embedded.AnnotationConfigEmbeddedWebApplicationContext@4066d194: startup date [Fri Feb 19 10:50:24 EST 2016]; root of context hierarchy
2016-02-19 10:50:28.870  INFO 21652 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/greeting]}" onto public hello.Greeting hello.GreetingController.greeting(java.lang.String)
2016-02-19 10:50:28.877  INFO 21652 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error],produces=[text/html]}" onto public org.springframework.web.servlet.ModelAndView org.springframework.boot.autoconfigure.web.BasicErrorController.errorHtml(javax.servlet.http.HttpServletRequest,javax.servlet.http.HttpServletResponse)
2016-02-19 10:50:28.880  INFO 21652 --- [           main] s.w.s.m.m.a.RequestMappingHandlerMapping : Mapped "{[/error]}" onto public org.springframework.http.ResponseEntity<java.util.Map<java.lang.String, java.lang.Object>> org.springframework.boot.autoconfigure.web.BasicErrorController.error(javax.servlet.http.HttpServletRequest)
2016-02-19 10:50:28.941  INFO 21652 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/webjars/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2016-02-19 10:50:28.941  INFO 21652 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2016-02-19 10:50:29.018  INFO 21652 --- [           main] o.s.w.s.handler.SimpleUrlHandlerMapping  : Mapped URL path [/**/favicon.ico] onto handler of type [class org.springframework.web.servlet.resource.ResourceHttpRequestHandler]
2016-02-19 10:50:29.210  INFO 21652 --- [           main] o.s.j.e.a.AnnotationMBeanExporter        : Registering beans for JMX exposure on startup
2016-02-19 10:50:29.328  INFO 21652 --- [           main] s.b.c.e.t.TomcatEmbeddedServletContainer : Tomcat started on port(s): 8080 (http)
2016-02-19 10:50:29.331  INFO 21652 --- [           main] hello.Application                        : Started Application in 5.375 seconds (JVM running for 6.08)
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
