# DEPLOY HELPER
Make a deployment using one specific and easy way

## JENKINS + JAVA + SPRING BOOT + MAVEN + TOMCAT

### Pre-requisites

- GitHUB Repository
- Java Version 17
- Jenkins Version: 2.484
- Tomcat Version: 9.0.96
- Docker + Docker Compose
- Spring Boot 2.6.4

### Create Java Spring Boot Project

To optimize the time of development and tests to check this tutorial document use the "simple api demo" repository 
hosted in the GitHUB website in the following address: https://github.com/huntercodexs/simple-api-demo.git 
branch tomcat-release. After you have made the clone of this repository, check the required features in the Java 
Spring Boot project to run in the Tomcat Server.

- pom.xml file

<code>

    <packaging>war</packaging>
    ...
    ...
    ...
    <!--TOMCAT-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-tomcat</artifactId>
        <scope>provided</scope>
    </dependency>
    ...
    ...
    ...
	<build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-shade-plugin</artifactId>
				<version>3.2.4</version>
				<configuration>
					<createDependencyReducedPom>false</createDependencyReducedPom>
				</configuration>
				<executions>
					<execution>
						<phase>package</phase>
						<goals>
							<goal>shade</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
		</plugins>
	</build>

</code>

- main class java

<code>

    import org.springframework.boot.SpringApplication;
    import org.springframework.boot.autoconfigure.SpringBootApplication;
    import org.springframework.boot.builder.SpringApplicationBuilder;
    import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;
    
    @SpringBootApplication
    public class SimpleApiDemoApplication extends SpringBootServletInitializer {
    
        @Override
        protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
            return application.sources(SimpleApiDemoApplication.class);
        }
    
        public static void main(String[] args) {
            SpringApplication.run(SimpleApiDemoApplication.class, args);
        }
    
    }

</code>

In the above code we can see the extends for SpringBootServletInitializer and the @Override that serve to set up the 
java spring boot application to run in the tomcat server.

### Create GitHUB Repository

> NOTE: This tutorial offer a simple/sample repository to tests or study the jenkins workflow process, and it means 
> that you don't need to create and publish a new repository just for tests goals, however, if you want to improve 
> your skill over deployment, you should to create yur own repository and publish it in your GitHub account.

The information from GitHub account are:

- GitHub Project URL: https://github.com/huntercodexs/simple-api-demo.git
- GitHub Account Username: {USERNAME}
- GitHub Account Password: {PASSWORD}
- GitHub Account Branch: {BRANCH}

### Create Tomcat Container

To create a Tomcat container use the repository: https://github.com/huntercodexs/docker-series.git.
After you create the container, from the repository just mentioned, start the docker container using the following command

<pre>
docker-compose start
</pre>

Now you must make some changes in the Tomcat Container

- Fix the tomcat webapps to avoid 404 Not Found error

<pre>
docker exec -it tomcat /bin/bash
mv webapps webapps2
mv webapps.dist/ webapps
</pre>

- Change the webapps/manager/META-INF/context.xml and webapps/host-manager/META-INF/context.xml file in the follow way

<code>

    <Context antiResourceLocking="false" privileged="true">
        <!--<CookieProcessor className="org.apache.tomcat.util.http.Rfc6265CookieProcessor"
                         sameSiteCookies="strict" />-->
        <!--<Valve className="org.apache.catalina.valves.RemoteAddrValve"
               allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" />-->
        <!--<Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.(?:Linked)?HashMap"/>-->
    </Context>

</code>

- Change the conf/tomcat-users.xml file

<code>

    <tomcat-users xmlns="http://tomcat.apache.org/xml"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
              xsi:schemaLocation="http://tomcat.apache.org/xml tomcat-users.xsd"
              version="1.0">
        ...
        ...
        ...
        <role rolename="tomcat"/>
        <role rolename="admin"/>
        <role rolename="manager"/>
        <role rolename="robot"/>
    
        <user username="admin" password="admin" roles="manager-gui,admin-gui,manager-script"/>
        <user username="robot" password="robot" roles="manager-script"/>
        <user username="manager" password="manager" roles="manager-gui"/>
        <user username="robot" password="robot" roles="manager-script"/>
    </tomcat-users>

</code>

> TIP: The username robot is used to make deploys in the tomcat server

### Create Jenkins Docker Container

To create a Jenkins container use the repository: https://github.com/huntercodexs/docker-series.git.
After you create the container, from the repository just mentioned, start the docker container using the following command

<pre>
docker-compose start
</pre>

Goto the web browser and type the http://localhost:39090/, make the proper configurations or entry with your login in 
the login screen. After you get access to the Jenkins Dashboard, you can create one job to automate the process of deploy. 
In order to make it you need to follow the instructions below:

###### Plugin Deploy Install

One important step is to install a proper and required plugin to deploy projects inside a docker container, for that 
goto the jenkins dashboard and click on "Manage Jenkins", lookup for Plugins and click on it. 

In the Available plugins you should type "deploy" and choose the following plugin:

<pre>
Deploy to container Plugin Version 1.16
This plugin allows you to deploy a war to a container after a successful build.
Glassfish 3.x remote deployment
Report an issue with this plugin
</pre>

###### Maven Configuration

This is a separated and important step, follow this steps to achieve a real success in the process. Once you are logged 
in the jenkins dashboard, goto the http://localhost:39090/configureTools/ url and scroll down until Maven installation. 
Click on "Add Maven" and choose the Version for maven compiler, for example 3.6.3, also give a name for this Maven 
installation, for example MAVEN_HOME.

- Click on "New Item"
- Enter an item name, for example: simple-api-demo
- Select "Freestyle project" and click on "OK" button
- Fill the form to create a job
  - General
    - Description: SIMPLE API DEMO
    - GitHub project: https://github.com/huntercodexs/simple-api-demo.git
    - Source Code Management
      - Git
        - Repository URL: https://github.com/huntercodexs/simple-api-demo.git
        - Credentials: huntercodexs:password (the username and password of the github account)
        - Branch Specifier (blank for 'any'): */tomcat-release (for example: tomcat-release)
    - Triggers
      - Poll SCM: * * * * *
    - Environment
      - Delete workspace before build starts
    - Build Steps 
      - Invoke top-level Maven targets
        - Maven Version (this one should be configured previously - see the Maven Configuration above)
        - Goals (for example: clean compile package)
    - Post-build Actions
      - Deploy war/ear to a container
        - WAR/EAR files: **/*.war
        - Context path: simple-api-demo
        - Containers (Add container)
          - Credentials: robot:robot
          - Tomcat URL: http://localhost:38080/
  - Click on Apply and Save
- Now in the JOB dashboard click on "Build Now" to deploy the project


