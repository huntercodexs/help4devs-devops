
# JAVA DEVELOPMENT + MAVEN

### Maven Plugins

###### Generate executable JAR file

Below it is possible to see a sample configuration to use in maven and get the JAR file easier. This plugin should 
be used when we need to generate a jar file executable in any machine or environment, it will be automatically set 
up the correct information in the MANIFEST file.

<code>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>3.3.0</version>
                <configuration>
                    <archive>
                        <manifest>
                            <addClasspath>true</addClasspath>
                            <mainClass>{{MAIN-CLASS}}</mainClass>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>
        </plugins>
    </build>

</code>

###### Include all dependencies in one single JAR file

To include all jar dependencies in one single file use the plugin described below, you can use this plugin configuration 
together others plugins, for example the plugin described above to generate an executable JAR file.

<code>

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
    </build>

</code>

or

<code>

    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.2.4</version>
                <configuration>
                    <createDependencyReducedPom>false</createDependencyReducedPom>
                    <finalName>{{FINAL-FILENAME}}</finalName>
                    <outputDirectory>./assets</outputDirectory>
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

###### Repackage with Build Info

<code>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<executions>
					<execution>
						<goals>
							<goal>repackage</goal>
							<goal>build-info</goal>
						</goals>
					</execution>
				</executions>
			</plugin>
		</plugins>
	</build>

</code>


###### Using together Spring Boot

<code>

	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<version>{{VERSION}}</version>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-resources-plugin</artifactId>
				<version>3.1.0</version>
			</plugin>
		</plugins>
	</build>

</code>

###### To exclude META-INF details

<code>

     <build>
        <plugins>
            <plugin>
                <artifactId>maven-surefire-plugin</artifactId>
                <version>3.2.2</version>
            </plugin>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-shade-plugin</artifactId>
                <version>3.5.1</version>
                <configuration>
                    <createDependencyReducedPom>false</createDependencyReducedPom>
                    <filters>
                        <filter>
                            <artifact>*:*</artifact>
                            <excludes>
                                <exclude>module-info.class</exclude>
                                <exclude>META-INF/*</exclude>
                                <exclude>META-INF/versions/**</exclude>
                                <exclude>META-INF/services/**</exclude>
                            </excludes>
                        </filter>
                    </filters>
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
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>3.11.0</version>
                <configuration>
                    <source>${maven.compiler.source}</source>
                    <target>${maven.compiler.target}</target>
                </configuration>
            </plugin>
        </plugins>
    </build>

</code>

###### Using Maven Deploy with Spring Boot Thin Layout

<code>

    <build>
		<plugins>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-deploy-plugin</artifactId>
				<configuration>
					<skip>true</skip>
				</configuration>
			</plugin>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<dependencies>
					<dependency>
						<groupId>org.springframework.boot.experimental</groupId>
						<artifactId>spring-boot-thin-layout</artifactId>
						<version>1.0.28.RELEASE</version>
					</dependency>
				</dependencies>
			</plugin>
			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-shade-plugin</artifactId>
				<version>3.2.4</version>
				<configuration>
					<createDependencyReducedPom>false</createDependencyReducedPom>
					<shadedArtifactAttached>true</shadedArtifactAttached>
					<shadedClassifierName>aws</shadedClassifierName>
				</configuration>
			</plugin>
		</plugins>
	</build>

</code>

###### Jar file build with copy dependencies

In this case the jar file will be generated in the target path separated of dependencies used in it that's in this 
specific case is called dependency-jars path.

<code>

    <build>
		<plugins>
			<plugin>
            <!--Make Jar executable with dependency class prefix-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-jar-plugin</artifactId>
                <version>2.4</version>
                <configuration>
                    <archive>
                        <manifest>
                            <addClasspath>true</addClasspath>
                            <mainClass>{{MAIN-CLASS}}</mainClass>
                            <classpathPrefix>dependency-jars/</classpathPrefix>
                        </manifest>
                    </archive>
                </configuration>
            </plugin>

            <!--Add dependencies to classpath-->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <version>2.5.1</version>
                <executions>
                    <execution>
                        <id>copy-dependencies</id>
                        <phase>package</phase>
                        <goals>
                            <goal>copy-dependencies</goal>
                        </goals>
                        <configuration>
                            <outputDirectory>
                                ${project.build.directory}/dependency-jars/
                            </outputDirectory>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

</code>

###### Removing assigned dependencies and pack all in one single jar file

Use when you receive the follow error: Invalid signature file digest for Manifest main attributes

<code>
    
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <version>2.6</version>
                <executions>
                    <execution>
                        <id>unpack-dependencies</id>
                        <phase>package</phase>
                        <goals>
                            <goal>unpack-dependencies</goal>
                        </goals>
                        <configuration>
                            <excludeScope>system</excludeScope>
                            <excludes>META-INF/*.SF,META-INF/*.DSA,META-INF/*.RSA</excludes>
                            <excludeGroupIds>junit,org.mockito,org.hamcrest</excludeGroupIds>
                            <outputDirectory>${project.build.directory}/classes</outputDirectory>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
        </plugins>
    </build>

</code>

###### Spring boot + Cucumber

<code>

        <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
                <configuration>
                    <testSourceDirectory>src/test/java</testSourceDirectory>
                    <excludes>
                        <exclude>**/CucumberTestRunner.java</exclude>
                    </excludes>
                </configuration>
                <executions>
                    <execution>
                        <id>integration-test</id>
                        <goals>
                            <goal>test</goal>
                        </goals>
                        <phase>integration-test</phase>
                        <configuration>
                            <excludes>
                                <exclude>none</exclude>
                            </excludes>
                            <includes>
                                <include>**/CucumberSuite.java</include>
                            </includes>
                        </configuration>
                    </execution>
                </executions>
            </plugin>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>

