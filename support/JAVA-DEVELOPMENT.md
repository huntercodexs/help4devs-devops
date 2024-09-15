
# JAVA DEVELOPMENT + MAVEN

### Use Maven Plugin

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
                            <mainClass>com.huntercodexs.hardsys.Application</mainClass>
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

