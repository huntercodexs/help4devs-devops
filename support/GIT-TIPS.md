
# GIT TIPS

##### Commit configuration user

<pre>
git config --global user.name "Hunter Codexs"
git config --global user.email "huntercodexs@gmail.com"
git config --list
</pre>

##### Clone repository

<pre>
git clone https://github.com/{USERNAME_GITHUB}/{REPOSITORY_NAME}
</pre>

##### Check repository status

<pre>
git status
</pre>

##### Check the remote target in the current repository

<pre>
git remote -v
</pre>

##### Change branch

<pre>
git checkout {BRANCH_NAME}
</pre>

##### Check the branch name

<pre>
git branch
</pre>

##### Add changes to the local stage

<pre>
git add .
</pre>

##### Create one commit to send the changes from the local stage to remote origin

<pre>
#Sample
git commit -m 'feat+data: [no-stabled] [work] Added database files to sample tests using API (PHP) + grid component repair'
</pre>

##### Send the changes to remote repository

<pre>
git push
</pre>

##### Synchronize the local repository with remote

<pre>
git pull
</pre>

##### Undo the changes made accidentally or that can broken anything after that - RESET

<pre>
git filter-branch --index-filter 'git rm --cached --ignore-unmatch dbhandler/ipch'
git rm --cached dbhandler/ipch
git commit --amend
git filter-branch ... 0123abcd..HEAD
git reset HEAD
git push -f
</pre>

##### To merge with another branch (it means that the informed branch will be merged in the current branch)

<pre>
git merge {BRANCH_NAME_TO_MERGE}
</pre>

##### Change the repository URL

<pre>
git remote remove origin
git remote add origin https://github.com/{USERNAME_GITHUB}/{REPOSITORY_NAME}
git remote set-url origin https://github.com/{USERNAME_GITHUB}/{REPOSITORY_NAME}
</pre>

##### Create new branch during the push

<pre>
git push --set-upstream origin devel-150621
</pre>

##### Change the commit message after the push command

<pre>
git commit --amend
</pre>

##### Publishing Packages - maven.pkg.github.com

- Create one personal access token
  - GitHub Account > Settings > Developers settings > Personal access token
    - Generate the token with the "write:packages" permissions

- Configure the settings.xml file for Maven

<pre>
/home/{USER}/.m2/settings.xml
</pre>

<code>

    <settings xmlns="http://maven.apache.org/SETTINGS/1.0.0"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0
        http://maven.apache.org/xsd/settings-1.0.0.xsd">
    
        <activeProfiles>
            <activeProfile>github</activeProfile>
        </activeProfiles>
    
        <profiles>
            <profile>
            <id>github</id>
            <repositories>
                <repository>
                <id>central</id>
                <url>https://repo1.maven.org/maven2</url>
                </repository>
                <repository>
                <id>github</id>
                <url>https://maven.pkg.github.com/{GITHUB-ACCOUNT}/{GITHUB-REPOSITORY}</url>
                <snapshots>
                    <enabled>true</enabled>
                </snapshots>
                </repository>
            </repositories>
            </profile>
        </profiles>
    
        <servers>
            <server>
            <id>github</id>
            <username>{GITHUB-ACCOUNT}</username>
            <password>{GITHUB-PERSONAL-ACCESS-TOKEN}</password>
            </server>
        </servers>
    </settings>

</code>

- Crete one java project and configure the pom.xml as something like below

<code>

    <?xml version="1.0" encoding="UTF-8"?>
    <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    
        <modelVersion>4.0.0</modelVersion>
    
        <parent>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-parent</artifactId>
            <version>2.6.4</version>
            <relativePath/> <!-- lookup parent from repository -->
        </parent>
        
        <packaging>jar</packaging>
    
        <version>{VERSION-PACKAGE}</version>
        <groupId>{DOMAIN-PACKAGE}</groupId>
    
        <name>{PACKAGE-NAME}</name>
        <artifactId>{PACKAGE-ID}</artifactId>
        <description>{PACKAGE-DESCRIPTION}</description>
    
        <properties>
            <java.version>{JAVA-VERSION}</java.version>
            <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
            <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        </properties>
    
        <dependencies>
            ...
        </dependencies>
    
        <repositories>
            ...
        </repositories>
    
        <pluginRepositories>
            ...
        </pluginRepositories>
    
        <build>
            <plugins>
    
                <!--Generate Final Jar with all dependency-->
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
    
        <distributionManagement>
            <repository>
                <id>github</id>
                <url>https://maven.pkg.github.com/{GITHUB-ACCOUNT}/{GITHUB-REPOSITORY}</url>
            </repository>
        </distributionManagement>
    
    </project>

</code>

- Execute the maven command to deploy

<pre>
mvn deploy
</pre>

- Now import the package in another java project, for example:

<code>

    <dependency>
        <groupId>com.huntercodexs</groupId>
        <artifactId>codexstester</artifactId>
        <version>1.0.9-SNAPSHOT</version>
        <scope>provided</scope>
        <exclusions>
            <exclusion>
                <groupId>com.vaadin.external.google</groupId>
                <artifactId>android-json</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

</code>

- OK, now it is possible to use the resources provided by the current imported package.

Enjoy !


