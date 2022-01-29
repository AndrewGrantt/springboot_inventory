# Java Project -- Web Inventory System
## Start From Environmental Setup
1. SpringBoot 2.6.3
2. PostgreSQL 14
3. Tomcat 9.0.56 embedded in SpringBoot
4. Eclipse 2021-12
6. Java 11

## Client Library WebJars
WebJars are client side dependencies packaged into JAR files, add following dependency
1. WebJars bootstrap 5.1.3
2. WebJars jquery 3.6.0

```xml
<dependency>
		<groupId>org.webjars</groupId>
		<artifactId>bootstrap</artifactId>
		<version>5.1.3</version>
		</dependency>
		<dependency>
			<groupId>org.webjars</groupId>
			<artifactId>jquery</artifactId>
			<version>3.6.0</version>
		</dependency>
```

## Lombok installation in Eclipse
Lombok is a library that facilitates many tedious tasks and reduces Java source code verbosity<br>
1. Download Lombok jar file
2. Type "java -jar lombok-1.18.4.jar" in concole
3. Installation finish, then restart Eclipse
4. Project should be able to use @Data annotation now

