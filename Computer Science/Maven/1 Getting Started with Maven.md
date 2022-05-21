# What is Maven?
- Apache Maven is an open source, standards based project management framework that simplifies the building, testing, reporting, and packaging of projects.
- [[Maven's initial roots]] were in the Apache Jakarta Alexandria project and subsequently used in the Apache Turbine project. 
- Maven has become one of the most widely used open source software programs around the world, here are a few reasons why Maven is so popular


# Standardized Directory Structure
- Often when starting a new project a considerable amount of time is spent deciding on the project layout and folder structure needed to store code and config files. 
	- This can make it hard for new developers to understand and adopt other teams' projects as well as for existing developers to jump between projects and find what they're looking for.
- Maven addresses these problems by standardizing the folder structure and organization of a project. Maven provides recommendations on where different parts of a project, such as source code, test code, and config files should reside. 
	- Ex: Maven suggests that all the Java source code should be placed in the *src/main/java* folder


# Declarative Dependency Management
- Most Java projects rely on other projects and frameworks to function properly, downloading these dependencies and keeping track of their versions manually can be cumbersome.
- Maven provides a convenient way to declare these project dependencies in a separate *pom.xml* file. It then automatically downloads those dependencies and allows you to use them in your project.
	- It is important to not that in the *pom.xml* you specify the *what* and not the *how*. The *pom.xml* can also serve as a documentation tool, conveying your project dependencies and their versions.


# Plug-ins
- Maven follows a *plug-in-based architecture*, making it easy to augment and customize its functionality. 
	- Plug-ins encapsulate reusable build and task logic. Today there are hundred of Maven plug-ins available that can be used to carry out tasks ranging from code compilation to packaging to project documentation generation.
- Maven also makes it easy to create your own plug-ins enabling you to integrate tasks and workflows specific to you or your organization.


# Uniform Build Abstraction
- Maven provides a uniform interface for building projects. Builds can be done using just a handful of commands and once you become familiar with the Maven's build process you can easily build other Maven projects. 


# Tools Support
* Maven provides a powerful command line interface to carry out different operations.
*  All major IDEs today provide good tool support for Maven, additionally Maven is fully integrated with continuous integration products such as Jenkins.


# Archetypes
- Maven archetypes are predefined project templates that can be used to generate new projects. 
- Archetypes are also a valuable tool for bundling best practices and common assets that you will need in each of your projects.
	- Ex: All Spring based web projects share common dependencies and require a set of configuration files. It is also likely that all of these web projects have similar *[[Log4J]]/[[Logback]]* configuration files, *CSS/Images*, and [[Thymeleaf]] page layouts. Maven lets you bundle these common assets into an archetype. When new projects are created using this archetype, they will automatically have the common assets included.


# Open Source
- Maven is open source and costs nothing to download and use. It comes with rich online documentation and the support of an active community. 


# Maven Alternatives

## [[Ant]] + [[Ivy]]
- Apache Ant (http://ant.apache.org) is a popular open source, Java based tool for scripting builds that uses Extensible Markup Language(XML) for its configuration.
	- The default configuration file for Ant is the *build.xml* file
- Using Ant typically involves defining tasks and targets.
	- An *Ant task* is a unit of work that needs to be completed. Typical tasks involve creating a directory, running a test, compiling source code, building a web application archive(WAR) file, etc.
	- A *target* is simply a set of tasks. It is is possible for a target to depend on other targets. This dependency lets us sequence target execution.


- Below is a simple *build.xml* file with one target called *compile*. The *compile* target has two *echo* tasks and one *javac* task. 
```xml
<project name="Sample Build File" default="compile" basedir=".">
  <target name="compile" description="Compile Source Code">
    <echo message="Starting Code Compilation"/>
    <javac srcdir="src" destdir="dist"/>
    <echo message="Completed Code Compilation"/>
  </target>
</project>
```
- Ant doesn't impost any conventions or restrictions on your project, making it extremely flexible. However, this can sometimes lead to complex, hard to understand and maintain *build.xml* files.
---
- Apache Ivy (https://ant.apache.org/ivy/) provides automated dependency management, making Ant easier to use. With Ivy, you declare the dependencies in an XML file called *ivy.xml*, as show below:
```xml
<ivy-module version="2.0">
  <info organization="com.apress" module="gswm-ivy" />

  <dependencies>
	  <dependency org="org.apache.logging.log4j" name="log4j-api" rev="2.11.2" />
  </dependencies>
</ivy-module>
```

## [[Gradle]]
- Gradle (https://gradle.org/) is an open source build, project automation tool that can be used for Java and non-Java projects. 
- Unlike Ant and Maven, which use XML for configuration, Gradle uses a Groovy-based *domain-specific language* (DSL).
- Gradle provides the flexibility of Ant, and it uses the same notion of tasks, example below:
```java
plugins {
	id 'java'
}

version = '1.0.0'

repositories {
	mavenCentral()
}

dependencies {
	testCompileOnly group: 'junit', name: 'junit', version: '4.10'
}
```

- The Gradle DSL and its adherence to convention over configuration results in compact build files. 
- The flexibility and performance of Gradle have contributed to its growing popularity, however, the learning curve for Gradle and Groovy DSL can be steep. IDE and plug-in support for Gradle is also less mature when compared to Maven.

### Survey results for build tool usage
![[Build tool usage report.png]]

# Maven Components
- Maven relies on several components to get its job done. Though you might not interact with these components directly an overview of these components helps you understand the internal working of Maven ad better equip you to troubleshoot Maven errors.


# Maven SCM
- Maven interacts with several source control/code management (SCM) systems to perform operations such as checking out code or creating a branch or a tag. 
- The Maven SCM (https://maven.apache.org/scm/) project provides a common API to perform such operations.
- The Maven release plug-in discussed in [[8 Maven Release]] heavily relies on Maven SCM components. 
- Maven SCM currently supports several popular code management systems such as Git, Subversion, and Perforce.


# Maven Wagon
- Maven automatically download project dependencies such as JAR files from different locations such as [[FTP]]s, file systems, and web sites.
- The Maven Wagon (https://maven.apache.org/wagon/) project provides a transport abstraction that enables Maven to interact with different transport protocols easily and retrieve dependencies. Supported protocols include:
	- ***File***: Allows retrieval of dependencies using file system protocol.
	- ***HTTP***: Allows retrieval of dependencies using HTTP/HTTPS protocols.  Two implementation variations are provided -- one uses Apache HttpClient and the other uses the standard Java library.
	- ***FTP***: Allows retrieval of dependencies using File Transfer Protocol


# Maven Doxia
- Maven Doxia (https://maven.apache.org/doxia/) is a content generation framework that can be used to generate static/dynamic content such as PDF files and web pages. 
- Doxia supports several popular markup languages such as Markdown, Apt, XHTML, and Confluence.
- Maven relies heavily on Doxia to generate project documentation and reports, discussed further in [[7 Documentation and Reporting]]

### Continue Reading
[[2 Setting Up Maven]]
[[Introducing Maven table of contents]]