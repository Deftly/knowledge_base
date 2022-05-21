# Maven Dependency Management
- Enterprise level projects typically depend on a variety of open source libraries. 
- For example, if you want to include [[Log4J]] for application logging you would go to the Log4J download page and download the JAR file and put it in your project lib folder or add it to the project's class path. 
- There are a few problems with this approach:
	1. The JAR file you download might depend on a few other libraries. You would have to hunt down all of those dependencies (and their dependencies) and add them to your project.
	2. When the time comes to update the JAR file, you would need to start the process all over again.
	3. You need to add JAR files to source control along with your source code so that your projects can be built on a computer other than your own. This increases project size, checkout, and built time.
	4. Sharing JAR files across teams within your organization becomes difficult.

To address these problems, Maven provides declarative dependency management. You declare your project's dependencies in an external pom.xml file and Maven will automatically download those dependencies.  

![[Maven Dependency Management.png]]

- When you run a Maven project for the first time Maven connects to the network and downloads artifacts and related metadata from remote repositories. The default remote repository is called *Maven Central* and it is located at https://repo.maven.apache.org/. 
- Maven places a copy of these downloaded artifacts in its local repository and on subsequent runs will check the local repository first before attempting to download them from a remote repository.
- The above architecture works in the majority of cases but it does pose a few problems in an enterprise environment.
	1. The sharing of company-related artifacts between teams is not possible. Because of security and IP concerns you wouldn't want to publish your enterprise's artifacts on Maven Central.
	2. Your organization might want teams to only use officially approved open source software.
	3. In times of heavy load on Maven Central, the download speeds of Maven artifacts are reduced which might have a negative impact on your builds.
  
![[Pasted image 20220521130554.png]]
- The internal repository manager acts as a proxy to remote repositories, caching artifacts resulting in faster artifact downloads and build performance improvements. 
- Additionally because you have control over the internal repository you can regulate the types of artifacts allowed in your organization. 
- You can also push your organization's artifacts onto the repository manager, thereby enabling collaboration.

### Open Source Repository managers
| Repository Manager      | URL                                                |
| ----------------------- | -------------------------------------------------- |
| Nexus Repository OSS    | https://www.sonatype.com/products/nexus-repository |
| Apache Archiva          | https://archiva.apache.org/                        |
| Artifactory Open Source | https://jfrog.com/community/open-source/                                                   |

## Using New Repositories
