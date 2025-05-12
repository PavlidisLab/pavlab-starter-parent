# pavlab-starter-parent

Curated dependencies for the Pavlidis Lab's Java applications

## Core dependencies

Here's the core dependencies that are managed by this POM:

 - Java 8+
 - Maven 3.0.5 or newer
 - [baseCode](https://github.com/pavlidisLab/baseCode)
 - Hibernate 4.2
 - MySQL
 - SLF4J or Apache Commons Logging APIs with Log4j 2 as backend
 - Log4j 2 with 1.2 compatibility API
 - JUnit 4, Mockito and AssertJ for testing
 - Lombok
 - Tomcat 8.5 with corresponding vendor-agnostic dependencies (JSP, Servlet 3.1 API)
 - Ehcache 2.4
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
        <version>1.2.1</version>
    </parent>

    <!-- add your groupId, artifactId, etc. -->
    <artifactId>foo</artifactId>

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

    <!-- Unfortunately, it's not possible to define those in a parent POM -->
    <distributionManagement>
        <site>
            <id>pavlab</id>
            <url>scpexe://frink.msl.ubc.ca/space/web/maven-sites/${project.groupId}/${project.artifactId}-${project.version}</url>
        </site>
    </distributionManagement>
    <profiles>
        <profile>
            <!-- For deployment where host is local (and ssh isn't available for builder, e.g. CI) -->
            <id>local-deploy</id>
            <distributionManagement>
                <site>
                    <id>pavlab</id>
                    <url>file:///space/web/maven-sites/${project.groupId}/${project.artifactId}-${project.version}</url>
                </site>
            </distributionManagement>
        </profile>
    </profiles>
</project>
```

For the SSH-based deployment, you will need to know the server configuration in
your `~/.m2/settings.xml`:

```xml
<settings>
    <servers>
        <server>
            <id>pavlab</id>
            <username>{username}</username>
            <privateKey>/path/to/your/home/.ssh/id_rsa</privateKey>
            <!-- ensure that artifacts are group-writable -->
            <filePermissions>664</filePermissions>
            <directoryPermissions>775</directoryPermissions>
        </server>
    </servers>
</settings>
```

If you are building from one of our servers, you can avoid using SSH altogether
with the `local-deploy` Maven profile (activated via `-Plocal-deploy`).

## Hibernate

To include Hibernate, ensure that you also include `dom4j`, `javassist`, `jboss-logging`
and `jboss-logging-annotations` artifacts as we define up-to-date dependencies to support Java 9+ and address
security vulnerabilities.

```xml
<build>
    <dependency>
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-core</artifactId>
    </dependency>
    <dependency>
        <groupId>org.dom4j</groupId>
        <artifactId>dom4j</artifactId>
    </dependency>
    <dependency>
        <groupId>org.javassist</groupId>
        <artifactId>javassist</artifactId>
    </dependency>
    <dependency>
        <groupId>org.jboss.logging</groupId>
        <artifactId>jboss-logging</artifactId>
    </dependency>
    <dependency>
        <groupId>org.jboss.logging</groupId>
        <artifactId>jboss-logging-annotations</artifactId>
    </dependency>
</build>
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

## Releasing

When ready to perform a release, activate the `release` profile (with `-Prelease`),
this will include source and javadocs JAR packaging.

```bash
mvn package -Prelease
```

## Deploying

To deploy, use the `deploy` lifecycle.

```bash
mvn deploy -Prelease
```

To deploy a Maven site, use the `deploy-site` lifecycle.

```bash
mvn deploy-site
```

## Updating dependencies

Due to the Maven 3.0.5 baseline requirement, it's not possible to use advanced
filtering features from the versions-maven-plugin that makes l

Use the `depcheck` profile to enable.
