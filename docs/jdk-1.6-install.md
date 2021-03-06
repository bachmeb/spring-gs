# jdk-1.6-install

##### Show all of the environment variables
	declare

##### Look for JAVA in the list of environmental variables
	env | grep JAVA

##### Echo the current JAVA_HOME
	echo $JAVA_HOME

##### Ask where is Java?
	whereis java

##### Check the Java version
*As of Feb 2016, the standard Java version installed on Amazon Linux AMI is 1.7*  
```
java -version
```

##### See if the Java compiler is installed
	whereis javac

##### Search yum for openjdk
	yum search openjdk

##### Install the Open JDK version 1.8
	sudo yum install java-1.8.0-openjdk-devel

##### Check the Java compiler version
*Should be javac 1.8.0_65*
```
javac -version
```

##### Check the Java version
*Should still be java version 1.7.0_91*  
```
java -version
```
```
java version "1.7.0_91"
OpenJDK Runtime Environment (amzn-2.6.2.2.63.amzn1-x86_64 u91-b00)
OpenJDK 64-Bit Server VM (build 24.91-b01, mixed mode)
```

##### Tell Linux to use the JDK 1.8 Java interpreter
    sudo alternatives --config java

##### Check the Java version
*Should now be 1.8*  
```
java -version
```
```
openjdk version "1.8.0_65"
OpenJDK Runtime Environment (build 1.8.0_65-b17)
OpenJDK 64-Bit Server VM (build 25.65-b01, mixed mode)
```

##### Echo the $JAVA_HOME environment variable
	echo $JAVA_HOME

##### Read the symlinks in /usr/lib/jvm/
	ls -l /usr/lib/jvm/

##### Confirm that /usr/lib/jvm/java points to etc/alternatives/java_sdk
	ls -l /usr/lib/jvm/java

##### Confirm that /etc/alternatives/java_sdk points to /usr/lib/jvm/java-1.8.0-openjdk.x86_64
	ls -l /etc/alternatives/java_sdk

##### Confirm that /usr/lib/jvm/java-1.8.0-openjdk.x86_64 is the installation directory for Java SDK 1.8
	ls -l /usr/lib/jvm/java-1.8.0-openjdk.x86_64

##### Set the JAVA_HOME environment variable to point to the JDK 1.8 directory
	export JAVA_HOME='/usr/lib/jvm/java'

##### Echo the $JAVA_HOME environment variable
	echo $JAVA_HOME

##### List the contents of the $JAVA_HOME/ directory
	ls -l $JAVA_HOME/
