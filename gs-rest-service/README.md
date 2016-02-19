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
    gradleVersion = '2.3'
}
```




* * *
[INDEX](../README.md)
