# gradle-install


##### Search yum for available Gradle packages
    yum search gradle

##### Make an installation directory for Gradle
    sudo mkdir -p /opt/packages/gradle && cd $_
    pwd

##### Download Gradle 2.10
    sudo wget https://services.gradle.org/distributions/gradle-2.10-bin.zip
    
##### Unzip the Gradle package
    sudo unzip gradle-2.10-bin.zip
    ls

##### Create a symlink 
    sudo ln -s /opt/packages/gradle/gradle-2.10/ /opt/gradle

##### Set GRADLE_HOME
    export GRADLE_HOME="/opt/gradle"

##### Set the PATH variable
    PATH="$PATH:$GRADLE_HOME/bin"

##### See if Gradle is installed correctly
    gradle -version

##### Add GRADLE_HOME/bin to your bash profile script
    nano ~/.bash_profile
```bash
# Gradle
if [ -d "$HOME/opt/gradle" ]; then
    export GRADLE_HOME="/opt/gradle"
    PATH="$PATH:$GRADLE_HOME/bin"
fi
```

[RETURN](../README.md)
