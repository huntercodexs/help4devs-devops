# MAVEN HELPER

## Deploy one jar file to the Central Maven

### Account

- Create one account in the sonatype website: https://central.sonatype.com/
  - It is possible to use any social media like GitHUB and Google account to make a login

### Namespace

- Claim for a namespace that you have and that you are de owner, for example: com.huntercodexs
- Look up for "View Namespaces" section in the menu of maven website and click on it
- Click in the "Verify Namespace" to verify the created namespace
  - In this point, if you create the account using GitHUB, the process will be made automatically to verify the own and trust of the current namespace
    - the namespace it will be something like: io.github.com.username
  - If you have created a new account without social media, or want to create a new namespace, follow the instructions:
    - Get the value generated for TXT Record in the maven panel;
    - Go to the hosting service and create a record usgin this value, for example:
      - AWS Route 53;
      - GoDaddy;
      - Hostinger;
- AWS Route 53 example

![sonatype-maven-create-txt-record.png](midias/images/sonatype-maven-create-txt-record.png)

- At this point the namespace verify should be Verified

![sonatype-maven-namespace-verified.png](midias/images/sonatype-maven-namespace-verified.png)

### Requirements

<code>

    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

        <modelVersion>4.0.0</modelVersion>
            
        <packaging>jar</packaging>
        <groupId>{PACKAGE-DOMAIN}</groupId>
        <artifactId>{PACKAGE-ID}</artifactId>
        <version>{PACKAGE-VERSION}</version>
        
        <name>{PACKAGE-NAME}</name>
        <description>{PACKAGE-DESCRIPTION}</description>
        <url>{PACKAGE-URL:GITHUB-REPOSITORY}</url>
        
        <licenses>
            <license>
                <name>The Apache Software License, Version 2.0</name>
                <url>http://www.apache.org/licenses/LICENSE-2.0.txt</url>
            </license>
        </licenses>
        
        <developers>
            <developer>
                <name>{DEVELOPER-NAME}</name>
                <email>{DEVELOPER-EMAIL}</email>
                <organization>{DEVELOPER-ORGANIZARION}</organization>
                <organizationUrl>{DEVELOPER-WEBSITE-URL}</organizationUrl>
            </developer>
        </developers>
        
        <scm>
            <connection>{SCM-CONNECTION}</connection>
            <developerConnection>{SCM-SSH-CONNECTION-URL}</developerConnection>
            <url>{BRANCH-MASTER-GITHUB-URL}</url>
        </scm>
        
        ...
    
    </project>

</code>

### GPG Keys

### MAVEN Settings

- Configure the settings.xml file for Maven

<pre>
/home/{USER}/.m2/settings.xml
</pre>

<code>

    <settings>
        <localRepository>/maven-repo</localRepository>
        <servers>
            <server>
                <id>ossrh</id>
                <username>{OSSRH-USERNAME}</username>
                <password>{OSSRH-PASSWORD}</password>
            </server>
        </servers>
            <profiles>
                <profile>
                    <id>ossrh</id>
                    <activation>
                        <activeByDefault>true</activeByDefault>
                    </activation>
                    <properties>
                        <gpg.executable>gpg2</gpg.executable>
                        <gpg.passphrase>{GPG-PASSPHRASE}</gpg.passphrase>
                    </properties>
                </profile>
        </profiles>
    </settings>

</code>

### Project POM Configuration

#### Distribution

<code>

    <distributionManagement>
        <snapshotRepository>
            <id>ossrh</id>
            <url>https://oss.sonatype.org/content/repositories/snapshots</url>
        </snapshotRepository>
        <repository>
            <id>ossrh</id>
            <url>https://oss.sonatype.org/service/local/staging/deploy/maven2/</url>
        </repository>
    </distributionManagement>

</code>

#### Maven Plugins

- Maven Nexus Plugin

<code>

    <plugin>
        <groupId>org.sonatype.plugins</groupId>
        <artifactId>nexus-staging-maven-plugin</artifactId>
        <version>1.6.7</version>
        <extensions>true</extensions>
        <configuration>
            <serverId>ossrh</serverId>
            <nexusUrl>https://oss.sonatype.org/</nexusUrl>
            <autoReleaseAfterClose>true</autoReleaseAfterClose>
        </configuration>
    </plugin> 

</code>

- Maven Source Plugin

<code>

    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-javadoc-plugin</artifactId>
        <version>2.9.1</version>
        <executions>
            <execution>
                <id>attach-javadocs</id>
                <goals>
                    <goal>jar</goal>
                </goals>
            </execution>
        </executions>
    </plugin>

</code>

- Maven JavaDoc Plugin

<code>

    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-javadoc-plugin</artifactId>
        <version>2.9.1</version>
        <executions>
            <execution>
                <id>attach-javadocs</id>
                <goals>
                    <goal>jar</goal>
                </goals>
            </execution>
        </executions>
    </plugin>

</code>

- Maven GPG Plugin

<code>

    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-gpg-plugin</artifactId>
        <version>1.5</version>
        <executions>
            <execution>
                <id>sign-artifacts</id>
                <phase>verify</phase>
                <goals>
                    <goal>sign</goal>
                </goals>
            </execution>
        </executions>
    </plugin>

</code>

### Project Deploy

<pre>
mvn clean deploy
</pre>