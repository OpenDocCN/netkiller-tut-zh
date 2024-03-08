# 第 2 章 Build Tools

## Apache Ant

[`ant.apache.org/`](http://ant.apache.org/)

### 安装 ant

#### 1.8

```
cd /usr/local/src
wget http://mirror.bjtu.edu.cn/apache//ant/binaries/apache-ant-1.8.1-bin.tar.gz
tar zxvf apache-ant-1.8.1-bin.tar.gz
mv apache-ant-1.8.1 /usr/local/
cd ..
ln -s apache-ant-1.8.1 apache-ant

```

```
ANT_HOME=/usr/local/apache-ant
export CLASSPATH=$CLASSPATH:$ANT_HOME/lib

```

#### 1.10.1

Netkiller OSCM 一键安装

```
curl -s https://raw.githubusercontent.com/oscm/shell/master/lang/java/ant/apache-ant-1.10.1.sh | bash

```

### ANT

#### ant.project.name

ant.project.name 一般式定义在

```

<project name="www.netkiller.cn" default="compile" basedir="." >
<echo>${ant.project.name}</echo>

```

我们希望从命令行传递这个值

```

<project default="compile" basedir="." >
<echo>${ant.project.name}</echo>

```

你需要将 project 中的定义去掉才能从命令行传递

```

ant -Dant.project.name=www.netkiller.cn -f build.xml

```

你也可以从 build.properties 文件定义 ant.project.name

```

MacBook-Pro:deployment neo$ cat build.properties 
ant.project.name=www.netkiller.cn			

```

```

ant -f build.xml -propertyfile build.properties	

```

#### 定义

### Project

```

<description>project Name</description>

```

#### property

在 build.xml 中定义 property

```

<property name="src" value="src"/>
<property name="dest" value="classes"/>
<property name="hello" value="hello.jar"/>		

```

引用 properties 文件

```

<property file="build.properties" />
<propety resource="build.properties"/>

```

设置系统的环境变量为前缀

```

<property environment="env"/> 
<property name="tomcat.home" value="${env.CATALINA_HOME}"/> 

```

命令行传值，使用-D 参数是会覆盖 build.xml 中的先前定义的变量

```

$ ant --help | grep property
  -D<property>=<value>   use value for given property
  -propertyfile <name>   load all properties from file with -D

```

#### ant

Project name

```

${ant.project.name}

```

#### environment

```

<property environment="env"/>
<echo message="JAVA_HOME is set to = ${env.JAVA_HOME}" />			

```

### path

定义

```

	<path id="classpath">  
        <fileset dir="${env.JAVA_HOME}/lib">  
            <include name="*.jar" />  
        </fileset>  
        <fileset dir="${env.CATALINA_HOME}/lib">  
            <include name="*.jar" />  
        </fileset>  
        <fileset dir="WebRoot/WEB-INF/lib" includes="*.jar"/>
    </path>    

```

引用

```

	<javac srcdir="${src.dir}" destdir="${classes.dir}" source="1.5" target="1.5">  
		<classpath refid="classpath" />  
	</javac>		

```

```

    <classpath>
		<path refid="classpath"/>
    </classpath>		

```

### copy

复制目录

```

	<copy todir="${basedir}/WebContent"> 
		<fileset dir="${basedir}/WebRoot" includes="**/*"/>
	</copy>	    

```

复制指定扩展名文件

```

	<copy todir="${dest}">  
		<fileset dir="${src}">  
			<include name="**/*.xml" />  
			<include name="**/*.properties" />  
		</fileset>
	</copy> 	

```

### javac

```

	<path id="classpath">
		<fileset dir="${env.JAVA_HOME}/lib">
			<include name="*.jar" />
		</fileset>
		<fileset dir="${env.CATALINA_HOME}/lib">
			<include name="*.jar" />
		</fileset>
		<fileset dir="${project.dir}/WebRoot/WEB-INF/lib" includes="*.jar" />
	</path>

	<javac srcdir="${project.src}" destdir="${project.build}/WEB-INF/classes" debug="true" listfiles="true">
			<classpath refid="classpath" />
			<include name="**/*.java"/>
			<exclude name="**/*.xml"/>
			<exclude name="**/*.properties"/>
	</javac>

```

listfiles 显示编译文件列表

debug 显示调试信息，编译错误信息

### condition

```

<?xml version="1.0"?>
<project name="test" default="doFoo" basedir=".">
  <property name="directory" value="/www/directory"/>

  <target name="doFoo" depends="dir.check" if="dir.exists">
    <echo>${directory} exists</echo>
  </target>

  <target name="doBar" depends="dir.check" unless="dir.exists">
    <echo>${directory} missing"</echo>
  </target>

  <target name="dir.check">
    <condition property="dir.exists">
      <available file="${directory}" type="dir"/>
    </condition>
  </target>
</project>		

```

### exec

```

<project name="{{ name }}" default="help" basedir=".">

  <property name="username" value="{{ username }}"/>
  <property name="host" value="{{ host }}"/>
  <property name="dir" value="/srv/{{ path }}/"/>

  <tstamp>
    <format property="TODAY_UK" pattern="yyyyMMddhhmmss" locale="en,UK"/>
  </tstamp>

  <target name="help" description="show available commands" >
    <exec executable="ant" dir="." failonerror="true">
      <arg value="-p"/>
    </exec>
  </target>

  <target name="deploy-to" description="show where we are deploying to" >
    <echo>${username}@${host}:${dir}</echo>
  </target>

  <target name="deploy" description="deploy usng rsync" >
    <exec executable="rsync" dir="." failonerror="true">
      <arg value="-r"/>
      <arg value="."/>
      <arg value="${username}@${host}:${dir}"/>
      <arg value="--exclude-from=rsync.excludes"/>
      <arg value="-v"/>
    </exec>
  </target>

  <target name="deploy-test" description="test deploy usng rsync with the dry run flag set" >
    <exec executable="rsync" dir="." failonerror="true">
      <arg value="-r"/>
      <arg value="."/>
      <arg value="${username}@${host}:${dir}"/>
      <arg value="--exclude-from=rsync.excludes"/>
      <arg value="--dry-run"/>
      <arg value="-v"/>
    </exec>
  </target>

  <target name="backup" description="backup site" >
    <exec executable="scp" dir="." failonerror="true">
      <arg value="-r"/>
      <arg value="${username}@${host}:${dir}"/>
      <arg value="backups/${TODAY_UK}"/>
    </exec>
  </target>

</project>		

```

#### sshexec

```

<sshexec host="${remove}" keyfile="~/.ssh/id_rsa" command="/srv/apache-tomcat/bin/catalina.sh stop -force" />			

```

### if

```

<if>
  <available file="my_directory" type="dir" />
  <then>
    <echo message="Directory exists" />
  </then>
  <else>
    <echo message="Directory does not exist" />
  </else>
</if>		

```

Ant 1.9.x 新增

```

<project name="tryit"

>
 <exec executable="java">
   <arg line="-X" if:true="${showextendedparams}"/>
   <arg line="-version" unless:true="${showextendedparams}"/>
 </exec>
 <condition property="onmac">
   <os family="mac"/>
 </condition>
 <echo if:set="onmac">running on MacOS</echo>
 <echo unless:set="onmac">not running on MacOS</echo>
</project>

<!DOCTYPE project>
<project  >
  <property unless:set="property" name="property.is.set" value="false"/>
  <property if:set="property" name="property.is.set" value="true"/>
  <echo>${property.is.set}</echo>
</project>

```

### macrodef

arg value 与 arg line

arg line 可以处理参数的空格， 而 arg value 则不能. arg line 可以处理空参数， arg value 传递空参数会报错.

```

<exec executable = "sh" dir = "@{dir}">
	<arg line = "ls -l /var/log" />
</exec>

<exec executable = "ls" dir = "@{dir}">
	<arg value = "-l" />
	<arg value = "/var/log" />
</exec>

```

```

        <macrodef name="mvn">
                <attribute name="options" default="" />
                <attribute name="goal" default="" />
                <attribute name="phase" default=" " />
                <attribute name="dir" default="" />
                <element name="args" optional="false" />
                <sequential>
                        <exec executable="mvn" dir="@{dir}" >
                                <arg value="@{options}" />
                                <arg value="@{goal}" />
                                <arg value="@{phase}" />
                        </exec>
                </sequential>
        </macrodef>

<!-- 执行下面宏将会出错，你必须传递 options，phase 参数 -->
<mvn goal="package" dir="${project.dir}"/>
<!-- 将 vale 改为 line 后正常 -->
		<exec executable="mvn" dir="@{dir}" >
                                <arg line="@{options}" />
                                <arg line="@{goal}" />
                                <arg line="@{phase}" />
        </exec>

```

运行方式 sequential 为顺序执行， parallel 为并行执行。

#### Git

```

<macrodef name = "git">
    <attribute name = "command" />
    <attribute name = "dir" default = "" />
    <element name = "args" optional = "true" />
    <sequential>
        <echo message = "git @{command}" />
        <exec executable = "git" dir = "@{dir}">
            <arg value = "@{command}" />
            <args/>
        </exec>
    </sequential>
</macrodef>
<macrodef name = "git-clone-pull">
    <attribute name = "repository" />
    <attribute name = "dest" />
    <sequential>
        <git command = "clone">
            <args>
                <arg value = "@{repository}" />
                <arg value = "@{dest}" />
            </args>
        </git>
        <git command = "pull" dir = "@{dest}" />
    </sequential>
</macrodef>

```

```

<git command = "clone">
    <args>
        <arg value = "git://github.com/280north/ojunit.git" />
        <arg value = "ojunit" />
    </args>
</git>

<git command = "pull" dir = "repository_path" />		

<git-clone-pull repository="git://github.com/280north/ojunit.git" dest="ojunit" />	

```

#### Rsync

```

	<macrodef name="rsync">
		<attribute name="option" default="auzv" />
		<attribute name="src" default="" />
		<attribute name="dest" default="" />
		<element name="args" optional="true" />
		<sequential>
			<exec executable="rsync">
				<arg value="@{option}" />
				<arg value="@{src}" />
				<arg value="@{dest}" />
				<args />
			</exec>
		</sequential>
	</macrodef>			

```

```

	<target name="deploy" depends="compile">
		<rsync option="-auzv" src="${project.dest}" dest="${remote}:${destination}">
			<args>
				<arg value="-P" />
			</args>
		</rsync>
	</target>			

```

#### SSH

```

	<macrodef name="ssh">
		<attribute name="host" />
		<attribute name="command" />
		<attribute name="keyfile" default="~/.ssh/id_rsa" />
		<element name="args" optional="true" />
		<sequential>
			<exec executable="ssh">
				<arg value="@{host}" />
				<!-- arg value="-i @{keyfile}" / -->
				<args />
				<arg value="@{command}" />
			</exec>
		</sequential>
	</macrodef>

```

```

	<target name="stop" depends="">
		<!-- ssh host="${remote}" command="/srv/apache-tomcat/bin/catalina.sh stop -force" keyfile="~/.ssh/id_rsa" / -->
		<ssh host="${remote}" command="/srv/apache-tomcat/bin/shutdown.sh" />
	</target>
	<target name="start" depends="">
		<ssh host="${remote}" command="/srv/apache-tomcat/bin/startup.sh" keyfile="~/.ssh/id_rsa" />
	</target>		

```

#### maven

```

        <macrodef name="mvn">
                <attribute name="options" default="" />
                <attribute name="goal" default="" />
                <attribute name="phase" default=" " />
                <attribute name="dir" default="" />
                <element name="args" optional="false" />
                <sequential>
                        <exec executable="mvn" dir="@{dir}" >
                                <arg line="@{options}" />
                                <arg value="@{goal}" />
                                <arg line="@{phase}" />
                        </exec>
                </sequential>
        </macrodef>			

```

### Javascript

```

$ cat build.xml 
<project name="Attachments" default="print">
    <property name="numAttachments" value="20" />
    <target name="generate">
        <script language="javascript"><![CDATA[
            var list = '1';
            var limit = project.getProperty( "numAttachments" );
            for (var i=2;i<limit;i++)
            { 
                list = list + ',' + i;
            }
            project.setNewProperty("list", list);            
		print(list);
        ]] >
        </script>    
    </target>
</project>

```

```

$ ant generate
Buildfile: /www/testing/build.xml

generate:
   [script] 1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19

BUILD SUCCESSFUL
Total time: 0 seconds

```

### mail

https://ant.apache.org/manual/Tasks/mail.html

```

cp ~/.m2/repository/com/sun/mail/javax.mail/1.5.6/javax.mail-1.5.6.jar /srv/apache-ant-1.9.6/lib
cp /www/.m2/repository/com/sun/mail/javax.mail/1.5.6/javax.mail-1.5.6.jar /srv/apache-ant-1.10.1/lib/

```

Examples

```

<mail from="me"
      tolist="you"
      subject="Results of nightly build"
      files="build.log"/>
Sends an email from me to you with a subject of Results of nightly build and includes the contents of the file build.log in the body of the message.

<mail mailhost="smtp.myisp.com" mailport="1025" subject="Test build">
  <from address="config@myisp.com"/>
  <replyto address="me@myisp.com"/>
  <to address="all@xyz.com"/>
  <message>The ${buildname} nightly build has completed</message>
  <attachments>
    <fileset dir="dist">
      <include name="**/*.zip"/>
    </fileset>
  </attachments>
</mail>

```

### basename

```

<basename property="jar.filename" file="${lib.jarfile}"/>
will set jar.filename to myjar.jar, if lib.jarfile is defined as either a full-path filename (eg., /usr/local/lib/myjar.jar), a relative-path filename (eg., lib/myjar.jar), or a simple filename (eg., myjar.jar).
<basename property="cmdname" file="D:/usr/local/foo.exe"
          suffix=".exe"/>
will set cmdname to foo.
<property environment="env"/>
<basename property="temp.dirname" file="${env.TEMP}"/>

```

### FAQ

#### warning: 'includeantruntime' was not set, defaulting to build.sysclasspath=last; set to false for repeatable builds

includeantruntime="false"

```

<target name="compile" depends="init">
   <javac includeantruntime="false" srcdir="${src}" destdir="${dest}"/>
</target>

```

or

```

<property name="build.sysclasspath" value="last"/>

```

#### 调试 exec

将 executable="echo" 设置成 echo 是一种不错的调试手段

```

        <macrodef name="gulp">
                <attribute name="stage" default=""/>
                <attribute name="src" default=""/>
                <attribute name="dir" default="" />
                <sequential>
                        <exec vmlauncher="false" executable="echo" dir="@{dir}" osfamily="unix">
                                <arg line="--stage @{stage} --src @{src}"/>
                                <!-- arg value="stage @{stage}" / -->
                        </exec>
                </sequential>
        </macrodef>

        <target name="gulp">
                <gulp stage="${git.branch}" src="cn" dir="."/>
        </target>

```

## Apache Ivy

[`ant.apache.org/ivy/index.html`](http://ant.apache.org/ivy/index.html)

### Ivy Install

#### source code

```
cd /usr/local/src
wget http://labs.renren.com/apache-mirror//ant/ivy/2.2.0/apache-ivy-2.2.0-bin.tar.gz
tar zxvf apache-ivy-2.2.0-bin.tar.gz
mv apache-ivy-2.2.0 /usr/local/
cd ..
ln -s apache-ivy-2.2.0 apache-ivy

```

```
IVY_HOME=/usr/local/apache-ivy

```

```
cp $IVY_HOME/ivy-2.2.0.jar $ANT_HOME/lib/

```

#### apt-get

```
$ sudo apt-get install ant
$ sudo apt-get install ivy

```

To know more about this package, you can use dpkg

```
$ dpkg -s ivy

```

### Test example

ant

```
cd $IVY_HOME/src/example/hello-ivy
ant

Buildfile: /usr/local/apache-ivy-2.2.0/src/example/hello-ivy/build.xml

resolve:
[ivy:retrieve] :: Ivy 2.2.0 - 20100923230623 :: http://ant.apache.org/ivy/ ::
[ivy:retrieve] :: loading settings :: url = jar:file:/usr/local/apache-ant/lib/ivy-2.2.0.jar!/org/apache/ivy/core/settings/ivysettings.xml
[ivy:retrieve] :: resolving dependencies :: org.apache#hello-ivy;working@example.com
[ivy:retrieve]  confs: [default]
[ivy:retrieve]  found commons-lang#commons-lang;2.0 in public
[ivy:retrieve]  found commons-cli#commons-cli;1.0 in public
[ivy:retrieve]  found commons-logging#commons-logging;1.0 in public
[ivy:retrieve] downloading http://repo1.maven.org/maven2/commons-lang/commons-lang/2.0/commons-lang-2.0.jar ...
[ivy:retrieve] .......................................................................................
[ivy:retrieve] ..................................................................................................................................................................
[ivy:retrieve] ........................................................................................... (165kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve]  [SUCCESSFUL ] commons-lang#commons-lang;2.0!commons-lang.jar (4790ms)
[ivy:retrieve] downloading http://repo1.maven.org/maven2/commons-lang/commons-lang/2.0/commons-lang-2.0-javadoc.jar ...
[ivy:retrieve] ................................................................................................................
[ivy:retrieve] .........................................
[ivy:retrieve] ..................................................
[ivy:retrieve] .....................................................
[ivy:retrieve] ..................................................................................................................................
[ivy:retrieve] ..................................................................................................................................
[ivy:retrieve] .................................................................................................................
[ivy:retrieve] ...........................................................................................................................................................................
[ivy:retrieve] .............................................................................................................................................................................................. (467kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve]  [SUCCESSFUL ] commons-lang#commons-lang;2.0!commons-lang.jar(javadoc) (14878ms)
[ivy:retrieve] downloading http://repo1.maven.org/maven2/commons-lang/commons-lang/2.0/commons-lang-2.0-sources.jar ...
[ivy:retrieve] ...........................................................................................................................................................................
[ivy:retrieve] ................................................................................................................................................................................................................
[ivy:retrieve] .............................................................................................................................................. (245kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve]  [SUCCESSFUL ] commons-lang#commons-lang;2.0!commons-lang.jar(source) (5046ms)
[ivy:retrieve] downloading http://repo1.maven.org/maven2/commons-cli/commons-cli/1.0/commons-cli-1.0-javadoc.jar ...
[ivy:retrieve] ....................................................................................................................................................
[ivy:retrieve] ...................................... (92kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve]  [SUCCESSFUL ] commons-cli#commons-cli;1.0!commons-cli.jar(javadoc) (2838ms)
[ivy:retrieve] downloading http://repo1.maven.org/maven2/commons-cli/commons-cli/1.0/commons-cli-1.0.jar ...
[ivy:retrieve] ......................................................... (29kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve]  [SUCCESSFUL ] commons-cli#commons-cli;1.0!commons-cli.jar (5147ms)
[ivy:retrieve] downloading http://repo1.maven.org/maven2/commons-cli/commons-cli/1.0/commons-cli-1.0-sources.jar ...
[ivy:retrieve] ...................................................................................................... (48kB)
[ivy:retrieve] .. (0kB)
[ivy:retrieve]  [SUCCESSFUL ] commons-cli#commons-cli;1.0!commons-cli.jar(source) (2163ms)
[ivy:retrieve] downloading http://repo1.maven.org/maven2/commons-logging/commons-logging/1.0/commons-logging-1.0.jar ...
[ivy:retrieve] ............................................ (21kB)
[ivy:retrieve] ... (0kB)
[ivy:retrieve]  [SUCCESSFUL ] commons-logging#commons-logging;1.0!commons-logging.jar (2638ms)
[ivy:retrieve] :: resolution report :: resolve 30806ms :: artifacts dl 37511ms
[ivy:retrieve]  :: evicted modules:
[ivy:retrieve]  commons-lang#commons-lang;1.0 by [commons-lang#commons-lang;2.0] in [default]
        ---------------------------------------------------------------------
        |                  |            modules            ||   artifacts   |
        |       conf       | number| search|dwnlded|evicted|| number|dwnlded|
        ---------------------------------------------------------------------
        |      default     |   4   |   3   |   3   |   1   ||   7   |   7   |
        ---------------------------------------------------------------------
[ivy:retrieve] :: retrieving :: org.apache#hello-ivy
[ivy:retrieve]  confs: [default]
[ivy:retrieve]  7 artifacts copied, 0 already retrieved (1069kB/11ms)

run:
    [mkdir] Created dir: /usr/local/apache-ivy-2.2.0/src/example/hello-ivy/build
    [javac] /usr/local/apache-ivy-2.2.0/src/example/hello-ivy/build.xml:53: warning: 'includeantruntime' was not set, defaulting to build.sysclasspath=last; set to false for repeatable builds
    [javac] Compiling 1 source file to /usr/local/apache-ivy-2.2.0/src/example/hello-ivy/build
     [java] standard message : hello ivy !
     [java] capitalized by org.apache.commons.lang.WordUtils : Hello Ivy !

BUILD SUCCESSFUL
Total time: 1 second

```

run it

```
neo@debian:/usr/local/apache-ivy/src/example/hello-ivy/build$ export CLASSPATH=$CLASSPATH:/usr/local/apache-ivy/src/example/hello-ivy/lib/*
neo@debian:/usr/local/apache-ivy/src/example/hello-ivy/build$ /usr/local/java/bin/java example.Hello
standard message : hello ivy !
capitalized by org.apache.commons.lang.WordUtils : Hello Ivy !

```

## Apache Maven

http://maven.apache.org/

### 安装

#### Ubuntu

```
$ sudo apt-get install maven2

$ mvn -version
Apache Maven 2.2.1 (rdebian-4)
Java version: 1.6.0_22
Java home: /usr/lib/jvm/java-6-openjdk/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux" version: "2.6.38-8-generic" arch: "amd64" Family: "unix"

```

```
JAVA_HOME="/usr/lib/jvm/java-6-openjdk/jre"
MAVEN_HOME="/usr/share/maven2/"

```

#### 源码安装

```
curl -s https://raw.githubusercontent.com/oscm/shell/master/lang/java/maven/maven.sh | bash

```

### Maven 命令

#### 参数

多线程下载 -Dmaven.artifact.threads=10 默认是 5

```

mvn -Dmaven.artifact.threads=10 test

您可以使用 MAVEN_OPTS 环境变量。例如：

export MAVEN_OPTS = -Dmaven.artifact.threads = 10

```

#### help

查看可用的 pom 定义

```
mvn help:effective-pom			

```

#### archetype:create

创建 Maven 项目

```

mvn archetype:create   -DarchetypeGroupId=org.apache.maven.archetypes   -DgroupId=com.company -DartifactId=my-app			

```

```

mvn archetype:create 
    -DgroupId=packageName    
    -DartifactId=webappName 
    -DarchetypeArtifactId=maven-archetype-webapp 			

```

#### clean

清楚

```
$ mvn clean		

```

#### compile

编译项目的源代码

```
$ mvn compile

```

##### 多线程编译

增加编译-Dmaven.compile.fork=true 参数，用以指明多线程进行编译

增加 -T 1C 参数，表示每个 CPU 核心跑一个工程

```

mvn clean package -T 1C -Dmaven.test.skip=true  -Dmaven.compile.fork=true				

```

-T 4 是直接指定 4 线程

-T 1C 表示 CPU 线程的倍数, 现在现在 1 个物理 CPU，有 4 个核心，8 个线程。那么此时-T 1C 就是 8 线程。

#### 编译测试代码

```

mvn test-compile  			

```

#### test

编译测试的源代码

```
mvn test-compile			

```

运行测试

```
$ mvn test

```

打包但不测试

```

mvn -D maven.test.skip=true package

```

忽略测试失败

```

mvn test -Dmaven.test.failure.ignore			

```

#### package

将编译后的代码打包

```
$ mvn package

```

```
$ ls target/*.war
target/www.netkiller.cn-0.0.1-SNAPSHOT.war

```

防止出现脏数据，可以先 clean 然后再打包，同时为了加快打包速度，逃过测试

```

mvn clean package -Dmaven.test.skip=true			

```

#### install

将项目打包后安装到本地仓库，可以作为其它项目的本地依赖。

```
$ mvn install		

```

跳过测试

```
mvn install -Dmaven.test.skip=true			

```

##### install-file

安装本地 jar 文件到 Maven 的.m2 库中。

```

mvn install:install-file -Dfile=Path/to/your.jar} -DgroupId=com.example -DartifactId=project -Dversion=1.2.0 -Dpackaging=jar

```

#### war

```
mvn compile war:war			

```

```
mvn compile war:exploded			

```

```
mvn compile war:inplace

```

#### exec

```
$ mvn exec:java -Dexec.mainClass="cn.netkiller.Test"			

```

#### dependency

##### build-classpath

```
$ mvn dependency:build-classpath
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building api.netkiller.cn 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-dependency-plugin:2.10:build-classpath (default-cli) @ api.cf88.com ---
[INFO] Dependencies classpath:
/www/.m2/repository/org/springframework/boot/spring-boot-starter-web/1.3.0.RELEASE/spring-boot-starter-web-1.3.0.RELEASE.jar:......:/www/.m2/repository/junit/junit/4.12/junit-4.12.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 2.692 s
[INFO] Finished at: 2016-08-10T17:32:49+08:00
[INFO] Final Memory: 25M/162M
[INFO] ------------------------------------------------------------------------

```

##### dependency:tree 显示包依赖树

```
[www@iZ62yln3rjjZ repository]$ mvn dependency:tree
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building api.example.com 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-dependency-plugin:2.10:tree (default-cli) @ api.cf88.com ---
[INFO] cf88.com:api.cf88.com:jar:0.0.1-SNAPSHOT
[INFO] +- org.springframework.boot:spring-boot-starter-web:jar:1.3.0.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter:jar:1.3.0.RELEASE:compile
[INFO] |  |  +- org.springframework.boot:spring-boot-starter-logging:jar:1.3.0.RELEASE:compile
[INFO] |  |  |  +- ch.qos.logback:logback-classic:jar:1.1.3:compile
[INFO] |  |  |  |  \- ch.qos.logback:logback-core:jar:1.1.3:compile
[INFO] |  |  |  +- org.slf4j:jul-to-slf4j:jar:1.7.13:compile
[INFO] |  |  |  \- org.slf4j:log4j-over-slf4j:jar:1.7.13:compile
[INFO] |  |  \- org.yaml:snakeyaml:jar:1.16:runtime
[INFO] |  +- org.springframework.boot:spring-boot-starter-tomcat:jar:1.3.0.RELEASE:compile
[INFO] |  |  +- org.apache.tomcat.embed:tomcat-embed-core:jar:8.0.28:compile
[INFO] |  |  +- org.apache.tomcat.embed:tomcat-embed-el:jar:8.0.28:compile
[INFO] |  |  +- org.apache.tomcat.embed:tomcat-embed-logging-juli:jar:8.0.28:compile
[INFO] |  |  \- org.apache.tomcat.embed:tomcat-embed-websocket:jar:8.0.28:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-validation:jar:1.3.0.RELEASE:compile
[INFO] |  |  \- org.hibernate:hibernate-validator:jar:5.2.2.Final:compile
[INFO] |  |     +- javax.validation:validation-api:jar:1.1.0.Final:compile
[INFO] |  |     \- com.fasterxml:classmate:jar:1.1.0:compile
[INFO] |  +- com.fasterxml.jackson.core:jackson-databind:jar:2.6.3:compile
[INFO] |  |  +- com.fasterxml.jackson.core:jackson-annotations:jar:2.6.3:compile
[INFO] |  |  \- com.fasterxml.jackson.core:jackson-core:jar:2.6.3:compile
[INFO] |  +- org.springframework:spring-web:jar:4.2.3.RELEASE:compile
[INFO] |  |  \- org.springframework:spring-aop:jar:4.2.3.RELEASE:compile
[INFO] |  |     \- aopalliance:aopalliance:jar:1.0:compile
[INFO] |  \- org.springframework:spring-webmvc:jar:4.2.3.RELEASE:compile
[INFO] +- org.springframework.boot:spring-boot-starter-data-jpa:jar:1.3.0.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot-starter-aop:jar:1.3.0.RELEASE:compile
[INFO] |  |  \- org.aspectj:aspectjweaver:jar:1.8.7:compile
[INFO] |  +- org.hibernate:hibernate-entitymanager:jar:4.3.11.Final:compile
[INFO] |  |  +- org.jboss.logging:jboss-logging:jar:3.3.0.Final:compile
[INFO] |  |  +- org.jboss.logging:jboss-logging-annotations:jar:1.2.0.Beta1:compile
[INFO] |  |  +- org.hibernate:hibernate-core:jar:4.3.11.Final:compile
[INFO] |  |  |  +- antlr:antlr:jar:2.7.7:compile
[INFO] |  |  |  \- org.jboss:jandex:jar:1.1.0.Final:compile
[INFO] |  |  +- dom4j:dom4j:jar:1.6.1:compile
[INFO] |  |  |  \- xml-apis:xml-apis:jar:1.0.b2:compile
[INFO] |  |  +- org.hibernate.common:hibernate-commons-annotations:jar:4.0.5.Final:compile
[INFO] |  |  +- org.hibernate.javax.persistence:hibernate-jpa-2.1-api:jar:1.0.0.Final:compile
[INFO] |  |  \- org.javassist:javassist:jar:3.18.1-GA:compile
[INFO] |  +- javax.transaction:javax.transaction-api:jar:1.2:compile
[INFO] |  +- org.springframework.data:spring-data-jpa:jar:1.9.1.RELEASE:compile
[INFO] |  |  \- org.springframework:spring-orm:jar:4.2.3.RELEASE:compile
[INFO] |  \- org.springframework:spring-aspects:jar:4.2.3.RELEASE:compile
[INFO] +- org.springframework.boot:spring-boot-starter-jdbc:jar:1.3.0.RELEASE:compile
[INFO] |  +- org.apache.tomcat:tomcat-jdbc:jar:8.0.28:compile
[INFO] |  |  \- org.apache.tomcat:tomcat-juli:jar:8.0.28:compile
[INFO] |  \- org.springframework:spring-jdbc:jar:4.2.3.RELEASE:compile
[INFO] +- org.springframework.boot:spring-boot-starter-redis:jar:1.3.0.RELEASE:compile
[INFO] |  +- org.springframework.data:spring-data-redis:jar:1.6.1.RELEASE:compile
[INFO] |  |  \- org.springframework:spring-context-support:jar:4.2.3.RELEASE:compile
[INFO] |  \- redis.clients:jedis:jar:2.7.3:compile
[INFO] |     \- org.apache.commons:commons-pool2:jar:2.4.2:compile
[INFO] +- org.springframework.boot:spring-boot-starter-data-mongodb:jar:1.3.0.RELEASE:compile
[INFO] |  \- org.mongodb:mongo-java-driver:jar:2.13.3:compile
[INFO] +- org.springframework.boot:spring-boot-starter-amqp:jar:1.3.0.RELEASE:compile
[INFO] |  +- org.springframework:spring-messaging:jar:4.2.3.RELEASE:compile
[INFO] |  \- org.springframework.amqp:spring-rabbit:jar:1.5.2.RELEASE:compile
[INFO] |     +- org.springframework.retry:spring-retry:jar:1.1.2.RELEASE:compile
[INFO] |     +- org.springframework.amqp:spring-amqp:jar:1.5.2.RELEASE:compile
[INFO] |     +- com.rabbitmq:http-client:jar:1.0.0.RELEASE:compile
[INFO] |     |  \- org.apache.httpcomponents:httpclient:jar:4.5.1:compile
[INFO] |     |     +- org.apache.httpcomponents:httpcore:jar:4.4.4:compile
[INFO] |     |     \- commons-codec:commons-codec:jar:1.9:compile
[INFO] |     \- com.rabbitmq:amqp-client:jar:3.5.6:compile
[INFO] +- org.springframework.boot:spring-boot-devtools:jar:1.3.0.RELEASE:compile
[INFO] |  +- org.springframework.boot:spring-boot:jar:1.3.0.RELEASE:compile
[INFO] |  \- org.springframework.boot:spring-boot-autoconfigure:jar:1.3.0.RELEASE:compile
[INFO] +- org.springframework.boot:spring-boot-starter-test:jar:1.3.0.RELEASE:test
[INFO] |  +- org.mockito:mockito-core:jar:1.10.19:test
[INFO] |  |  \- org.objenesis:objenesis:jar:2.1:test
[INFO] |  +- org.hamcrest:hamcrest-core:jar:1.3:test
[INFO] |  +- org.hamcrest:hamcrest-library:jar:1.3:test
[INFO] |  +- org.springframework:spring-core:jar:4.2.3.RELEASE:compile
[INFO] |  \- org.springframework:spring-test:jar:4.2.3.RELEASE:test
[INFO] +- org.springframework.data:spring-data-mongodb:jar:1.8.1.RELEASE:compile
[INFO] |  +- org.springframework:spring-tx:jar:4.2.3.RELEASE:compile
[INFO] |  +- org.springframework:spring-context:jar:4.2.3.RELEASE:compile
[INFO] |  +- org.springframework:spring-beans:jar:4.2.3.RELEASE:compile
[INFO] |  +- org.springframework:spring-expression:jar:4.2.3.RELEASE:compile
[INFO] |  +- org.springframework.data:spring-data-commons:jar:1.11.1.RELEASE:compile
[INFO] |  +- org.slf4j:slf4j-api:jar:1.7.13:compile
[INFO] |  \- org.slf4j:jcl-over-slf4j:jar:1.7.13:compile
[INFO] +- mysql:mysql-connector-java:jar:5.1.37:compile
[INFO] +- com.google.code.gson:gson:jar:2.3.1:compile
[INFO] \- junit:junit:jar:4.12:test
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 4.400 s
[INFO] Finished at: 2016-08-09T10:25:07+08:00
[INFO] Final Memory: 23M/163M
[INFO] ------------------------------------------------------------------------		

```

##### copy-dependencies 导出依赖包

```
$ mvn dependency:copy-dependencies

```

```
$ ls target/dependency/
antlr-2.7.7.jar                                javassist-3.18.1-GA.jar                    snakeyaml-1.16.jar                                spring-boot-starter-web-1.3.0.RELEASE.jar        spring-webmvc-4.2.3.RELEASE.jar
aopalliance-1.0.jar                            javax.transaction-api-1.2.jar              spring-aop-4.2.3.RELEASE.jar                      spring-boot-starter-websocket-1.3.0.RELEASE.jar  spring-websocket-4.2.3.RELEASE.jar
aspectjweaver-1.8.7.jar                        jboss-logging-3.3.0.Final.jar              spring-aspects-4.2.3.RELEASE.jar                  spring-context-4.2.3.RELEASE.jar                 thymeleaf-2.1.4.RELEASE.jar
classmate-1.1.0.jar                            jboss-logging-annotations-1.2.0.Beta1.jar  spring-beans-4.2.3.RELEASE.jar                    spring-core-4.2.3.RELEASE.jar                    thymeleaf-layout-dialect-1.3.1.jar
dom4j-1.6.1.jar                                jcl-over-slf4j-1.7.13.jar                  spring-boot-1.3.0.RELEASE.jar                     spring-data-commons-1.11.1.RELEASE.jar           thymeleaf-spring4-2.1.4.RELEASE.jar
groovy-2.4.4.jar                               jstl-1.2.jar                               spring-boot-autoconfigure-1.3.0.RELEASE.jar       spring-data-jpa-1.9.1.RELEASE.jar                tomcat-embed-core-8.0.28.jar
hibernate-commons-annotations-4.0.5.Final.jar  jul-to-slf4j-1.7.13.jar                    spring-boot-starter-1.3.0.RELEASE.jar             spring-expression-4.2.3.RELEASE.jar              tomcat-embed-el-8.0.28.jar
hibernate-core-4.3.11.Final.jar                log4j-over-slf4j-1.7.13.jar                spring-boot-starter-aop-1.3.0.RELEASE.jar         spring-jdbc-4.2.3.RELEASE.jar                    tomcat-embed-logging-juli-8.0.28.jar
hibernate-entitymanager-4.3.11.Final.jar       logback-classic-1.1.3.jar                  spring-boot-starter-data-jpa-1.3.0.RELEASE.jar    spring-messaging-4.2.3.RELEASE.jar               tomcat-embed-websocket-8.0.28.jar
hibernate-jpa-2.1-api-1.0.0.Final.jar          logback-core-1.1.3.jar                     spring-boot-starter-jdbc-1.3.0.RELEASE.jar        spring-orm-4.2.3.RELEASE.jar                     tomcat-jdbc-8.0.28.jar
hibernate-validator-5.2.2.Final.jar            mybatis-3.3.0.jar                          spring-boot-starter-logging-1.3.0.RELEASE.jar     spring-security-config-4.0.3.RELEASE.jar         tomcat-juli-8.0.28.jar
jackson-annotations-2.6.3.jar                  mybatis-spring-1.2.3.jar                   spring-boot-starter-security-1.3.0.RELEASE.jar    spring-security-core-4.0.3.RELEASE.jar           unbescape-1.1.0.RELEASE.jar
jackson-core-2.6.3.jar                         mysql-connector-java-5.1.37.jar            spring-boot-starter-thymeleaf-1.3.0.RELEASE.jar   spring-security-web-4.0.3.RELEASE.jar            validation-api-1.1.0.Final.jar
jackson-databind-2.6.3.jar                     ognl-3.0.8.jar                             spring-boot-starter-tomcat-1.3.0.RELEASE.jar      spring-tx-4.2.3.RELEASE.jar                      xml-apis-1.0.b2.jar
jandex-1.1.0.Final.jar                         slf4j-api-1.7.13.jar                       spring-boot-starter-validation-1.3.0.RELEASE.jar  spring-web-4.2.3.RELEASE.jar		

```

##### analyze 查看未被使用的包

```
$ mvn dependency:analyze			

```

##### sources 下载源码

```
mvn dependency:sources			

```

#### jar

```

mvn -Djar.finalName=myCustomName package			

```

```

mvn jar:jar -Djar.finalName=custom-jar-name			

```

#### 构建装配 Maven Assembly

```

mvn install assembly:assembly			

```

#### 加密密码

--encrypt-master-password

```

neo@MacBook-Pro ~ % mvn --encrypt-master-password test
{LhCRW9y8CG4HfT7ExHI3msP56Cv+fgjdiNhTk883hZs=}			

```

#### help:describe

显示插件的详细信息

```

MacBook-Pro:deployment neo$ mvn help:describe -DgroupId=org.apache.maven.plugins                   -DartifactId=maven-war-plugin                   -Ddetail=true
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building Maven Stub Project (No POM) 1
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-help-plugin:2.2:describe (default-cli) @ standalone-pom ---
[INFO] org.apache.maven.plugins:maven-war-plugin:3.1.0

Name: Apache Maven WAR Plugin
Description: Builds a Web Application Archive (WAR) file from the project
  output and its dependencies.
Group Id: org.apache.maven.plugins
Artifact Id: maven-war-plugin
Version: 3.1.0
Goal Prefix: war

This plugin has 4 goals:

war:exploded
  Description: Create an exploded webapp in a specified directory.
  Implementation: org.apache.maven.plugins.war.WarExplodedMojo
  Language: java
  Bound to phase: package

  Available parameters:

    archive
      The archive configuration to use. See Maven Archiver Reference.

    archiveClasses (Default: false)
      Whether a JAR file will be created for the classes in the webapp. Using
      this optional configuration parameter will make the compiled classes to
      be archived into a JAR file and the classes directory will then be
      excluded from the webapp.

    cacheFile (Default: ${project.build.directory}/war/work/webapp-cache.xml)
      Required: true
      The file containing the webapp structure cache.

    containerConfigXML
      The path to a configuration file for the servlet container. Note that the
      file name may be different for different servlet containers. Apache
      Tomcat uses a configuration file named context.xml. The file will be
      copied to the META-INF directory.

    delimiters
      Set of delimiters for expressions to filter within the resources. These
      delimiters are specified in the form 'beginToken*endToken'. If no '*' is
      given, the delimiter is assumed to be the same for start and end.

      So, the default filtering delimiters might be specified as:

      <delimiters>
        <delimiter>${*}</delimiter>
        <delimiter>@</delimiter>
      </delimiters>

      Since the '@' delimiter is the same on both ends, we don't need to
      specify '@*@' (though we can).

    dependentWarExcludes
      The comma separated list of tokens to exclude when doing a WAR overlay.
      Default is Overlay.DEFAULT_EXCLUDES

    dependentWarIncludes
      The comma separated list of tokens to include when doing a WAR overlay.
      Default is Overlay.DEFAULT_INCLUDES

    escapedBackslashesInFilePath (Default: false)
      To escape interpolated values with Windows path c:\foo\bar will be
      replaced with c:\\foo\\bar.

    escapeString
      Expression preceded with this String won't be interpolated. \${foo} will
      be replaced with ${foo}.

    filteringDeploymentDescriptors (Default: false)
      To filter deployment descriptors. Disabled by default.

    filters
      Filters (property files) to include during the interpolation of the
      pom.xml.

    includeEmptyDirectories (Default: false)
      (no description available)

    nonFilteredFileExtensions
      A list of file extensions that should not be filtered. Will be used when
      filtering webResources and overlays.

    outputFileNameMapping
      The file name mapping to use when copying libraries and TLDs. If no file
      mapping is set (default) the files are copied with their standard names.

    overlays
      The overlays to apply. Each <overlay> element may contain:
      - id (defaults to currentBuild)
      - groupId (if this and artifactId are null, then the current project is
        treated as its own overlay)
      - artifactId (see above)
      - classifier
      - type
      - includes (a list of string patterns)
      - excludes (a list of string patterns)
      - filtered (defaults to false)
      - skip (defaults to false)
      - targetPath (defaults to root of webapp structure)

    recompressZippedFiles (Default: true)
      Indicates if zip archives (jar,zip etc) being added to the war should be
      compressed again. Compressing again can result in smaller archive size,
      but gives noticeably longer execution time.

    resourceEncoding (Default: ${project.build.sourceEncoding})
      The encoding to use when copying filtered web resources.

    supportMultiLineFiltering (Default: false)
      Stop searching endToken at the end of line

    useCache (Default: false)
      Whether the cache should be used to save the status of the webapp across
      multiple runs. Experimental feature so disabled by default.

    useDefaultDelimiters (Default: true)
      Use default delimiters in addition to custom delimiters, if any.

    useJvmChmod (Default: true)
      use jvmChmod rather that cli chmod and forking process

    warSourceDirectory (Default: ${basedir}/src/main/webapp)
      Required: true
      Single directory for extra files to include in the WAR. This is where you
      place your JSP files.

    warSourceExcludes
      The comma separated list of tokens to exclude when copying the content of
      the warSourceDirectory.

    warSourceIncludes (Default: **)
      The comma separated list of tokens to include when copying the content of
      the warSourceDirectory.

    webappDirectory (Default:
    ${project.build.directory}/${project.build.finalName})
      Required: true
      The directory where the webapp is built.

    webResources
      The list of webResources we want to transfer.

    webXml
      The path to the web.xml file to use.

    workDirectory (Default: ${project.build.directory}/war/work)
      Required: true
      Directory to unpack dependent WARs into if needed.

war:help
  Description: Display help information on maven-war-plugin.
    Call mvn war:help -Ddetail=true -Dgoal=<goal-name> to display parameter
    details.
  Implementation: org.apache.maven.plugins.war.HelpMojo
  Language: java

  Available parameters:

    detail (Default: false)
      User property: detail
      If true, display all settable properties for each goal.

    goal
      User property: goal
      The name of the goal for which to show help. If unspecified, all goals
      will be displayed.

    indentSize (Default: 2)
      User property: indentSize
      The number of spaces per indentation level, should be positive.

    lineLength (Default: 80)
      User property: lineLength
      The maximum length of a display line, should be positive.

war:inplace
  Description: Generate the webapp in the WAR source directory.
  Implementation: org.apache.maven.plugins.war.WarInPlaceMojo
  Language: java

  Available parameters:

    archive
      The archive configuration to use. See Maven Archiver Reference.

    archiveClasses (Default: false)
      Whether a JAR file will be created for the classes in the webapp. Using
      this optional configuration parameter will make the compiled classes to
      be archived into a JAR file and the classes directory will then be
      excluded from the webapp.

    cacheFile (Default: ${project.build.directory}/war/work/webapp-cache.xml)
      Required: true
      The file containing the webapp structure cache.

    containerConfigXML
      The path to a configuration file for the servlet container. Note that the
      file name may be different for different servlet containers. Apache
      Tomcat uses a configuration file named context.xml. The file will be
      copied to the META-INF directory.

    delimiters
      Set of delimiters for expressions to filter within the resources. These
      delimiters are specified in the form 'beginToken*endToken'. If no '*' is
      given, the delimiter is assumed to be the same for start and end.

      So, the default filtering delimiters might be specified as:

      <delimiters>
        <delimiter>${*}</delimiter>
        <delimiter>@</delimiter>
      </delimiters>

      Since the '@' delimiter is the same on both ends, we don't need to
      specify '@*@' (though we can).

    dependentWarExcludes
      The comma separated list of tokens to exclude when doing a WAR overlay.
      Default is Overlay.DEFAULT_EXCLUDES

    dependentWarIncludes
      The comma separated list of tokens to include when doing a WAR overlay.
      Default is Overlay.DEFAULT_INCLUDES

    escapedBackslashesInFilePath (Default: false)
      To escape interpolated values with Windows path c:\foo\bar will be
      replaced with c:\\foo\\bar.

    escapeString
      Expression preceded with this String won't be interpolated. \${foo} will
      be replaced with ${foo}.

    filteringDeploymentDescriptors (Default: false)
      To filter deployment descriptors. Disabled by default.

    filters
      Filters (property files) to include during the interpolation of the
      pom.xml.

    includeEmptyDirectories (Default: false)
      (no description available)

    nonFilteredFileExtensions
      A list of file extensions that should not be filtered. Will be used when
      filtering webResources and overlays.

    outputFileNameMapping
      The file name mapping to use when copying libraries and TLDs. If no file
      mapping is set (default) the files are copied with their standard names.

    overlays
      The overlays to apply. Each <overlay> element may contain:
      - id (defaults to currentBuild)
      - groupId (if this and artifactId are null, then the current project is
        treated as its own overlay)
      - artifactId (see above)
      - classifier
      - type
      - includes (a list of string patterns)
      - excludes (a list of string patterns)
      - filtered (defaults to false)
      - skip (defaults to false)
      - targetPath (defaults to root of webapp structure)

    recompressZippedFiles (Default: true)
      Indicates if zip archives (jar,zip etc) being added to the war should be
      compressed again. Compressing again can result in smaller archive size,
      but gives noticeably longer execution time.

    resourceEncoding (Default: ${project.build.sourceEncoding})
      The encoding to use when copying filtered web resources.

    supportMultiLineFiltering (Default: false)
      Stop searching endToken at the end of line

    useCache (Default: false)
      Whether the cache should be used to save the status of the webapp across
      multiple runs. Experimental feature so disabled by default.

    useDefaultDelimiters (Default: true)
      Use default delimiters in addition to custom delimiters, if any.

    useJvmChmod (Default: true)
      use jvmChmod rather that cli chmod and forking process

    warSourceDirectory (Default: ${basedir}/src/main/webapp)
      Required: true
      Single directory for extra files to include in the WAR. This is where you
      place your JSP files.

    warSourceExcludes
      The comma separated list of tokens to exclude when copying the content of
      the warSourceDirectory.

    warSourceIncludes (Default: **)
      The comma separated list of tokens to include when copying the content of
      the warSourceDirectory.

    webappDirectory (Default:
    ${project.build.directory}/${project.build.finalName})
      Required: true
      The directory where the webapp is built.

    webResources
      The list of webResources we want to transfer.

    webXml
      The path to the web.xml file to use.

    workDirectory (Default: ${project.build.directory}/war/work)
      Required: true
      Directory to unpack dependent WARs into if needed.

war:war
  Description: Build a WAR file.
  Implementation: org.apache.maven.plugins.war.WarMojo
  Language: java
  Bound to phase: package

  Available parameters:

    archive
      The archive configuration to use. See Maven Archiver Reference.

    archiveClasses (Default: false)
      Whether a JAR file will be created for the classes in the webapp. Using
      this optional configuration parameter will make the compiled classes to
      be archived into a JAR file and the classes directory will then be
      excluded from the webapp.

    attachClasses (Default: false)
      Whether classes (that is the content of the WEB-INF/classes directory)
      should be attached to the project as an additional artifact.
      By default the classifier for the additional artifact is 'classes'. You
      can change it with the someclassifier parameter.

      If this parameter true, another project can depend on the classes by
      writing something like:

        myGroup
        myArtifact
        myVersion
        classes

    cacheFile (Default: ${project.build.directory}/war/work/webapp-cache.xml)
      Required: true
      The file containing the webapp structure cache.

    classesClassifier (Default: classes)
      The classifier to use for the attached classes artifact.

    classifier
      Classifier to add to the generated WAR. If given, the artifact will be an
      attachment instead. The classifier will not be applied to the JAR file of
      the project - only to the WAR file.

    containerConfigXML
      The path to a configuration file for the servlet container. Note that the
      file name may be different for different servlet containers. Apache
      Tomcat uses a configuration file named context.xml. The file will be
      copied to the META-INF directory.

    delimiters
      Set of delimiters for expressions to filter within the resources. These
      delimiters are specified in the form 'beginToken*endToken'. If no '*' is
      given, the delimiter is assumed to be the same for start and end.

      So, the default filtering delimiters might be specified as:

      Since the '@' delimiter is the same on both ends, we don't need to
      specify '@*@' (though we can).

    dependentWarExcludes
      The comma separated list of tokens to exclude when doing a WAR overlay.
      Default is Overlay.DEFAULT_EXCLUDES

    dependentWarIncludes
      The comma separated list of tokens to include when doing a WAR overlay.
      Default is Overlay.DEFAULT_INCLUDES

    escapedBackslashesInFilePath (Default: false)
      To escape interpolated values with Windows path c:\foo\bar will be
      replaced with c:\\foo\\bar.

    escapeString
      Expression preceded with this String won't be interpolated. \${foo} will
      be replaced with ${foo}.

    failOnMissingWebXml
      Whether or not to fail the build if the web.xml file is missing. Set to
      false if you want your WAR built without a web.xml file. This may be
      useful if you are building an overlay that has no web.xml file.
      Starting with 3.1.0, this property defaults to false if the project
      depends on the Servlet 3.0 API or newer.

    filteringDeploymentDescriptors (Default: false)
      To filter deployment descriptors. Disabled by default.

    filters
      Filters (property files) to include during the interpolation of the
      pom.xml.

    includeEmptyDirectories (Default: false)
      (no description available)

    nonFilteredFileExtensions
      A list of file extensions that should not be filtered. Will be used when
      filtering webResources and overlays.

    outputDirectory (Default: ${project.build.directory})
      Required: true
      The directory for the generated WAR.

    outputFileNameMapping
      The file name mapping to use when copying libraries and TLDs. If no file
      mapping is set (default) the files are copied with their standard names.

    overlays
      The overlays to apply. Each overlay element may contain:
      - id (defaults to currentBuild)
      - groupId (if this and artifactId are null, then the current project is
        treated as its own overlay)
      - artifactId (see above)
      - classifier
      - type
      - includes (a list of string patterns)
      - excludes (a list of string patterns)
      - filtered (defaults to false)
      - skip (defaults to false)
      - targetPath (defaults to root of webapp structure)

    packagingExcludes
      The comma separated list of tokens to exclude from the WAR before
      packaging. This option may be used to implement the skinny WAR use case.
      Note that you can use the Java Regular Expressions engine to include and
      exclude specific pattern using the expression %regex[]. Hint: read the
      about (?!Pattern).

    packagingIncludes
      The comma separated list of tokens to include in the WAR before
      packaging. By default everything is included. This option may be used to
      implement the skinny WAR use case. Note that you can use the Java Regular
      Expressions engine to include and exclude specific pattern using the
      expression %regex[].

    primaryArtifact (Default: true)
      Whether this is the main artifact being built. Set to false if you don't
      want to install or deploy it to the local repository instead of the
      default one in an execution.

    recompressZippedFiles (Default: true)
      Indicates if zip archives (jar,zip etc) being added to the war should be
      compressed again. Compressing again can result in smaller archive size,
      but gives noticeably longer execution time.

    resourceEncoding (Default: ${project.build.sourceEncoding})
      The encoding to use when copying filtered web resources.

    skip (Default: false)
      User property: maven.war.skip
      You can skip the execution of the plugin if you need to. Its use is NOT
      RECOMMENDED, but quite convenient on occasion.

    supportMultiLineFiltering (Default: false)
      Stop searching endToken at the end of line

    useCache (Default: false)
      Whether the cache should be used to save the status of the webapp across
      multiple runs. Experimental feature so disabled by default.

    useDefaultDelimiters (Default: true)
      Use default delimiters in addition to custom delimiters, if any.

    useJvmChmod (Default: true)
      use jvmChmod rather that cli chmod and forking process

    warSourceDirectory (Default: ${basedir}/src/main/webapp)
      Required: true
      Single directory for extra files to include in the WAR. This is where you
      place your JSP files.

    warSourceExcludes
      The comma separated list of tokens to exclude when copying the content of
      the warSourceDirectory.

    warSourceIncludes (Default: **)
      The comma separated list of tokens to include when copying the content of
      the warSourceDirectory.

    webappDirectory (Default:
    ${project.build.directory}/${project.build.finalName})
      Required: true
      The directory where the webapp is built.

    webResources
      The list of webResources we want to transfer.

    webXml
      The path to the web.xml file to use.

    workDirectory (Default: ${project.build.directory}/war/work)
      Required: true
      Directory to unpack dependent WARs into if needed.

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.960 s
[INFO] Finished at: 2017-08-03T14:33:18+08:00
[INFO] Final Memory: 10M/155M
[INFO] ------------------------------------------------------------------------			

```

```

MacBook-Pro:api.netkiller.cn neo$ mvn help:describe \
	-DgroupId=org.springframework.boot \
	-DartifactId=spring-boot-maven-plugin \
	-Ddetail=true			

```

### Maven 仓库

http://search.maven.org/

http://mvnrepository.com/

本地下载目录

```
$ ls ~/.m2

```

### pom.xml

#### properties

定义 properties

```

<properties>
    <spring.version>4.0.1.RELEASE</spring.version>
</properties>

```

引用 properties

```

  <!-- Spring dependencies -->
   <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>${spring.version}</version>
  </dependency>

```

例 2.1. Maven properties

pom.xml 文件

```

<project  
 xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

 <modelVersion>4.0.0</modelVersion>
 <groupId>com.javahash.web</groupId>
 <artifactId>Spring4MVCHelloWorld</artifactId>
 <packaging>war</packaging>
 <version>1.0-SNAPSHOT</version>
 <name>Spring4MVCHelloWorld Maven Webapp</name>
 <url>http://maven.apache.org</url>

 <properties>
    <spring.version>4.0.1.RELEASE</spring.version>
 </properties>

 <dependencies>

    <dependency>
     <groupId>junit</groupId>
     <artifactId>junit</artifactId>
     <version>4.12</version>
     <scope>test</scope>
    </dependency>

   <!-- Spring dependencies -->
   <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>${spring.version}</version>
  </dependency>

 <dependency>
   <groupId>org.springframework</groupId>
   <artifactId>spring-web</artifactId>
   <version>${spring.version}</version>
 </dependency>

 <dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>${spring.version}</version>
 </dependency>
</dependencies>

 <build>
 <finalName>Spring4MVCHelloWorld</finalName>
 </build>
</project>

```

##### java.version

```

	<properties>
         <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
         <java.version>1.8</java.version>
    </properties>

```

#### repositories 仓库配置

##### 默认仓库

```

		<repository>
			<id>mvnrepository</id>
			<name>mvnrepository</name>
			<url>http://www.mvnrepository.com/</url>
			<layout>default</layout>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</repository>

```

##### 阿里云仓库

```

	<repositories>
		<repository>
			<id>alimaven</id>
			<name>aliyun maven</name>
			<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
			<releases>
				<enabled>true</enabled>
			</releases>
			<snapshots>
				<enabled>false</enabled>
			</snapshots>
		</repository>
	</repositories>				

```

#### dependencies

```

  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    <dependency>
    	<groupId>org.apache.struts</groupId>
    	<artifactId>struts2-core</artifactId>
    	<version>2.3.24.1</version>
    </dependency>
  </dependencies>

```

#### dependencyManagement

声明父项目引用包的版本号。

```

	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>org.springframework.cloud</groupId>
				<artifactId>spring-cloud-netflix</artifactId>
				<version>1.3.5.RELEASE</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
			<dependency>
				<groupId>org.springframework.cloud</groupId>
				<artifactId>spring-cloud-config</artifactId>
				<version>1.3.3.RELEASE</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>			

```

#### build

##### finalName

最终 jar 包得名字

```

<build>
		<finalName>crawler</finalName>
</build>

```

##### sourceDirectory

编译源码目录

```

<sourceDirectory>src</sourceDirectory>				

```

##### resources 文件处理

resources 用来处理 src/main/resources 目录中得内容

```

		<resources>
			<resource>
				<directory>src/main/resources</directory>
				<targetPath>${project.build.directory}/conf</targetPath>
			</resource>
			<resource>
				<directory>src/main/resources</directory>
				<excludes>
					<exclude>development.properties</exclude>
				</excludes>
			</resource>
			<resource>
				<directory>src/main/resources</directory>
				<includes>
					<include>development.properties</include>
				</includes>
				<targetPath>resources</targetPath>
			</resource>

			<resource>
				<directory>lib</directory>
				<includes>
					<include>**/*.sh</include>
					<include>**/*.bat</include>
				</includes>
				<targetPath>${project.build.directory}/lib</targetPath>
			</resource>
		</resources>

```

###### resources

将资源文件编译后复制到 WEB-INF/classes 目录中

```

		<resources>
			<resource>
				<directory>src/resources</directory>
			</resource>
		</resources>			

```

include / exclude

```

<resources>  
    <!-- Filter jdbc.properties & mail.properties. NOTE: We don't filter applicationContext-infrastructure.xml,   
        let it go with spring's resource process mechanism. -->  
    <resource>  
        <directory>src/main/resources</directory>  
        <filtering>true</filtering>  
        <includes>  
            <include>jdbc.properties</include>  
            <include>mail.properties</include>  
        </includes>  
    </resource>  
    <!-- Include other files as resources files. -->  
    <resource>  
        <directory>src/main/resources</directory>  
        <filtering>false</filtering>  
        <excludes>  
            <exclude>jdbc.properties</exclude>  
            <exclude>mail.properties</exclude>  
        </excludes>  
    </resource>  
</resources>

```

#### plugins

##### 跳过 Unit test

```

<project>
  [...]
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-surefire-plugin</artifactId>
        <configuration>
          <skip>true</skip>
        </configuration>
      </plugin>
    </plugins>
  </build>
  [...]
</project>			

```

##### maven-shade-plugin

```

			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-shade-plugin</artifactId>
				<version>1.4</version>
				<executions>
					<execution>
						<phase>package</phase>
						<goals>
							<goal>shade</goal>
						</goals>
						<configuration>
							<transformers>
								<transformer implementation="org.apache.maven.plugins.shade.resource.ManifestResourceTransformer">
									<mainClass>cn.netkiller.Oracle</mainClass>
								</transformer>
							</transformers>
						</configuration>
					</execution>
				</executions>
			</plugin>				

```

### Maven Module

对于大型互联网项目，不可能把所有代码都放在一个项目目录下，常常会将一个项目拆分成多个子项目

例如一个电商系统

1.  网站
2.  资讯中心
3.  商品中心
4.  物流中心
5.  订单中心
6.  客服中心
7.  后台

在开发过程中常常会遇到这种需求，有一个部分代码是共用的，所有项目都会使用到。所以需要将这部分代码独立成一个项目。

测试目录深度

```

Project
    |
    |--- common -> https://example.com/xxxx/common.git
    |     | ---pom.xml
    |--- project1
    |     |--- pom.xml
    |--- project2
    |     |--- pom.xml
    |---pom.xml	

```

common 是公共项目，独立仓库。通过 git submodule 技术挂载到项目目录。project1，project2 构建依赖 common 项目产生的 jar 包。

#### Parent

例 2.2. Maven parent

```

<?xml version="1.0" encoding="UTF-8"?>
<project   xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>

	<groupId>demo</groupId>
	<artifactId>maven</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>pom</packaging>

	<name>maven</name>
	<url>http://maven.apache.org</url>

	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<maven.compiler.source>11</maven.compiler.source>
		<maven.compiler.target>${maven.compiler.source}</maven.compiler.target>
		<junit.jupiter.version>5.4.0</junit.jupiter.version>
	</properties>

	<dependencies>

	</dependencies>
	<modules>
		<module>project1</module>
		<module>project2</module>
		<module>common</module>
	</modules>
	<build>

		<plugins>
			<plugin>
				<artifactId>maven-surefire-plugin</artifactId>
				<version>3.0.0-M3</version>
			</plugin>
			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.8.0</version>
			</plugin>

		</plugins>
	</build>
</project>

```

注意

```

<packaging>pom</packaging> 必须是 pom			

```

项目下面的子模块

```

    <modules>
		<module>project1</module>
		<module>project2</module>
		<module>common</module>
    </modules>			

```

#### 公共项目 common

例 2.3. watir-webdriver example

```

<?xml version="1.0"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"  >
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>demo</groupId>
		<artifactId>maven</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>
	<groupId>demo</groupId>
	<artifactId>common</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>common</name>
	<url>http://maven.apache.org</url>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>
	<dependencies>

	</dependencies>
</project>

```

添加 parent 标签, 声明项目的父子关系

```

    <parent>
		<groupId>demo</groupId>
		<artifactId>maven</artifactId>
		<version>0.0.1-SNAPSHOT</version>
    </parent>			

```

#### 常规项目

```

<?xml version="1.0"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"  >
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>demo</groupId>
		<artifactId>maven</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>
	<groupId>demo</groupId>
	<artifactId>project1</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>project1</name>
	<url>http://maven.apache.org</url>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>
	<dependencies>
		<dependency>
			<groupId>demo</groupId>
			<artifactId>common</artifactId>
			<version>0.0.1-SNAPSHOT</version>
		</dependency>
	</dependencies>
</project>

```

声明项目的父子关系

```

    <parent>
		<groupId>demo</groupId>
		<artifactId>maven</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>			

```

由于 project1 依赖 common 加入下面依赖

```

		<dependency>
			<groupId>demo</groupId>
			<artifactId>common</artifactId>
			<version>0.0.1-SNAPSHOT</version>
		</dependency>			

```

project2 跟 project1 类似

#### 现在测试效果

在父项目目录运行 mvn package

```

neo@MacBook-Pro ~/workspace/maven % mvn package
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO] 
[INFO] maven                                                              [pom]
[INFO] common                                                             [jar]
[INFO] project1                                                           [jar]
[INFO] project2                                                           [jar]
[INFO] 
[INFO] -----------------------------< demo:maven >-----------------------------
[INFO] Building maven 0.0.1-SNAPSHOT                                      [1/4]
[INFO] --------------------------------[ pom ]---------------------------------
[INFO] 
[INFO] ----------------------------< demo:common >-----------------------------
[INFO] Building common 0.0.1-SNAPSHOT                                     [2/4]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ common ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/neo/workspace/maven/common/src/main/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ common ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ common ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/neo/workspace/maven/common/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ common ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- maven-surefire-plugin:3.0.0-M3:test (default-test) @ common ---
[INFO] 
[INFO] -------------------------------------------------------
[INFO]  T E S T S
[INFO] -------------------------------------------------------
[INFO] 
[INFO] Results:
[INFO] 
[INFO] Tests run: 0, Failures: 0, Errors: 0, Skipped: 0
[INFO] 
[INFO] 
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ common ---
[INFO] Building jar: /Users/neo/workspace/maven/common/target/common-0.0.1-SNAPSHOT.jar
[INFO] META-INF/maven/demo/common/pom.xml already added, skipping
[INFO] META-INF/maven/demo/common/pom.properties already added, skipping
[INFO] 
[INFO] ---------------------------< demo:project1 >----------------------------
[INFO] Building project1 0.0.1-SNAPSHOT                                   [3/4]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ project1 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/neo/workspace/maven/project1/src/main/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ project1 ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ project1 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/neo/workspace/maven/project1/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ project1 ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to /Users/neo/workspace/maven/project1/target/test-classes
[INFO] 
[INFO] --- maven-surefire-plugin:3.0.0-M3:test (default-test) @ project1 ---
[INFO] 
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ project1 ---
[INFO] Building jar: /Users/neo/workspace/maven/project1/target/project1-0.0.1-SNAPSHOT.jar
[INFO] META-INF/maven/demo/project1/pom.xml already added, skipping
[INFO] META-INF/maven/demo/project1/pom.properties already added, skipping
[INFO] 
[INFO] ---------------------------< demo:project2 >----------------------------
[INFO] Building project2 0.0.1-SNAPSHOT                                   [4/4]
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ project2 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/neo/workspace/maven/project2/src/main/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ project2 ---
[INFO] Nothing to compile - all classes are up to date
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ project2 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/neo/workspace/maven/project2/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ project2 ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to /Users/neo/workspace/maven/project2/target/test-classes
[INFO] 
[INFO] --- maven-surefire-plugin:3.0.0-M3:test (default-test) @ project2 ---
[INFO] 
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ project2 ---
[INFO] Building jar: /Users/neo/workspace/maven/project2/target/project2-0.0.1-SNAPSHOT.jar
[INFO] META-INF/maven/demo/project2/pom.xml already added, skipping
[INFO] META-INF/maven/demo/project2/pom.properties already added, skipping
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for maven 0.0.1-SNAPSHOT:
[INFO] 
[INFO] maven .............................................. SUCCESS [  0.006 s]
[INFO] common ............................................. SUCCESS [  2.317 s]
[INFO] project1 ........................................... SUCCESS [  0.539 s]
[INFO] project2 ........................................... SUCCESS [  0.101 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  3.115 s
[INFO] Finished at: 2019-02-28T17:03:29+08:00
[INFO] ------------------------------------------------------------------------
neo@MacBook-Pro ~/workspace/maven % 	

```

我们可以看到有三个包产生

```

neo@MacBook-Pro ~/workspace/maven % find . -iname '*.jar'
./project1/target/project1-0.0.1-SNAPSHOT.jar
./common/target/common-0.0.1-SNAPSHOT.jar
./project2/target/project2-0.0.1-SNAPSHOT.jar			

```

我们也可以单独编译子项目

```

neo@MacBook-Pro ~/workspace/maven/project1 % mvn package
[INFO] Scanning for projects...
[INFO] 
[INFO] ---------------------------< demo:project1 >----------------------------
[INFO] Building project1 0.0.1-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO] 
[INFO] --- maven-resources-plugin:2.6:resources (default-resources) @ project1 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/neo/workspace/maven/project1/src/main/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.0:compile (default-compile) @ project1 ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to /Users/neo/workspace/maven/project1/target/classes
[INFO] 
[INFO] --- maven-resources-plugin:2.6:testResources (default-testResources) @ project1 ---
[INFO] Using 'UTF-8' encoding to copy filtered resources.
[INFO] skip non existing resourceDirectory /Users/neo/workspace/maven/project1/src/test/resources
[INFO] 
[INFO] --- maven-compiler-plugin:3.8.0:testCompile (default-testCompile) @ project1 ---
[INFO] Changes detected - recompiling the module!
[INFO] Compiling 1 source file to /Users/neo/workspace/maven/project1/target/test-classes
[INFO] 
[INFO] --- maven-surefire-plugin:3.0.0-M3:test (default-test) @ project1 ---
[INFO] 
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ project1 ---
[INFO] Building jar: /Users/neo/workspace/maven/project1/target/project1-0.0.1-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  2.015 s
[INFO] Finished at: 2019-02-28T17:09:18+08:00
[INFO] ------------------------------------------------------------------------

```

共享库 common-0.0.1-SNAPSHOT.jar 已经安装到 ～/.m2 目录下。

```

neo@MacBook-Pro ~/workspace/maven/project1 % find ~/.m2 -iname 'common-0.0.1-SNAPSHOT.jar'
/Users/neo/.m2/repository/demo/common/0.0.1-SNAPSHOT/common-0.0.1-SNAPSHOT.jar		

```

### 依赖管理

#### 创建依赖模块

```

<?xml version="1.0"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"  >
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>cn.netkiller</groupId>
		<artifactId>parent</artifactId>
		<version>0.0.1-SNAPSHOT</version>
	</parent>
	<groupId>cn.netkiller</groupId>
	<artifactId>dependencies</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<packaging>pom</packaging>

	<name>dependencies</name>
	<url>http://maven.apache.org</url>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>
	<dependencies>
		<dependency>
			<groupId>org.apache.httpcomponents</groupId>
			<artifactId>httpclient</artifactId>
			<version>4.5.7</version>
		</dependency>	
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<version>3.8.1</version>
			<scope>test</scope>
		</dependency>
	</dependencies>

</project>

```

#### 引用依赖管理

```

<?xml version="1.0" encoding="UTF-8"?>
<project xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd"  >
	<modelVersion>4.0.0</modelVersion>
	<parent>
		<groupId>cn.netkiller</groupId>
		<version>0.0.1-SNAPSHOT</version>
		<artifactId>parent</artifactId>
	</parent>
	<artifactId>example</artifactId>
	<name>example</name>
	<url>http://maven.apache.org</url>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
	</properties>

	<dependencyManagement>
		<dependencies>
			<dependency>
				<groupId>cn.netkiller</groupId>
				<artifactId>dependencies</artifactId>
				<version>${project.parent.version}</version>
				<type>pom</type>
				<scope>import</scope>
			</dependency>
		</dependencies>
	</dependencyManagement>

	<dependencies>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-web</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.cloud</groupId>
			<artifactId>spring-cloud-starter-oauth2</artifactId>
		</dependency>

	</dependencies>
</project>

```

### plugins

#### maven-compiler-plugin

配置默认的 jdk 版本

```

	<properties>
      <maven.compiler.source>1.8</maven.compiler.source>
      <maven.compiler.target>1.8</maven.compiler.target>
      <maven.compiler.compilerVersion>1.7</maven.compiler.compilerVersion>
    </properties>			

```

在插件中配置

```

			<plugin>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.8.0</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
				</configuration>
			</plugin>			

```

禁止编译警告 -Xlint:unchecked，-Xlint:deprecation

```

			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.1</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
					<encoding>UTF-8</encoding>
					<compilerArgs>
						<arg>-verbose</arg>
						<arg>-Xlint:unchecked</arg>
						<arg>-Xlint:deprecation</arg>
					</compilerArgs>
				</configuration>
			</plugin>

```

```

			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.1</version>
				<configuration>
					<source>1.8</source>
					<target>1.8</target>
					<encoding>UTF-8</encoding>
					<compilerArgs>
						<arg>-verbose</arg>
						<arg>-Xlint:unchecked</arg>
						<arg>-Xlint:deprecation</arg>
						<arg>-bootclasspath</arg>
						<arg>${env.JAVA_HOME}/jre/lib/rt.jar</arg>
						<arg>-extdirs</arg>
						<arg>${project.basedir}/libs</arg>
					</compilerArgs>
				</configuration>
			</plugin>

```

compilerArgs 可以实现编译参数的传递

```

<plugin>                                                                                                                                      

    <groupId>org.apache.maven.plugins</groupId>                                                                                               
    <artifactId>maven-compiler-plugin</artifactId>                                                                                            
    <version>3.1</version>                                                                                                                    
    <configuration>
	    <!-- 指定 maven 编译的 jdk 版本 -->      
        <!-- 一般而言，target 与 source 是保持一致的，但是，有时候为了让程序能在其他版本的 jdk 中运行(对于低版本目标 jdk，源代码中不能使用低版本 jdk 中不支持的语法)，会存在 target 不同于 source 的情况 -->                    
        <source>1.8</source> <!-- 源代码使用的 JDK 版本 -->                                                                                             
        <target>1.8</target> <!-- 需要生成的目标 class 文件的编译版本 -->                                                                                     
        <encoding>UTF-8</encoding><!-- 字符集编码 -->
        <skipTests>true</skipTests><!-- 跳过测试 -->                                                                             
        <verbose>true</verbose>
        <showWarnings>true</showWarnings>                                                                                                               
        <fork>true</fork><!-- 要使 compilerVersion 标签生效，还需要将 fork 设为 true，用于明确表示编译版本配置的可用 -->                                                        
        <executable><!-- path-to-javac --></executable><!-- 使用指定的 javac 命令，例如：<executable>${JAVA_1_4_HOME}/bin/javac</executable> -->           
        <compilerVersion>1.3</compilerVersion><!-- 指定插件将使用的编译器的版本 -->                                                                         
        <meminitial>128m</meminitial><!-- 编译器使用的初始内存 -->                                                                                      
        <maxmem>512m</maxmem><!-- 编译器使用的最大内存 -->                                                                                              
        <compilerArgument>-verbose -bootclasspath ${java.home}\lib\rt.jar</compilerArgument><!-- 这个选项用来传递编译器自身不包含但是却支持的参数选项 -->               
    </configuration>                                                                                                                          
</plugin>   			

```

#### maven-war-plugin

设置 war 文件名 warName

```

<project>
  ...
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-war-plugin</artifactId>
        <version>2.3</version>
        <configuration>
          <warName>bird.war</warName>
        </configuration>
      </plugin>
    </plugins>
  </build>
  ...
</project>			

```

#### maven-antrun-plugin

查看可用的 pom 定义

```

or alternatively, we can use an external build.xml.

<project>
  <modelVersion>4.0.0</modelVersion>
  <artifactId>my-test-app</artifactId>
  <groupId>my-test-group</groupId>
  <version>1.0-SNAPSHOT</version>

  <build>
    <plugins>

      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-antrun-plugin</artifactId>
        <version>1.8</version>
        <executions>
          <execution>
            <id>compile</id>
            <phase>compile</phase>
            <configuration>
              <target>
                <property name="compile_classpath" refid="maven.compile.classpath"/>
                <property name="runtime_classpath" refid="maven.runtime.classpath"/>
                <property name="test_classpath" refid="maven.test.classpath"/>
                <property name="plugin_classpath" refid="maven.plugin.classpath"/>

                <echo message="compile classpath: ${compile_classpath}"/>
                <echo message="runtime classpath: ${runtime_classpath}"/>
                <echo message="test classpath:    ${test_classpath}"/>
                <echo message="plugin classpath:  ${plugin_classpath}"/>

                <ant antfile="${basedir}/build.xml">
                  <target name="test"/>
                </ant>
              </target>
            </configuration>
            <goals>
              <goal>run</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    </plugins>
  </build>
</project>

The build.xml:

<?xml version="1.0"?>
<project name="test6">

    <target name="test">

      <echo message="compile classpath: ${compile_classpath}"/>
      <echo message="runtime classpath: ${runtime_classpath}"/>
      <echo message="test classpath:    ${test_classpath}"/>
      <echo message="plugin classpath:  ${plugin_classpath}"/>

    </target>

</project>

```

#### maven-install-plugin

```

		<plugin>
			<groupId>org.apache.maven.plugins</groupId>
			<artifactId>maven-install-plugin</artifactId>
			<version>2.5.2</version>
			<inherited>false</inherited>
			<executions>
				<execution>
					<id>install:com.oracle:ojdbc6:11g</id>
					<phase>validate</phase>
					<goals>
						<goal>install-file</goal>
					</goals>
					<configuration>
						<file>${project.basedir}/lib/ojdbc6.jar</file>
						<groupId>com.oracle</groupId>
						<artifactId>ojdbc6</artifactId>
						<version>11.2.0.3</version>
						<packaging>jar</packaging>
						<createChecksum>true</createChecksum>
						<generatePom>true</generatePom>
					</configuration>
				</execution>
			</executions>
		</plugin>

```

#### maven-surefire-plugin

```

	<plugin>
		<groupId>org.apache.maven.plugins</groupId>
		<artifactId>maven-surefire-plugin</artifactId>
		<version>2.20</version>
		<configuration>
			<skip>true</skip>
		</configuration>
	</plugin>		

```

#### maven-deploy-plugin

```

			<plugin>
		      <!--skip deploy (this is just a test module) -->
		      <artifactId>maven-deploy-plugin</artifactId>
		      <configuration>
		        <skip>true</skip>
		      </configuration>
	        </plugin>			

```

#### maven-jar-plugin

指定 jar 创建目录，下面配置运行 mvn package 后将在 target 目录下创建一个 project 目录。

```

	<plugin>
		<groupId>org.apache.maven.plugins</groupId>
		<artifactId>maven-jar-plugin</artifactId>
		<version>2.3.1</version>
		<configuration>
			<outputDirectory>${project.build.directory}/project</outputDirectory>
		</configuration>
	</plugin>

```

知道 jar 文件的默认 mainClass

```

			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-jar-plugin</artifactId>
				<version>2.6</version>
				<configuration>
					<outputDirectory>${project.build.directory}/project/lib</outputDirectory>
					<archive>
						<manifest>
							<addClasspath>true</addClasspath>
							<classpathPrefix>lib/</classpathPrefix>
							<mainClass>cn.netkiller.web.App</mainClass>
						</manifest>
					</archive>
				</configuration>
			</plugin>

```

#### maven-dependency-plugin

```

			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-dependency-plugin</artifactId>
				<!-- <version>2.10</version> -->
				<executions>
					<execution>
						<id>copy-dependencies</id>
						<phase>package</phase>
						<goals>
							<goal>copy-dependencies</goal>
						</goals>
						<configuration>
							<!-- 把依赖的所有 maven jar 包拷贝到 lib 目录中（这样所有的 jar 包都在 lib 目录中） -->
							<outputDirectory>${project.build.directory}/project/lib</outputDirectory>
							<overWriteReleases>false</overWriteReleases>
							<overWriteSnapshots>false</overWriteSnapshots>
							<overWriteIfNewer>true</overWriteIfNewer>
						</configuration>
					</execution>
				</executions>
			</plugin>

```

#### spring-boot-maven-plugin

```

			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<configuration>
					<mainClass>cn.netkiller.Application</mainClass>
				</configuration>
			</plugin>

```

```

MacBook-Pro:api.netkiller.cn neo$ mvn help:describe -DgroupId=org.springframework.boot                   -DartifactId=spring-boot-maven-plugin                   -Ddetail=true
[INFO] Scanning for projects...
[INFO] 
[INFO] ------------------------------------------------------------------------
[INFO] Building api 0.0.1-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-help-plugin:2.2:describe (default-cli) @ api ---
[INFO] org.springframework.boot:spring-boot-maven-plugin:1.5.6.RELEASE

Name: Spring Boot Maven Plugin
Description: Spring Boot Maven Plugin
Group Id: org.springframework.boot
Artifact Id: spring-boot-maven-plugin
Version: 1.5.6.RELEASE
Goal Prefix: spring-boot

This plugin has 6 goals:

spring-boot:build-info
  Description: Generate a build-info.properties file based the content of the
    current MavenProject.
  Implementation: org.springframework.boot.maven.BuildInfoMojo
  Language: java
  Bound to phase: generate-resources

  Available parameters:

    additionalProperties
      Additional properties to store in the build-info.properties. Each entry
      is prefixed by build. in the generated build-info.properties.

    outputFile (Default:
    ${project.build.outputDirectory}/META-INF/build-info.properties)
      The location of the generated build-info.properties.

spring-boot:help
  Description: Display help information on spring-boot-maven-plugin.
    Call mvn spring-boot:help -Ddetail=true -Dgoal=<goal-name> to display
    parameter details.
  Implementation: org.springframework.boot.maven.HelpMojo
  Language: java

  Available parameters:

    detail (Default: false)
      User property: detail
      If true, display all settable properties for each goal.

    goal
      User property: goal
      The name of the goal for which to show help. If unspecified, all goals
      will be displayed.

    indentSize (Default: 2)
      User property: indentSize
      The number of spaces per indentation level, should be positive.

    lineLength (Default: 80)
      User property: lineLength
      The maximum length of a display line, should be positive.

spring-boot:repackage
  Description: Repackages existing JAR and WAR archives so that they can be
    executed from the command line using java -jar. With layout=NONE can also
    be used simply to package a JAR with nested dependencies (and no main
    class, so not executable).
  Implementation: org.springframework.boot.maven.RepackageMojo
  Language: java
  Bound to phase: package

  Available parameters:

    attach (Default: true)
      Attach the repackaged archive to be installed and deployed.

    classifier
      Classifier to add to the artifact generated. If given, the artifact will
      be attached with that classifier and the main artifact will be deployed
      as the main artifact. If this is not given (default), it will replace the
      main artifact and only the repackaged artifact will be deployed.
      Attaching the artifact allows to deploy it alongside to the original one,
      see the maven documentation for more details.

    embeddedLaunchScript
      The embedded launch script to prepend to the front of the jar if it is
      fully executable. If not specified the 'Spring Boot' default script will
      be used.

    embeddedLaunchScriptProperties
      Properties that should be expanded in the embedded launch script.

    excludeArtifactIds
      User property: excludeArtifactIds
      Comma separated list of artifact names to exclude (exact match).

    excludeDevtools (Default: true)
      Exclude Spring Boot devtools from the repackaged archive.

    excludeGroupIds
      User property: excludeGroupIds
      Comma separated list of groupId names to exclude (exact match).

    excludes
      Collection of artifact definitions to exclude. The Exclude element
      defines a groupId and artifactId mandatory properties and an optional
      classifier property.

    executable (Default: false)
      Make a fully executable jar for *nix machines by prepending a launch
      script to the jar.
      Currently, some tools do not accept this format so you may not always be
      able to use this technique. For example, jar -xf may silently fail to
      extract a jar or war that has been made fully-executable. It is
      recommended that you only enable this option if you intend to execute it
      directly, rather than running it with java -jar or deploying it to a
      servlet container.

    finalName (Default: ${project.build.finalName})
      Required: true
      Name of the generated archive.

    includes
      Collection of artifact definitions to include. The Include element
      defines a groupId and artifactId mandatory properties and an optional
      classifier property.

    includeSystemScope (Default: false)
      Include system scoped dependencies.

    layout
      The type of archive (which corresponds to how the dependencies are laid
      out inside it). Possible values are JAR, WAR, ZIP, DIR, NONE. Defaults to
      a guess based on the archive type.

    layoutFactory
      The layout factory that will be used to create the executable archive if
      no explicit layout is set. Alternative layouts implementations can be
      provided by 3rd parties.

    mainClass
      The name of the main class. If not specified the first compiled class
      found that contains a 'main' method will be used.

    outputDirectory (Default: ${project.build.directory})
      Required: true
      Directory containing the generated archive.

    requiresUnpack
      A list of the libraries that must be unpacked from fat jars in order to
      run. Specify each library as a <dependency> with a <groupId> and a
      <artifactId> and they will be unpacked at runtime.

    skip (Default: false)
      User property: skip
      Skip the execution.

spring-boot:run
  Description: Run an executable archive application.
  Implementation: org.springframework.boot.maven.RunMojo
  Language: java
  Bound to phase: validate
  Before this mojo executes, it will call:
    Phase: 'test-compile'

  Available parameters:

    addResources (Default: false)
      User property: run.addResources
      Add maven resources to the classpath directly, this allows live in-place
      editing of resources. Duplicate resources are removed from target/classes
      to prevent them to appear twice if ClassLoader.getResources() is called.
      Please consider adding spring-boot-devtools to your project instead as it
      provides this feature and many more.

    agent
      User property: run.agent
      Path to agent jar. NOTE: the use of agents means that processes will be
      started by forking a new JVM.

    arguments
      User property: run.arguments
      Arguments that should be passed to the application. On command line use
      commas to separate multiple arguments.

    classesDirectory (Default: ${project.build.outputDirectory})
      Required: true
      Directory containing the classes and resource files that should be
      packaged into the archive.

    excludeArtifactIds
      User property: excludeArtifactIds
      Comma separated list of artifact names to exclude (exact match).

    excludeGroupIds
      User property: excludeGroupIds
      Comma separated list of groupId names to exclude (exact match).

    excludes
      Collection of artifact definitions to exclude. The Exclude element
      defines a groupId and artifactId mandatory properties and an optional
      classifier property.

    folders
      Additional folders besides the classes directory that should be added to
      the classpath.

    fork
      User property: fork
      Flag to indicate if the run processes should be forked. fork is
      automatically enabled if an agent, jvmArguments or working directory are
      specified, or if devtools is present.

    includes
      Collection of artifact definitions to include. The Include element
      defines a groupId and artifactId mandatory properties and an optional
      classifier property.

    jvmArguments
      User property: run.jvmArguments
      JVM arguments that should be associated with the forked process used to
      run the application. On command line, make sure to wrap multiple values
      between quotes. NOTE: the use of JVM arguments means that processes will
      be started by forking a new JVM.

    mainClass
      The name of the main class. If not specified the first compiled class
      found that contains a 'main' method will be used.

    noverify
      User property: run.noverify
      Flag to say that the agent requires -noverify.

    profiles
      User property: run.profiles
      The spring profiles to activate. Convenience shortcut of specifying the
      'spring.profiles.active' argument. On command line use commas to separate
      multiple profiles.

    skip (Default: false)
      User property: skip
      Skip the execution.

    useTestClasspath (Default: false)
      User property: useTestClasspath
      Flag to include the test classpath when running.

    workingDirectory
      User property: run.workingDirectory
      Current working directory to use for the application. If not specified,
      basedir will be used. NOTE: the use of working directory means that
      processes will be started by forking a new JVM.

spring-boot:start
  Description: Start a spring application. Contrary to the run goal, this
    does not block and allows other goal to operate on the application. This
    goal is typically used in integration test scenario where the application
    is started before a test suite and stopped after.
  Implementation: org.springframework.boot.maven.StartMojo
  Language: java
  Bound to phase: pre-integration-test

  Available parameters:

    addResources (Default: false)
      User property: run.addResources
      Add maven resources to the classpath directly, this allows live in-place
      editing of resources. Duplicate resources are removed from target/classes
      to prevent them to appear twice if ClassLoader.getResources() is called.
      Please consider adding spring-boot-devtools to your project instead as it
      provides this feature and many more.

    agent
      User property: run.agent
      Path to agent jar. NOTE: the use of agents means that processes will be
      started by forking a new JVM.

    arguments
      User property: run.arguments
      Arguments that should be passed to the application. On command line use
      commas to separate multiple arguments.

    classesDirectory (Default: ${project.build.outputDirectory})
      Required: true
      Directory containing the classes and resource files that should be
      packaged into the archive.

    excludeArtifactIds
      User property: excludeArtifactIds
      Comma separated list of artifact names to exclude (exact match).

    excludeGroupIds
      User property: excludeGroupIds
      Comma separated list of groupId names to exclude (exact match).

    excludes
      Collection of artifact definitions to exclude. The Exclude element
      defines a groupId and artifactId mandatory properties and an optional
      classifier property.

    folders
      Additional folders besides the classes directory that should be added to
      the classpath.

    fork
      User property: fork
      Flag to indicate if the run processes should be forked. fork is
      automatically enabled if an agent, jvmArguments or working directory are
      specified, or if devtools is present.

    includes
      Collection of artifact definitions to include. The Include element
      defines a groupId and artifactId mandatory properties and an optional
      classifier property.

    jmxName
      The JMX name of the automatically deployed MBean managing the lifecycle
      of the spring application.

    jmxPort
      The port to use to expose the platform MBeanServer if the application
      needs to be forked.

    jvmArguments
      User property: run.jvmArguments
      JVM arguments that should be associated with the forked process used to
      run the application. On command line, make sure to wrap multiple values
      between quotes. NOTE: the use of JVM arguments means that processes will
      be started by forking a new JVM.

    mainClass
      The name of the main class. If not specified the first compiled class
      found that contains a 'main' method will be used.

    maxAttempts
      The maximum number of attempts to check if the spring application is
      ready. Combined with the 'wait' argument, this gives a global timeout
      value (30 sec by default)

    noverify
      User property: run.noverify
      Flag to say that the agent requires -noverify.

    profiles
      User property: run.profiles
      The spring profiles to activate. Convenience shortcut of specifying the
      'spring.profiles.active' argument. On command line use commas to separate
      multiple profiles.

    skip (Default: false)
      User property: skip
      Skip the execution.

    useTestClasspath (Default: false)
      User property: useTestClasspath
      Flag to include the test classpath when running.

    wait
      The number of milli-seconds to wait between each attempt to check if the
      spring application is ready.

    workingDirectory
      User property: run.workingDirectory
      Current working directory to use for the application. If not specified,
      basedir will be used. NOTE: the use of working directory means that
      processes will be started by forking a new JVM.

spring-boot:stop
  Description: Stop a spring application that has been started by the 'start'
    goal. Typically invoked once a test suite has completed.
  Implementation: org.springframework.boot.maven.StopMojo
  Language: java
  Bound to phase: post-integration-test

  Available parameters:

    fork
      User property: fork
      Flag to indicate if process to stop was forked. By default, the value is
      inherited from the MavenProject. If it is set, it must match the value
      used to StartMojo start the process.

    jmxName
      The JMX name of the automatically deployed MBean managing the lifecycle
      of the application.

    jmxPort
      The port to use to lookup the platform MBeanServer if the application has
      been forked.

    skip (Default: false)
      User property: skip
      Skip the execution.

[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 0.976 s
[INFO] Finished at: 2017-08-03T15:05:53+08:00
[INFO] Final Memory: 12M/155M
[INFO] ------------------------------------------------------------------------			

```

#### tomcat8-maven-plugin

```

			<plugin>
				<groupId>org.apache.maven.plugins</groupId>
				<artifactId>maven-compiler-plugin</artifactId>
				<version>3.5.1</version>
			</plugin>
			<plugin>
				<groupId>org.apache.tomcat.maven</groupId>
				<artifactId>tomcat8-maven-plugin</artifactId>
				<version>2.2</version>
				<configuration>
					<url>http://localhost:8082/manager/text</url>
					<server>tomcat</server>
					<username>admin</username>
					<password>admin@123456</password>
					<path>/${project.build.finalName}</path>
					<!-- war 文件路径缺省情况下指向 target -->
					<!--<warFile>${basedir}/target/${project.build.finalName}.war</warFile> -->
				</configuration>
			</plugin>		

```

```

mvn tomcat:deploy			

```

## Gradle 5

### 安装 Gradle

#### CentOS

安装 Gradle

```

curl -s https://raw.githubusercontent.com/oscm/shell/38c2d4d307ce7a0760fc88d5d9703eef64d3b81c/lang/java/gradle/gradle-2.9.sh | bash

```

安装后运行 gradle --help 验证释放正确安装。

```

neo@MacBook-Pro ~ % gradle --help

Welcome to Gradle 5.1.1!

Here are the highlights of this release:
 - Control which dependencies can be retrieved from which repositories
 - Production-ready configuration avoidance APIs

For more details see https://docs.gradle.org/5.1.1/release-notes.html

USAGE: gradle [option...] [task...]

-?, -h, --help            Shows this help message.
-a, --no-rebuild          Do not rebuild project dependencies.
-b, --build-file          Specify the build file.
--build-cache             Enables the Gradle build cache. Gradle will try to reuse outputs from previous builds.
-c, --settings-file       Specify the settings file.
--configure-on-demand     Configure necessary projects only. Gradle will attempt to reduce configuration time for large multi-project builds. [incubating]
--console                 Specifies which type of console output to generate. Values are 'plain', 'auto' (default), 'rich' or 'verbose'.
--continue                Continue task execution after a task failure.
-D, --system-prop         Set system property of the JVM (e.g. -Dmyprop=myvalue).
-d, --debug               Log in debug mode (includes normal stacktrace).
--daemon                  Uses the Gradle Daemon to run the build. Starts the Daemon if not running.
--foreground              Starts the Gradle Daemon in the foreground.
-g, --gradle-user-home    Specifies the gradle user home directory.
-I, --init-script         Specify an initialization script.
-i, --info                Set log level to info.
--include-build           Include the specified build in the composite.
-m, --dry-run             Run the builds with all task actions disabled.
--max-workers             Configure the number of concurrent workers Gradle is allowed to use.
--no-build-cache          Disables the Gradle build cache.
--no-configure-on-demand  Disables the use of configuration on demand. [incubating]
--no-daemon               Do not use the Gradle daemon to run the build. Useful occasionally if you have configured Gradle to always run with the daemon by default.
--no-parallel             Disables parallel execution to build projects.
--no-scan                 Disables the creation of a build scan. For more information about build scans, please visit https://gradle.com/build-scans.
--offline                 Execute the build without accessing network resources.
-P, --project-prop        Set project property for the build script (e.g. -Pmyprop=myvalue).
-p, --project-dir         Specifies the start directory for Gradle. Defaults to current directory.
--parallel                Build projects in parallel. Gradle will attempt to determine the optimal number of executor threads to use.
--priority                Specifies the scheduling priority for the Gradle daemon and all processes launched by it. Values are 'normal' (default) or 'low' [incubating]
--profile                 Profile build execution time and generates a report in the <build_dir>/reports/profile directory.
--project-cache-dir       Specify the project-specific cache directory. Defaults to .gradle in the root project directory.
-q, --quiet               Log errors only.
--refresh-dependencies    Refresh the state of dependencies.
--rerun-tasks             Ignore previously cached task results.
-S, --full-stacktrace     Print out the full (very verbose) stacktrace for all exceptions.
-s, --stacktrace          Print out the stacktrace for all exceptions.
--scan                    Creates a build scan. Gradle will emit a warning if the build scan plugin has not been applied. (https://gradle.com/build-scans)
--status                  Shows status of running and recently stopped Gradle Daemon(s).
--stop                    Stops the Gradle Daemon if it is running.
-t, --continuous          Enables continuous build. Gradle does not exit and will re-execute tasks when task file inputs change.
--update-locks            Perform a partial update of the dependency lock, letting passed in module notations change version. [incubating]
-v, --version             Print version info.
-w, --warn                Set log level to warn.
--warning-mode            Specifies which mode of warnings to generate. Values are 'all', 'summary'(default) or 'none'
--write-locks             Persists dependency resolution for locked configurations, ignoring existing locking information if it exists [incubating]
-x, --exclude-task        Specify a task to be excluded from execution.

```

#### Mac

```

neo@MacBook-Pro ~ %  brew install gradle

```

### Example

```

mkdir -p src/main/java/hello

vim src/main/java/hello/HelloWorld.java

```

```

package hello;

public class HelloWorld {
  public static void main(String[] args) {
    System.out.println("Helloworld!!!");
  }
}

```

```

$ gradle build
$ gradle run -q
Helloworld!!!

```

### gradle 命令

#### tasks 列出任务

```

[neo@netkiller test]$ gradle tasks
:tasks

------------------------------------------------------------
All tasks runnable from root project
------------------------------------------------------------

Application tasks
-----------------
installApp - Installs the project as a JVM application along with libs and OS specific scripts.
run - Runs this project as a JVM application

Build tasks
-----------
assemble - Assembles the outputs of this project.
build - Assembles and tests this project.
buildDependents - Assembles and tests this project and all projects that depend on it.
buildNeeded - Assembles and tests this project and all projects it depends on.
classes - Assembles main classes.
clean - Deletes the build directory.
jar - Assembles a jar archive containing the main classes.
testClasses - Assembles test classes.

Build Setup tasks
-----------------
init - Initializes a new Gradle build. [incubating]

Distribution tasks
------------------
assembleDist - Assembles the main distributions
distTar - Bundles the project as a distribution.
distZip - Bundles the project as a distribution.
installDist - Installs the project as a distribution as-is.

Documentation tasks
-------------------
javadoc - Generates Javadoc API documentation for the main source code.

Help tasks
----------
components - Displays the components produced by root project 'test'. [incubating]
dependencies - Displays all dependencies declared in root project 'test'.
dependencyInsight - Displays the insight into a specific dependency in root project 'test'.
help - Displays a help message.
model - Displays the configuration model of root project 'test'. [incubating]
projects - Displays the sub-projects of root project 'test'.
properties - Displays the properties of root project 'test'.
tasks - Displays the tasks runnable from root project 'test'.

Verification tasks
------------------
check - Runs all checks.
test - Runs the unit tests.

Other tasks
-----------
wrapper

Rules
-----
Pattern: clean<TaskName>: Cleans the output files of a task.
Pattern: build<ConfigurationName>: Assembles the artifacts of a configuration.
Pattern: upload<ConfigurationName>: Assembles and uploads the artifacts belonging to a configuration.

To see all tasks and more detail, run gradle tasks --all

To see more detail about a task, run gradle help --task <task>

BUILD SUCCESSFUL

Total time: 5.157 secs

```

### build.gradle

apply plugin

```

apply plugin: 'java'		

```

#### repositories

```

repositories {
    mavenCentral()
}			

```

配置阿里云仓库

```

allprojects {
    repositories {
        maven{ url 'http://maven.aliyun.com/nexus/content/groups/public/'}
    }
}			

```

#### dependencies

```

	dependencies {
		compile 'org.springframework:spring-context:4.2.2.RELEASE'
	}

```

#### jar

```

	jar {
		baseName = 'hello'
		version = '0.1.0'
	}

```

设置 Main-Class

```

jar {
    manifest {
        attributes 'Main-Class': 'demo.Test'
        attributes 'Class-Path': 'junit5.jar'
    }
}			

```

### gradle.properties

#### 列出 properties

```

[neo@netkiller gradle]$ gradle properties
:properties

------------------------------------------------------------
Root project
------------------------------------------------------------

allprojects: [root project 'gradle']
ant: org.gradle.api.internal.project.DefaultAntBuilder@12072edc
antBuilderFactory: org.gradle.api.internal.project.DefaultAntBuilderFactory@159576c3
artifacts: org.gradle.api.internal.artifacts.dsl.DefaultArtifactHandler_Decorated@7a80747
asDynamicObject:
org.gradle.api.internal.ExtensibleDynamicObject@2875ca3e
baseClassLoaderScope: org.gradle.api.internal.initialization.DefaultClassLoaderScope@4d30c132
buildDir: /opt/www/gradle/build
buildFile: /opt/www/gradle/build.gradle
buildScriptSource: org.gradle.groovy.scripts.UriScriptSource@3bdbe135
buildscript: org.gradle.api.internal.initialization.DefaultScriptHandler@609e7d46
childProjects: {}
class: class org.gradle.api.internal.project.DefaultProject_Decorated
classLoaderScope:
org.gradle.api.internal.initialization.DefaultClassLoaderScope@4532b038
clone: task ':clone'
compile: task ':compile'
compileTest: task ':compileTest'
components: []
configurationActions: org.gradle.configuration.project.DefaultProjectConfigurationActionContainer@2cf5006
configurations: []
convention: org.gradle.api.internal.plugins.DefaultConvention@788ebb5a
defaultTasks: []
deferredProjectConfiguration: org.gradle.api.internal.project.DeferredProjectConfiguration@62ae4f8b
dependencies:
org.gradle.api.internal.artifacts.dsl.dependencies.DefaultDependencyHandler_Decorated@21e8614a
depth: 0
description: null
dist: task ':dist'
ext: org.gradle.api.internal.plugins.DefaultExtraPropertiesExtension@1f4b52aa
extensions: org.gradle.api.internal.plugins.DefaultConvention@788ebb5a
fileOperations: org.gradle.api.internal.file.DefaultFileOperations@a2026f3
fileResolver: org.gradle.api.internal.file.BaseDirFileResolver@44dd20b6
gradle: build 'gradle'
group:
inheritedScope:
org.gradle.api.internal.ExtensibleDynamicObject$InheritedDynamicObject@118eb00c
logger: org.gradle.logging.internal.slf4j.OutputEventListenerBackedLogger@2ec7ecd5
logging: org.gradle.logging.internal.DefaultLoggingManager@478dabf1
modelRegistry: org.gradle.model.internal.registry.DefaultModelRegistry@26137fea
modelSchemaStore: org.gradle.model.internal.manage.schema.extract.DefaultModelSchemaStore@4a32ef2d
module: org.gradle.api.internal.artifacts.ProjectBackedModule@31bca1c3
name: gradle
parent: null
parentIdentifier: null
path: :
pluginManager: org.gradle.api.internal.plugins.DefaultPluginManager_Decorated@55f49969
plugins: [org.gradle.api.plugins.HelpTasksPlugin@4f88f506]
processOperations: org.gradle.api.internal.file.DefaultFileOperations@a2026f3
project: root project 'gradle'
project.dir: repo
project.url: git@172.16.0.1:example.com/admin.example.com.git
projectDir: /opt/www/gradle
projectEvaluationBroadcaster: ProjectEvaluationListener broadcast
projectEvaluator:
org.gradle.configuration.project.LifecycleProjectEvaluator@19035ff9
projectRegistry: org.gradle.api.internal.project.DefaultProjectRegistry@2c91e143
properties: {...}
pull: task ':pull'
repositories: []
resources: org.gradle.api.internal.resources.DefaultResourceHandler@1d5c0c91
rootDir: /opt/www/gradle
rootProject: root project 'gradle'
scriptHandlerFactory: org.gradle.api.internal.initialization.DefaultScriptHandlerFactory@63d12a6
scriptPluginFactory:
org.gradle.configuration.DefaultScriptPluginFactory@1393537d
serviceRegistryFactory: org.gradle.internal.service.scopes.ProjectScopeServices$4@2d4e3d95
services: ProjectScopeServices
standardOutputCapture: org.gradle.logging.internal.DefaultLoggingManager@478dabf1
state: project state 'EXECUTED'
status: release
stop: task ':stop'
subprojects: []
tasks: [task ':clone', task ':compile', task ':compileTest', task ':dist', task ':properties', task ':pull', task ':stop', task ':test']
test: task ':test'
version:
unspecified

BUILD SUCCESSFUL

Total time: 4.672 secs

```

#### 自定义 gradle.properties

```

[neo@netkiller gradle]$ cat gradle.properties
Name=Netkiller
Email=netkiller@msn.com

```

```

[neo@netkiller gradle]$ gradle properties | egrep "Name|Email"
Email: netkiller@msn.com
Name: Netkiller

```

```

[neo@netkiller gradle]$ cat build.gradle 
task hello << {
     println "hello, $Name<$Email>"
}

```

```

[neo@netkiller gradle]$ gradle hello -q
hello, Netkiller<netkiller@msn.com>

```

通过 systemProp 前缀传递 System.properties 参数

```

[neo@netkiller gradle]$ cat gradle.properties 
systemProp.Name = 'Neo Chen'

[neo@netkiller gradle]$ cat build.gradle

task hello << {
      println "hello, " + System.properties['Name']
}				

```

```

ext {
     Name='Neo'
}

task hello << {
     println "hello, $Name"
}				

```

#### System.properties

-D 参数传递

```

task hello << {
     println System.properties['Name']
}

$ gradle hello -DName='Neo' -q
Neo

```

-P 参数传递

```

task hello << {
      println "hello, $Name"
}

$ gradle hello -PName='Neo' -q
hello, Neo				

```

## JitPack - Easy to use package repository for Git

### JitPack

Easy to use package repository for Git

Publish your JVM and Android libraries

## Artifactory

### Artifactory Web UI

通过访问 http://localhost:8081/artifactory/ ，用默认管理员账号密码(用户名：admin，密码：password ) 登录，进入管理界面体验 Artifactory。

### build.gradle

```

buildscript {
    repositories {

        maven {
            url "http://artifactory.netkiller.cn:8081/artifactory/libs-release-local"
        }

        jcenter()
    }
    dependencies {
        ...
    }
}

allprojects {
    repositories {
        maven {
            url "http://artifactory.netkiller.cn:8081/artifactory/libs-release-local"
        }

        jcenter()
    }
}

```

Artifactory 默认将 jecnter 库缓存到私有 maven 仓库中。我们可以在项目中配置 jcenter 仓库信息列表，后续编译所需要的包都直接从私服 maven 仓库读取，加快项目编译速度。

```

buildscript {
    repositories {
        maven {
            url "http://artifactory.netkiller.cn:8081/artifactory/jcenter" // 配置私服 jcenter 仓库信息
        }

        maven {
            url "http://artifactory.netkiller.cn:8081/artifactory/libs-release-local"
        }

        jcenter()
    }
    dependencies {
       ...
    }
}

allprojects {
    repositories {
        maven {
            url "http://artifactory.netkiller.cn:8081/artifactory/jcenter" // 配置私服 jcenter 仓库信息
        }

        maven {
            url "http://artifactory.netkiller.cn:8081/artifactory/libs-release-local"
        }

        jcenter()
    }
}		

```