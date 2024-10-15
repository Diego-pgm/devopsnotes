# Application Basics

## Java

- Has been one of the most popular programming languages widely used for building desktop, mobile and web applications.

- Free

- Open-source

- Huge Community

### Install Java

- Get the tar package from `jdk.java.net` (Current version jdk-23).
```
wget https://download.java.net/java/GA/jdk23.0.1/c28985cbf10d4e648e4004050f8781aa/11/GPL/openjdk-23.0.1_linux-aarch64_bin.tar.gz
```

- Decompress the package.
```
tar -xvf openjdk-23.0.1_linux-aarch64_bin.tar.gz -C /opt/
```

- Change the directory and check version
```
jdk-23.0.1/bin/java -version
```

- Set java binary path in environment `PATH`.
```
export PATH=$PATH:/opt/jdk-23/bin
```

### Java Development Kit (JDK)

- It is a set of tools that help develop, build and run java apps.

- Develop tools:
  - jdb
  - javadoc

- Build tools:
  - javac
  - jar

- Run tools:
  - java
  - JRE (Java Runtime Environment)


### Simple Java App

1. Develop Source Code

MyClass.java
```
public class MyClass {
    public static void main(String[] args){
        System.out.println("Hello World!");
    }
}
```

2. Compile
```
javac MyClass.java
```
MyClass.class

3. Run
```
java MyClass
```

- This is for compiling just one file

### Package

- A typical application may have many files.

- app
  - MyClass.class
  - Service1.class
  - Service2.class
  - Utility.class
  - Tools.class

- They may be dependent on each other or may have dependencies on external libraries.

- To distribute the app to end user we need to package it up and that's where an archive like `JAR` can help.

- This will package all the `classes` and `dependencies` in a single file `JAR` (Java Archive) and if the application has static pages or images, it will package it in a `war` file (Web Archive).

**Package classes**
```
jar cf MyApp.jar MyClass.class Service1.class Service2.class
```

- This package can be run in any system that `has the jre installed`.

```
java -jar MyApp.jar
```

### Document 

- Our code also needs to be documented so that others can easily read and understand the various functionalities.

```
javadoc -d doc MyClass.java
```

### Build tools

- All these steps (developing, compiling, packaging and running) are made manually and when the application increases its classes, and number of people working on it it can be difficult to keep track.

- Build tools can help automate much of these processes.

- Build tools for Java:
  - Maven
  - Gradle
  - ANT

```
javac MyClass.java

javadoc MyClass.java

jar cf MyClass.jar
```

- These steps can be converted into an `ant build` configuration file, which is in an XML format.

```
<?xml version="1.0"?>
<project name="Ant" default="main" basedir=".">
  <target name="compile">
    <javac srcdir="/app/src" destdir="/app/build">
    </javac>
  </target>
  <target name="docs" depends="compile">
    <javadoc packagenames="src" sourcepath="/app/src", destdir="/app/docs">
      <fileset dir="/app/src">
        <include name="*"/>
      </fileset>
    </javadoc>
  </target>
  <target name="jar" depends="compile">
    <jar basedir="/app/build" destfile="/app/dist/MyClass.jar">
      <manifest>
        <attribure name="Main-Class" value="MyClass" />
      </manifest>
    </jar>
  </target>
  <target name="main" depends="compile, jar, docs">
    <description>Main target</description>
  </target>
</project>
```

- Run `ant` command.
```
ant
```