</code>

### Configure Maven Local Repository

When we need to import a library or dependency for our current Java project, and we just need to make it locally to 
avoid expend much time publishing in the central maven repository until we have sure about the status from working of  
the project, we can use the follow scenario to get that.

###### Create a Java Project

You can make it using any kind of IDE, I recommend IntelliJ for that, but fell free to choose any one of this. Go to 
create project, make the convenient settings and programmings, so after all use maven command to generate the jar file.

But be careful with the plugin that you are currently using for generate the jar file, that will be used as a dependency 
to import in another project. Below is a simple plugin configuration, named shade from maven apache, that create the 
complete and correct structure of folders, packages, namespaces, among others features for Java project:

<code>

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

So, when you finally have the final jar file by running the code like 

<pre>
maven install
</pre>

witch generate the jar file in the target output folder path in the current project, and that jar file can be shared 
with another Java project as a dependency.

For example, we are suppose that we have a project named as aws-lambda-demo, and that project was correctly programmed 
and configured to generate the jar file, so in order to afford that library for another project or developers we just 
need to send this jar file.

Now this jar file should be put in the specific local in the machine where the project consumer is running, for example:

<pre>
{M2-REPOSITORY}/{DOMAIN}/{PROJECT-NAME}/{PROJECT-VERSION}/{JAR-FILE}

/home/$USER/.m2/repository/com/huntercodexs/demo/aws-lambda-demo/${PROJECT-VERSION}/aws-lambda-demo-1.0-SNAPSHOT.jar
</pre>

In the above case the ${PROJECT-VERSION} is 1.0-SNAPSHOT, and the project name is aws-lambda-demo, so in the pom.xml 
file that is being used by the project consumer, we need to make the follow configurations to use that dependency:

<code>

    <dependency>
        <groupId>com.huntercodexs.demo</groupId>
        <artifactId>aws-lambda-demo</artifactId>
        <version>1.0-SNAPSHOT</version>
    </dependency>

</code>

Where

- groupId is the part of the path when the dependency was placed (.../com/huntercodexs/demo/...)
- artifactId is the name of the dependency (aws-lambda-demo)
- version is the folder where the jar file is placed

Also, it is possible to give others fields, such as scope or exclusion, but for now it is enough for our purpose. 

### Tips

In order to achieve a better result, keep in mind that it is very recommended to follow the rules below:

- Java Version: Ensure that the Java version is the same 
- Spring Boot Project: Not recommended to do that, but you can import a spring boot project in other one, but it can generate some troubles like dependency conflict, so be careful.
- Vulnerabilities: If you are using dependencies in the library make sure that the vulnerabilities are solved to avoid problems with third or partners.

Only for best clarify the things, we are offer one simple example of Java code to show how to use one resource from a
imported library, as follows:

<code>

    ...
    public class SimpleApiDemoApplication {

        public static void main(String[] args) {
    
            //Java Library Dependency
            Help4DevsAwsLambdaDemo help4DevsAwsLambdaDemo = new Help4DevsAwsLambdaDemo();
            System.out.println(help4DevsAwsLambdaDemo.handleRequestName("Huntercodexs"));
    
            SpringApplication.run(SimpleApiDemoApplication.class, args);
        }
    
    }

</code>

> There is another way to improve that scenario that consists of using NEXUS, and you can see how to configure and use 
> that one following the "NEXUS AND MAVEN" document placed in this quick devops repository.

