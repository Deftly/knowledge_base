# Installing Maven
- Download the latest version of Maven from the Apache maven web site (https://maven.apache.org/download.cgi) or use your package manager to install it
	- Note Maven is a Java-based application and requires the Java Development Kit (JDK) to function properly. 
```bash
sudo dnf install maven
```


# Testing Installation
- To verify the installation was successful run the following command
```bash
mvn -v
```
- You should see something similar to the following:
```bash
Apache Maven 3.6.3 (Red Hat 3.6.3-13)
Maven home: /usr/share/maven
Java version: 17.0.2, vendor: Oracle Corporation, runtime: /usr/java/jdk-17.0.2
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "5.16.19-200.fc35.x86_64", arch: "amd64", family: "unix"
```


# Additional Settings
- User-specific configuration is provided in a settings.xml file which Maven will look for in two locations - in the conf directory of Maven's installation(*/etc/maven*) and .m2 directory in the user's home. 
	- The settings.xml under the conf directory is known as the global settings, and the file under .m2 is referred to as the user settings. If both files exist, Maven will merge the contents of the two files the user settings will take precedence.

## *settings.xml* Elements 
| Element Name    | Description                                                                                                                                                                                                                                                                                                     |
| --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| localRepository | Maven stores copies of plug-ins and dependencies locally under *~/.m2/repository*. The localRepository element can be used to change the path of the local repository                                                                                                                                           |
| interactiveMode | When this value is set to *true*, Maven interacts with the user for input. If the value is *false*, Maven will try to use sensible defaults.                                                                                                                                                                    |
| offline         | When set to *true*, this configuration instructs Maven to not connect to network and operate in an offline mode. When set to true Maven will not attempt to download new dependencies or updates to dependencies                                                                                                |
| servers         | Maven can interact with a variety of servers, such as Git servers, build servers, and remote repository servers. This element allows you to specify security credentials needed to connect to those servers.                                                                                                    |
| mirrors         | Allows you to specify alternate locations for downloading dependencies from remote repositories. For example your Org. might have mirrored a public repository on their internal network. The mirror element allows you to force Maven to use the internal mirrored repository instead of the public repository |
| proxies         | Contain the HTTP proxy information needed to connect to the Internet                                                                                                                                                                                                                                            |

# Setting up a Proxy
- Maven requires an Internet connection to download plug-ins and dependencies (discussed in detail in [[3 Maven Dependency Management]]). Some organizations use HTTP proxies to restrict access to the Internet. 
	- In these scenarios running Maven will result in *Unable to download artifact* errors. To address this, edit the settings.xml and add the proxy information specific to your organization. Below is a sample configuration
```xml
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
						http://maven.apache.org/xsd/settings-1.0.0.xsd">
	<proxies>
		<proxy>
			<id>companyProxy</id>
			<active>true</active>
			<protocol>http</protocol>
			<host>proxy.company.com</host>
			<port>8080</port>
			<username>proxyusername</username>
			<password>proxypassword</password>
			<nonProxyHosts/>
		</proxy>
	</proxies>
</settings>
```
# Securing Passwords
- Previously we saw the password to connect to the proxy server was stored in plain text in the settings.xml file which can be easily compromised. To address this, Maven provides a mechanism to encrypt the passwords that get stored in settings.xml  
- To start the encryption process create a master password using the following:
```bash
mvn -emp mymasterpassword
{LCWw0+NAqw0HuYH9HNz+1D7aElXM242PtuyoDXDAuelxjwZC8MyXaACkHSy7tZwU}
```
- Maven requires the newly generated master password to be saved in a settings-security.xml file under the .m2 directory. 
```xml
<settingsSecurity>
<master>{LCWw0+NAqw0HuYH9HNz+1D7aElXM242PtuyoDXDAuelxjwZC8MyXaACkHSy7tZwU}</master>
</settingsSecurity>
```
- Run the following command to encrypt the "proxypassword". Once the command completes, copy the output and replace the clear text password in the settings.xml.
```bash
mvn -ep proxypassword
{i4RnaIHgxqgHyKYySxor+csvhmHweTAvNjuORNYyu5w+}
```
# Summary
- In this chapter we covered that Maven downloads the plug-ins and artifacts needed for its operation.
- These artifacts are stored in the *~/.m2/repository* directory
- The *~/.m2* repository also contains the settings.xml file which can be used to configure Maven's behavior.

### Continue Reading
[[3 Maven Dependency Management]]
[[1 Getting Started with Maven]]
[[Introducing Maven table of contents]]