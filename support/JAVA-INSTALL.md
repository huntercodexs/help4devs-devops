
# JAVA INSTALL

- Java 8
- Java 11
- Java 17
- Java 21

### Ubuntu installation

You can use the following command to install a specific java version in the target machine

<pre>
sudo apt-get install openjdk-${VERSION}-jre-headless
</pre>


### Almalinux installation

The command to install specific java in the target machine based on RHEL/Fedora distribution is

<pre>
sudo yum install openjdk-${VERSION}-jre-headless
</pre>

### Generic Installation

> You can use the command below to make it. However, if you want to 
> keep more than one openjdk version in the same machine, please install 
> the openjdk manually, for example: 

- Get the jdk installer that normally is available like a tar.gz file

<pre>
jdk-11.0.1_linux-x64_bin.tar.gz
jdk-17.0.1_linux-x64_bin.tar.gz
jdk-21.0.1_linux-x64_bin.tar.gz
jdk-8u212-linux-x64.tar.gz     
</pre>

- Extract files

<pre>
tar -xvf jdk-8u212-linux-x64.tar.gz
jdk1.8.0_212
tar -xvf jdk-11.0.1_linux-x64_bin.tar.gz
jdk-11.0.1
tar -xvf jdk-17.0.1_linux-x64_bin.tar.gz
jdk-17.0.1
tar -xvf jdk-21.0.1_linux-x64_bin.tar.gz
jdk-21.0.1
</pre>

- Install each version in the separated path

<pre>
sudo mkdir -p /usr/share/jdk/
mv jdk1.8.0_212 jdk-11.0.1 jdk-17.0.1 jdk-21.0.1 /usr/share/jdk/
pwd
/usr/share/jdk
ls -ltr
total 16
drwxr-xr-x 7 almalinux-vbox almalinux-vbox 4096 abr  2  2019 jdk1.8.0_212
drwxrwxr-x 8 almalinux-vbox almalinux-vbox 4096 jun 25 17:45 jdk-11.0.1
drwxrwxr-x 9 almalinux-vbox almalinux-vbox 4096 jun 25 17:45 jdk-17.0.1
drwxrwxr-x 9 almalinux-vbox almalinux-vbox 4096 jun 25 17:45 jdk-21.0.1
</pre>

- Check java version for each package

<pre>
# Java 8
./jdk1.8.0_212/bin/java -version
java version "1.8.0_212"
Java(TM) SE Runtime Environment (build 1.8.0_212-b10)
Java HotSpot(TM) 64-Bit Server VM (build 25.212-b10, mixed mode)

# Java 11
./jdk-11.0.1/bin/java -version
java version "11.0.1" 2018-10-16 LTS
Java(TM) SE Runtime Environment 18.9 (build 11.0.1+13-LTS)
Java HotSpot(TM) 64-Bit Server VM 18.9 (build 11.0.1+13-LTS, mixed mode)

# Java 17
./jdk-17.0.1/bin/java -version
java version "17.0.1" 2021-10-19 LTS
Java(TM) SE Runtime Environment (build 17.0.1+12-LTS-39)
Java HotSpot(TM) 64-Bit Server VM (build 17.0.1+12-LTS-39, mixed mode, sharing)

# Java 21
./jdk-21.0.1/bin/java -version
java version "21.0.1" 2023-10-17 LTS
Java(TM) SE Runtime Environment (build 21.0.1+12-LTS-29)
Java HotSpot(TM) 64-Bit Server VM (build 21.0.1+12-LTS-29, mixed mode, sharing)
</pre>

- Create symlink/shortcut for each java version (Optional)

You can create a symlink for each version to turn easier the use of specific java version, like this:

<pre>
cd /usr/bin
sudo ln -s /usr/share/jdk/jdk1.8.0_212/bin/java java8
sudo ln -s /usr/share/jdk/jdk-11.0.1/bin/java java11
sudo ln -s /usr/share/jdk/jdk-17.0.1/bin/java java17
sudo ln -s /usr/share/jdk/jdk-21.0.1/bin/java java21

ls -ltr
java8 -> /usr/share/jdk/jdk1.8.0_212/bin/java
java11 -> /usr/share/jdk/jdk-11.0.1/bin/java
java17 -> /usr/share/jdk/jdk-17.0.1/bin/java
java21 -> /usr/share/jdk/jdk-21.0.1/bin/java
</pre>

Now you can just execute:

<pre>
java8 -version
java11 -version
java17 -version
java21 -version
</pre>

- Create the env variable

> WARNING: The most important step is to create an JAVA_HOME environment variable because you need to set up 
> at least one java version as a default, and this variable just support one version per time, so make sure that 
> you have really known what you are doing when you do that.

<pre>
JAVA_HOME=/usr/share/jdk/jdk-17.0.1
export JAVA_HOME
PATH=$PATH:$JAVA_HOME/bin
export PATH
source /etc/profile
echo "JAVA_HOME=/usr/share/jdk/jdk-17.0.1" >> ~/.bashrc
source ~/.bashrc
</pre>

In this way you will set the environment as something like that

<pre>
$JAVA_HOME/bin/java -version
java version "17.0.1" 2021-10-19 LTS
Java(TM) SE Runtime Environment (build 17.0.1+12-LTS-39)
Java HotSpot(TM) 64-Bit Server VM (build 17.0.1+12-LTS-39, mixed mode, sharing)
</pre>

> TIP: In some cases it's required to create a symlink in /usr/bin pointing to default java active in the
current machine, so what you only need to do is

<pre>
cd /usr/bin
sudo ln -s $JAVA_HOME/bin/java java
</pre>

So the result should be something like that

<pre>
java -> /usr/share/jdk/jdk-17.0.1/bin/java
</pre>
