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


# Step-By-Step Procedures

## Step 1: Create Spring project from Spring Intializr
Go to the [Spring Initializer](https://start.spring.io/)
- Choose "Maven Project", Language "Java" and Spring Boot version "2.6.3"
- Group: type "shop"
- Artifact: type “shopApp”
- Name: type “shopApp”
- Description: type any description
- Choose “Jar”, it will include embedded Tomcat server provided by Spring Boot
- Choose Java SDK 11

Add the following Dependencies
- Spring Web: required for RESTful web applications
- Spring Data JPA: required to access the data from the database. JPA (Java Persistence API) 
- PostgreSQL Driver: required to connect with PostgreSQL database
- Thymeleaf Driver: Thymeleaf is a Java-based library provides a good support for XHTML/HTML5 in web applications

<img width="1219" alt="Screenshot1" src="https://user-images.githubusercontent.com/48862763/151650813-c310bf0b-517a-49fc-80ff-fedfa662ed81.png">










