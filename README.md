# pavlab-starter-parent

Curated dependencies for the Pavlidis Lab's Java applications

## Core dependencies

Here's the core dependencies that are managed by this POM:

 - Java 8+
 - Maven 3.0.5 or newer
 - [baseCode](https://github.com/pavlidisLab/baseCode)
 - Hibernate 3.6.10
 - MySQL
 - SLF4J or Apache Commons Logging APIs with Log4j as backend
 - Log4j 1.2 compatibility API
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
        <version>1.1.0</version>
    </parent>

    <!-- add your groupId, artifactId, etc. -->

    <!-- Enforce minimal Maven version (optional) -->
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-enforcer-plugin</artifactId>
            </plugin>
        </plugins>
    </build>

    <!-- Our POMs are not yet part of Maven Central, so you will need the following entry -->
    <repositories>
        <repository>
            <id>pavlab</id>
            <name>PavLab</name>
            <url>https://maven2.pavlab.msl.ubc.ca/</url>
        </repository>
    </repositories>
</project>
```

## Quick note if you want to use Log4j 1.2 compatibility API

There might be some good reasons for still using Log4j 1.2 compatibility mode
with Log4j 2.

To honor any existing `log4j.properties` configuration, you need to set
`-Dlog4j1.compatibility=true` somewhere in your application deployment and test
JVM flags.

For Tomcat 8.5, add it to `$CATALINA_OPTS` under `bin/setenv.sh`.

For a CLI application using Maven Appassembler, add it to
`<extraJvmArguments/>` configuration property.

For tests with Maven Surefire, add it to `<argLine/>` configuration property.
