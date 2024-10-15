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

```
<?xml version="1.0"?>
<project name="Ant" default="main" basedir=".">
    <!-- Compiles the java code (including the usage of library for JUnit -->
    <target name="compile">
        <javac srcdir="/opt/ant/src" destdir="/opt/ant/build">
        </javac>
    </target>
    <!-- Creates Javadoc -->
    <target name="docs" depends="compile">
        <javadoc packagenames="src" sourcepath="/opt/ant/src" destdir="/opt/ant/docs">
            <!-- Define which files / directory should get included, we include all -->
            <fileset dir="/opt/ant/src">
                <include name="**" />
            </fileset>
        </javadoc>
    </target>
    <!--Creates the deployable jar file  -->
    <target name="jar" depends="compile">
        <jar basedir="/opt/ant/build" destfile="/opt/ant/dist/MyClass.jar" >
            <manifest>
                <attribute name="Main-Class" value="MyClass" />
            </manifest>
        </jar>
    </target>
    <target name="run" depends="jar">
      <java jar="/opt/ant/dist/MyClass.jar" fork="true" />
    </target>
    <target name="main" depends="compile, jar, docs, run">
        <description>Main target</description>
    </target>
</project>
```

### Maven

- Project Tree
.
├── pom.xml
└── src
    ├── main
    │   └── java
    │       └── com
    │           └── mycompany
    │               └── app
    │                   └── App.java
    └── test
        └── java
            └── com
                └── mycompany
                    └── app
                        └── AppTest.java


`App.java`
```
package com.mycompany.app;

/**
 * Hello world!
 *
 */
public class App 
{
    public static void main( String[] args )
    {
        System.out.println( "Hello World!" );
    }
}
```

`AppTest.java`
```
package com.mycompany.app;

import static org.junit.Assert.assertTrue;

import org.junit.Test;

/**
 * Unit test for simple App.
 */
public class AppTest 
{
    /**
     * Rigorous Test :-)
     */
    @Test
    public void shouldAnswerWithTrue()
    {
        assertTrue( true );
    }
}
```


pom.xml
```
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.mycompany.app</groupId>
  <artifactId>my-app</artifactId>
  <version>1.0-SNAPSHOT</version>

  <name>my-app</name>
  <!-- FIXME change it to the project's website -->
  <url>http://www.example.com</url>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.7</maven.compiler.source>
    <maven.compiler.target>1.7</maven.compiler.target>
  </properties>

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>

  <build>
    <pluginManagement><!-- lock down plugins versions to avoid using Maven defaults (may be moved to parent pom) -->
      <plugins>
        <!-- clean lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#clean_Lifecycle -->
        <plugin>
          <artifactId>maven-clean-plugin</artifactId>
          <version>3.1.0</version>
        </plugin>
        <!-- default lifecycle, jar packaging: see https://maven.apache.org/ref/current/maven-core/default-bindings.html#Plugin_bindings_for_jar_packaging -->
        <plugin>
          <artifactId>maven-resources-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.8.0</version>
        </plugin>
        <plugin>
          <artifactId>maven-surefire-plugin</artifactId>
          <version>2.22.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-jar-plugin</artifactId>
          <version>3.0.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-install-plugin</artifactId>
          <version>2.5.2</version>
        </plugin>
        <plugin>
          <artifactId>maven-deploy-plugin</artifactId>
          <version>2.8.2</version>
        </plugin>
        <!-- site lifecycle, see https://maven.apache.org/ref/current/maven-core/lifecycles.html#site_Lifecycle -->
        <plugin>
          <artifactId>maven-site-plugin</artifactId>
          <version>3.7.1</version>
        </plugin>
        <plugin>
          <artifactId>maven-project-info-reports-plugin</artifactId>
          <version>3.0.0</version>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>
</project>
```