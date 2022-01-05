# pavlab-starter-parent

Curated dependencies for the Pavlidis Lab's Java applications

## Core dependencies

Here's the core dependencies that are managed by this POM:

 - [baseCode](https://github.com/pavlidisLab/baseCode)
 - Hibernate 3.6.10
 - MySQL
 - SLF4J or Apache Commons Logging APIs with Log4j as backend
 - JUnit 4, Mockito and AssertJ for testing
 - Lombok
 - Tomcat 8.5 with corresponding vendor-agnostic dependencies (JSP, Servlet 3.1 API)
 - Ehcache 2.4.3
 - [Git-Flow Maven Plugin](https://github.com/aleksandr-m/gitflow-maven-plugin)

## Setup

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>ubc.pavlab</groupId>
        <artifactId>pavlab-starter-parent</artifactId>
        <version>1.0</version>
    </parent>

    <!-- add your groupId, artifactId, etc. -->

    <!-- our POMs are not yet part of Maven Central, so you will need the following entry -->
    <repositories>
        <repository>
            <id>pavlab</id>
            <name>PavLab</name>
            <url>https://maven2.pavlab.msl.ubc.ca/</url>
        </repository>
    </repositories>
</project>
```
