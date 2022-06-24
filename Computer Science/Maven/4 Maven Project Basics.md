# Maven Project Basics
- Maven provides conventions and a standard directory layout for all of its projects, this chapter will explain the basics of a Maven project and the *pom.xml* file.  

## Basic Project Organization
- Below is a bare-bones Maven-based Java project:
![[Maven Java project structure.png]]  

- The *gswm* is the root directory of the project. Typically the name of the root folder matches the name of the generated artifact.
- The *src* directory contains project-related artifacts such as source code or property files.
- The *src/main/java* directory contains the Java source code. 
- The *src/test/java* directory contains the Java unit test code.
- The *target* folder holds generated artifacts such as *.class* files.
- Every maven project has a *pom.xml* file at the root of the project. It holds the project and configuration information such as dependencies and plug-ins.

---
- In addition to the *src/main* and *src/test* directories, Maven recommends several other directories to hold additional files and resources.  

| Directory Name     | Description                                                                                                   |
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| src/main/resources | Holds resources, such as Spring Configuration files that need to end up in the generated artifact             |
| src/main/config    | Holds configuration files such as Tomcat context files. These files will not end up in the generated artifact |
| src/main/scripts   | Holds any scripts that system administrators and developers need for the application.                         |
| src/test/resources | Holds configuration files needed for testing.                                                                 |
| src/main/webapp    | Holds web assets such as *.jsp* files, style sheets, and images.                                              |
| src/it             | Holds integration tests for the application.                                                                  |
| src/main/db        | Holds database files such as [[SQL]] scripts.                                                                 |
| src/site           | Holds files required during the generation of the project site.                                               |                                                                                                               |

## Understanding the *pom.xml* File
- The *pom.xml* file is the most important file in a Maven project, it holds the configuration information needed by Maven. 
- We start the *pom.xml* file the *project* element, then we provide the *groupId*, *artifactId*, and *version* coordinates. The *packaging* element tell Maven that it needs to create the specified archive for this project. Finally, the *developers* element is used to add information about the developers who work on this project.
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" 
		 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
                             http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>

  <groupId>com.sid.mavennotes</groupId>
  <artifactId>mavenguide</artifactId>
  <version>1.0.0-SNAPSHOT</version>
  <packaging>jar</packaging>

  <name>Getting Started with Maven</name>
  <url></url>

  <developers>
    <developer>
      <id>sid</id>
      <name>Siddharth Buddharaju</name>
      <email>siddharth.buddharaju@gmail.com</email>
      <properties>
        <active>true</active>
      </properties>
    </developer>
  </developers>
</project>
```

#### Maven versioning
- It is recommended that Maven projects use the following conventions for versioning:
	{major-version}.{minor-version}.{incremental-version}-qualifier  

- The major, minor and incremental values are numeric and the qualifier can have values such as RC(release candidate), alpha, beta, and SNAPSHOT.
	- Ex: 1.0.0, 2.4.5-SNAPSHOT, 3.1.1-RC1  

- The SNAPSHOT qualifier in the project's version indicates that the project is in a development stage. When a project use a SNAPSHOT dependency, every time the project is built, Maven will fetch and use the latest SNAPSHOT artifact.  

- Most repository managers accept release builds once once. However, when developing in a continuous integration environment you want to build often and push your latest build to the repository manager so it is best practice to suffix your version with SNAPSHOT during development.

## Properties in pom.xml
