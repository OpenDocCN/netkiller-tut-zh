# 部分 XIII. 软件项目管理工具

## project management tool

## 1. Nexus Repository OSS

[`www.sonatype.com/download-oss-sonatype`](https://www.sonatype.com/download-oss-sonatype)

```

wget https://www.sonatype.com/oss-thank-you-tar.gz	

```

### 1.1. 安装 Nexus

#### 1.1.1. Docker

```

docker run -d -p 8081:8081 --restart=always --name nexus sonatype/nexus3			

```

### 1.2. Nexus UI

http://localhost:8081/ 登陆用户名 admin 默认密码 admin123

### 1.3. maven 设置

maven 在 settings.xml 中配置如下，下次 maven 就会通过访问电脑上的私服来获取 jar 包

```

<mirrors>
    <mirror>
      <id>nexus</id>
      <mirrorOf>*</mirrorOf>
      <url>http://localhost:8081/repository/maven-public/</url>
    </mirror>
</mirrors>

```

### 1.4. Node.js

输入命令登陆远程仓库

```

npm login --registry=http://nexus.netkiller.cn/repository/npm/		

```

在项目中输入

```

npm pack	

```

上传

```

npm publish --registry=http://nexus.netkiller.cn/repository/npm/		

```

### 1.5. Ruby

安装 nexus 包

```

$ gem install nexus		

```

打包

```

gem build project.gemspec

```

上传，系统会提示上传 URL

```

gem nexus project-1.0.0.gem

```

## 第 170 章 TRAC

[`trac.edgewall.org`](http://trac.edgewall.org/)

### 1. Ubuntu 安装

#### 1.1. source code

过程 170.1. TRAC - source

1.  setup.py

    ```
    wget http://peak.telecommunity.com/dist/ez_setup.py
    python ez_setup.py

    ```

2.  Trac

    ```
    wget http://download.edgewall.org/trac/Trac-1.1.1.tar.gz
    tar zxvf Trac-1.1.1.tar.gz
    cd Trac-1.1.1
    sudo python ./setup.py install
    cd ..

    ```

#### 1.2. easy_install

过程 170.2. TRAC - easy_install

1.  easy_install

    ```
    $ sudo apt-get install python-setuptools

    ```

2.  Installing Trac

    ```
    sudo easy_install Pygments
    sudo easy_install Genshi
    sudo easy_install Trac

    ```

    ClearSilver

    ```
    sudo apt-get install python-clearsilver

    ```

    python svn

    ```
     sudo apt-get install python-svn python-svn-dbg

    ```

    create svn repos

    ```
    $ svnadmin create /home/netkiller/repos

    ```

#### 1.3. Apache httpd

```

# cat /etc/httpd/conf.d/trac.conf
<VirtualHost *:80>
  # Change this to the domain which points to your host, i.e. the domain
  # you set as "phabricator.base-uri".
  ServerName trac.repo

  <Location />
    SetHandler mod_python
    PythonInterpreter main_interpreter
    PythonHandler trac.web.modpython_frontend
    PythonOption TracEnv /gitroot/trac/default
    PythonOption TracUriRoot /
  </Location>
# Replace all occurrences of /srv/trac with your trac root below
# and uncomment the respective SetEnv and PythonOption directives.
#  <LocationMatch /cgi-bin/trac\.f?cgi>
#	SetEnv TRAC_ENV /srv/trac
#  </LocationMatch>
#  <IfModule mod_python.c>
#    <Location /cgi-bin/trac.cgi>
#      SetHandler mod_python
#      PythonHandler trac.web.modpython_frontend
#      #PythonOption TracEnv /srv/trac
#    </Location>
#  </IfModule>
</VirtualHost>

```

### 2. CentOS 安装

http://trac.edgewall.org/

```
[root@development ~]# yum install python-setuptools
[root@development ~]# easy_install Trac

[root@development ~]# trac-admin /var/www/myproject initenv

```

#### 2.1. trac.ini

subversion 仓库配置

```
vim /srv/example/conf/trac.ini

repository_dir = /svnroot/example.com

```

#### 2.2. standalone

```
tracd -s --port 8000 /var/www/myproject

```

multiple projects

```
tracd --port 8000 /var/www/trac/project1/ /var/www/trac/project2 ...
or
tracd --port 8000 -e /var/www/trac/

```

#### 2.3. Using Authentication

Using Authentication

To create a .passwd file using htdigest:

```
htdigest -c /var/www/trac/.passwd localhost neo

```

then for additional users:

```
htdigest /var/www/trac/.passwd localhost netkiller

```

bind ip

```
tracd -d --host 192.168.3.9 --port 8000 --auth=*,/srv/trac/.passwd,localhost -e /srv/trac

```

```
$ tracd -p 8080 \
   --auth=project1,/path/to/users.htdigest,mycompany.com \
   --auth=project2,/path/to/users.htdigest,mycompany.com \
   /path/to/project1 /path/to/project2

tracd -p 8000 \
   --auth=*,/var/www/trac/.passwd,localhost \
   -e /var/www/trac/

```

#### 2.4. trac-admin

```

# trac-admin /srv/example help
trac-admin - The Trac Administration Console 0.12.3

Usage: trac-admin </path/to/projenv> [command [subcommand] [option ...]]

Invoking trac-admin without command starts interactive mode.

help                 Show documentation
initenv              Create and initialize a new environment
attachment add       Attach a file to a resource
attachment export    Export an attachment from a resource to a file or stdout
attachment list      List attachments of a resource
attachment remove    Remove an attachment from a resource
changeset added      Notify trac about changesets added to a repository
changeset modified   Notify trac about changesets modified in a repository
component add        Add a new component
component chown      Change component ownership
component list       Show available components
component remove     Remove/uninstall a component
component rename     Rename a component
config get           Get the value of the given option in "trac.ini"
config remove        Remove the specified option from "trac.ini"
config set           Set the value for the given option in "trac.ini"
deploy               Extract static resources from Trac and all plugins
hotcopy              Make a hot backup copy of an environment
milestone add        Add milestone
milestone completed  Set milestone complete date
milestone due        Set milestone due date
milestone list       Show milestones
milestone remove     Remove milestone
milestone rename     Rename milestone
permission add       Add a new permission rule
permission list      List permission rules
permission remove    Remove a permission rule
priority add         Add a priority value option
priority change      Change a priority value
priority list        Show possible ticket priorities
priority order       Move a priority value up or down in the list
priority remove      Remove a priority value
repository add       Add a source repository
repository alias     Create an alias for a repository
repository list      List source repositories
repository remove    Remove a source repository
repository resync    Re-synchronize trac with repositories
repository set       Set an attribute of a repository
repository sync      Resume synchronization of repositories
resolution add       Add a resolution value option
resolution change    Change a resolution value
resolution list      Show possible ticket resolutions
resolution order     Move a resolution value up or down in the list
resolution remove    Remove a resolution value
session add          Create a session for the given sid
session delete       Delete the session of the specified sid
session list         List the name and email for the given sids
session purge        Purge all anonymous sessions older than the given age
session set          Set the name or email attribute of the given sid
severity add         Add a severity value option
severity change      Change a severity value
severity list        Show possible ticket severities
severity order       Move a severity value up or down in the list
severity remove      Remove a severity value
ticket remove        Remove ticket
ticket_type add      Add a ticket type
ticket_type change   Change a ticket type
ticket_type list     Show possible ticket types
ticket_type order    Move a ticket type up or down in the list
ticket_type remove   Remove a ticket type
upgrade              Upgrade database to current version
version add          Add version
version list         Show versions
version remove       Remove version
version rename       Rename version
version time         Set version date
wiki dump            Export wiki pages to files named by title
wiki export          Export wiki page to file or stdout
wiki import          Import wiki page from file or stdin
wiki list            List wiki pages
wiki load            Import wiki pages from files
wiki remove          Remove wiki page
wiki rename          Rename wiki page
wiki replace         Replace the content of wiki pages from files (DANGEROUS!)
wiki upgrade         Upgrade default wiki pages to current version

```

##### 2.4.1. Permissions

```
BROWSER_VIEW

CHANGESET_VIEW

CONFIG_VIEW

EMAIL_VIEW

FILE_VIEW

LOG_VIEW

MILESTONE_ADMIN

MILESTONE_CREATE

MILESTONE_DELETE

MILESTONE_MODIFY

MILESTONE_VIEW

PERMISSION_ADMIN

PERMISSION_GRANT

PERMISSION_REVOKE

REPORT_ADMIN

REPORT_CREATE

REPORT_DELETE

REPORT_MODIFY

REPORT_SQL_VIEW

REPORT_VIEW

ROADMAP_ADMIN

ROADMAP_VIEW

SEARCH_VIEW

TICKET_ADMIN

TICKET_APPEND

TICKET_CHGPROP

TICKET_CREATE

TICKET_EDIT_CC

TICKET_EDIT_COMMENT

TICKET_EDIT_DESCRIPTION

TICKET_MODIFY

TICKET_VIEW

TIMELINE_VIEW

TRAC_ADMIN

VERSIONCONTROL_ADMIN

WIKI_ADMIN

WIKI_CREATE

WIKI_DELETE

WIKI_MODIFY

WIKI_RENAME

WIKI_VIEW

```

admin

```
$ trac-admin /path/to/projenv permission add neo TICKET_ADMIN  TRAC_ADMIN  WIKI_ADMIN

```

group

```
$ trac-admin /path/to/projenv permission add admin MILESTONE_ADMIN PERMISSION_ADMIN REPORT_ADMIN ROADMAP_ADMIN TICKET_ADMIN TRAC_ADMIN VERSIONCONTROL_ADMIN WIKI_ADMIN

$ trac-admin /path/to/projenv permission add developer WIKI_ADMIN
$ trac-admin /path/to/projenv permission add developer REPORT_ADMIN
$ trac-admin /path/to/projenv permission add developer TICKET_ADMIN

```

user

```
$ trac-admin /path/to/projenv permission add bob developer
$ trac-admin /path/to/projenv permission add john developer

```

##### 2.4.2. Resync

```
# trac-admin /srv/example repository resync '(default)'

```

旧版本 trac: trac-admin /srv/trac/neo resync

### 3. Project Environment

#### 3.1. Sqlite

1.  Creating a Project Environment

    ```

    $ trac-admin /home/netkiller/projectenv initenv

    Creating a new Trac environment at /home/netkiller/projectenv

    Trac will first ask a few questions about your environment
    in order to initalize and prepare the project database.

     Please enter the name of your project.
     This name will be used in page titles and descriptions.

    Project Name [My Project]>

     Please specify the connection string for the database to use.
     By default, a local SQLite database is created in the environment
     directory. It is also possible to use an already existing
     PostgreSQL database (check the Trac documentation for the exact
     connection string syntax).

    Database connection string [sqlite:db/trac.db]>

     Please specify the type of version control system,
     By default, it will be svn.

     If you don't want to use Trac with version control integration,
     choose the default here and don't specify a repository directory.
     in the next question.

    Repository type [svn]>

     Please specify the absolute path to the version control
     repository, or leave it blank to use Trac without a repository.
     You can also set the repository location later.

    Path to repository [/path/to/repos]> /home/netkiller/repos

     Please enter location of Trac page templates.
     Default is the location of the site-wide templates installed with Trac.

    Templates directory [/usr/share/trac/templates]>

    Creating and Initializing Project
     Installing default wiki pages
     /usr/share/trac/wiki-default/TracIni => TracIni
     /usr/share/trac/wiki-default/TracSupport => TracSupport
     /usr/share/trac/wiki-default/WikiStart => WikiStart
     /usr/share/trac/wiki-default/TitleIndex => TitleIndex
     /usr/share/trac/wiki-default/TracModPython => TracModPython
     /usr/share/trac/wiki-default/TracInterfaceCustomization => TracInterfaceCustomization
     /usr/share/trac/wiki-default/WikiDeletePage => WikiDeletePage
     /usr/share/trac/wiki-default/TracTicketsCustomFields => TracTicketsCustomFields
     /usr/share/trac/wiki-default/TracChangeset => TracChangeset
     /usr/share/trac/wiki-default/TracLogging => TracLogging
     /usr/share/trac/wiki-default/TracSyntaxColoring => TracSyntaxColoring
     /usr/share/trac/wiki-default/TracImport => TracImport
     /usr/share/trac/wiki-default/TracTimeline => TracTimeline
     /usr/share/trac/wiki-default/TracAdmin => TracAdmin
     /usr/share/trac/wiki-default/InterWiki => InterWiki
     /usr/share/trac/wiki-default/WikiPageNames => WikiPageNames
     /usr/share/trac/wiki-default/TracNotification => TracNotification
     /usr/share/trac/wiki-default/TracFastCgi => TracFastCgi
     /usr/share/trac/wiki-default/InterTrac => InterTrac
     /usr/share/trac/wiki-default/TracUnicode => TracUnicode
     /usr/share/trac/wiki-default/TracGuide => TracGuide
     /usr/share/trac/wiki-default/TracRevisionLog => TracRevisionLog
     /usr/share/trac/wiki-default/TracBrowser => TracBrowser
     /usr/share/trac/wiki-default/WikiRestructuredText => WikiRestructuredText
     /usr/share/trac/wiki-default/TracLinks => TracLinks
     /usr/share/trac/wiki-default/TracInstall => TracInstall
     /usr/share/trac/wiki-default/TracPermissions => TracPermissions
     /usr/share/trac/wiki-default/WikiMacros => WikiMacros
     /usr/share/trac/wiki-default/TracQuery => TracQuery
     /usr/share/trac/wiki-default/TracBackup => TracBackup
     /usr/share/trac/wiki-default/TracWiki => TracWiki
     /usr/share/trac/wiki-default/SandBox => SandBox
     /usr/share/trac/wiki-default/TracRoadmap => TracRoadmap
     /usr/share/trac/wiki-default/TracAccessibility => TracAccessibility
     /usr/share/trac/wiki-default/TracSearch => TracSearch
     /usr/share/trac/wiki-default/TracPlugins => TracPlugins
     /usr/share/trac/wiki-default/RecentChanges => RecentChanges
     /usr/share/trac/wiki-default/WikiNewPage => WikiNewPage
     /usr/share/trac/wiki-default/TracCgi => TracCgi
     /usr/share/trac/wiki-default/TracRss => TracRss
     /usr/share/trac/wiki-default/CamelCase => CamelCase
     /usr/share/trac/wiki-default/WikiFormatting => WikiFormatting
     /usr/share/trac/wiki-default/TracTickets => TracTickets
     /usr/share/trac/wiki-default/TracStandalone => TracStandalone
     /usr/share/trac/wiki-default/InterMapTxt => InterMapTxt
     /usr/share/trac/wiki-default/TracReports => TracReports
     /usr/share/trac/wiki-default/WikiHtml => WikiHtml
     /usr/share/trac/wiki-default/WikiProcessors => WikiProcessors
     /usr/share/trac/wiki-default/TracUpgrade => TracUpgrade
     /usr/share/trac/wiki-default/TracEnvironment => TracEnvironment
     /usr/share/trac/wiki-default/WikiRestructuredTextLinks => WikiRestructuredTextLinks

    Warning:

    You should install the SVN bindings

    ---------------------------------------------------------------------
    Project environment for 'My Project' created.

    You may now configure the environment by editing the file:

      /home/netkiller/projectenv/conf/trac.ini

    If you'd like to take this new project environment for a test drive,
    try running the Trac standalone web server `tracd`:

      tracd --port 8000 /home/netkiller/projectenv

    Then point your browser to http://localhost:8000/projectenv.
    There you can also browse the documentation for your installed
    version of Trac, including information on further setup (such as
    deploying Trac to a real web server).

    The latest documentation can also always be found on the project
    website:

      http://trac.edgewall.org/

    Congratulations!

    ```

2.  Running the Standalone Server

    ```
    tracd --port 8000 /home/netkiller/projectenv

    ```

3.  testing

    http://192.168.1.7:8000/projectenv/

4.  auth

    ```
    sudo apt-get install apache2-utils

    $ htdigest -c /home/neo/trac/conf/passwd.digest localhost neo
    Adding password for neo in realm localhost.
    New password:
    Re-type new password:

    $ htdigest /home/neo/trac/conf/passwd.digest localhost nchen
    Adding user nchen in realm localhost
    New password:
    Re-type new password:

    $ trac-admin /home/neo/trac permission add admin TRAC_ADMIN
    $ trac-admin /home/neo/trac permission add netkiller admin

    $ trac-admin /home/neo/trac permission add developer TICKET_ADMIN
    $ trac-admin /home/neo/trac permission add nchen developer
    $ trac-admin /home/neo/trac permission add neo developer

    $ trac-admin /home/neo/trac permission list
    User           Action
    ------------------------------
    admin          TRAC_ADMIN
    anonymous      BROWSER_VIEW
    anonymous      CHANGESET_VIEW
    anonymous      FILE_VIEW
    anonymous      LOG_VIEW
    anonymous      MILESTONE_VIEW
    anonymous      REPORT_SQL_VIEW
    anonymous      REPORT_VIEW
    anonymous      ROADMAP_VIEW
    anonymous      SEARCH_VIEW
    anonymous      TICKET_VIEW
    anonymous      TIMELINE_VIEW
    anonymous      WIKI_VIEW
    authenticated  TICKET_CREATE
    authenticated  TICKET_MODIFY
    authenticated  WIKI_CREATE
    authenticated  WIKI_MODIFY
    developer      TICKET_ADMIN
    nchen          developer
    neo            developer
    netkiller      admin

    ```

5.  daemon

    ```
    $ tracd -d -s --port 8000 /home/netkiller/projectenv
    $ tracd -d -s --port 8000 --auth trac,/home/neo/trac/conf/passwd.digest,localhost /home/neo/trac

    ```

#### 3.2. MySQL

```
GRANT ALL PRIVILEGES ON trac.* TO trac@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
CREATE DATABASE IF NOT EXISTS trac default charset utf8 COLLATE utf8_general_ci;

```

```
Database connection string [sqlite:db/trac.db]> mysql://trac:password@localhost:3306/trac

```

下面开始创建项目

```

# trac-admin /home/git/trac initenv
Creating a new Trac environment at /home/git/trac

Trac will first ask a few questions about your environment
in order to initialize and prepare the project database.

 Please enter the name of your project.
 This name will be used in page titles and descriptions.

Project Name [My Project]>

 Please specify the connection string for the database to use.
 By default, a local SQLite database is created in the environment
 directory. It is also possible to use an already existing
 PostgreSQL database (check the Trac documentation for the exact
 connection string syntax).

Database connection string [sqlite:db/trac.db]> mysql://trac:trac@localhost:3306/trac

Creating and Initializing Project
 Installing default wiki pages
  TracRepositoryAdmin imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracRepositoryAdmin
  TracNavigation imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracNavigation
  TracUpgrade imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracUpgrade
  TracRevisionLog imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracRevisionLog
  TracTickets imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracTickets
  TracIni imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracIni
  PageTemplates imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/PageTemplates
  TracTimeline imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracTimeline
  TracAccessibility imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracAccessibility
  WikiHtml imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/WikiHtml
  SandBox imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/SandBox
  TracImport imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracImport
  TracPlugins imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracPlugins
  TracRoadmap imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracRoadmap
  TracAdmin imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracAdmin
  TracBatchModify imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracBatchModify
  TracBrowser imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracBrowser
  InterWiki imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/InterWiki
  WikiRestructuredText imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/WikiRestructuredText
  WikiProcessors imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/WikiProcessors
  WikiNewPage imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/WikiNewPage
  TracEnvironment imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracEnvironment
  TracLogging imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracLogging
  TracSupport imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracSupport
  TracNotification imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracNotification
  TracGuide imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracGuide
  WikiStart imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/WikiStart
  TracWorkflow imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracWorkflow
  TracRss imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracRss
  TracLinks imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracLinks
  InterMapTxt imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/InterMapTxt
  WikiPageNames imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/WikiPageNames
  WikiFormatting imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/WikiFormatting
  WikiRestructuredTextLinks imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/WikiRestructuredTextLinks
  TracUnicode imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracUnicode
  TracChangeset imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracChangeset
  TitleIndex imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TitleIndex
  WikiDeletePage imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/WikiDeletePage
  TracReports imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracReports
  TracWiki imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracWiki
  RecentChanges imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/RecentChanges
  TracBackup imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracBackup
  TracModPython imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracModPython
  TracSearch imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracSearch
  TracModWSGI imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracModWSGI
  TracTicketsCustomFields imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracTicketsCustomFields
  TracQuery imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracQuery
  TracStandalone imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracStandalone
  InterTrac imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/InterTrac
  TracFineGrainedPermissions imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracFineGrainedPermissions
  TracInterfaceCustomization imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracInterfaceCustomization
  TracCgi imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracCgi
  TracFastCgi imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracFastCgi
  TracPermissions imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracPermissions
  TracInstall imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracInstall
  TracSyntaxColoring imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/TracSyntaxColoring
  CamelCase imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/CamelCase
  WikiMacros imported from /root/.python-eggs/Trac-1.1.1-py2.6.egg-tmp/trac/wiki/default-pages/WikiMacros

---------------------------------------------------------------------
Project environment for 'My Project' created.

You may now configure the environment by editing the file:

  /home/git/trac/conf/trac.ini

If you'd like to take this new project environment for a test drive,
try running the Trac standalone web server `tracd`:

  tracd --port 8000 /home/git/trac

Then point your browser to http://localhost:8000/trac.
There you can also browse the documentation for your installed
version of Trac, including information on further setup (such as
deploying Trac to a real web server).

The latest documentation can also always be found on the project
website:

  http://trac.edgewall.org/

Congratulations!

```

#### 3.3. Plugin

##### 3.3.1. AccountManagerPlugin

http://trac-hacks.org/wiki/AccountManagerPlugin

```
cd accountmanagerplugin/
python setup.py install
python setup.py bdist_egg

cp dist/TracAccountManager-0.4.4-py2.6.egg /home/git/trac/plugins/

```

##### 3.3.2. Subtickets

http://trac-hacks.org/wiki/SubticketsPlugin

### 4. trac.ini

#### 4.1. repository

```
[trac]
repository_dir = /opt/svnroot/neo
repository_sync_per_request = (default)
repository_type = svn

```

svn 仓库地址 repository_dir

#### 4.2. attachment 附件配置

上传附件尺寸控制

```
[attachment]
max_size=262144

```

### 5. trac-admin

权限组定制

```
trac-admin /opt/trac permission add admin TRAC_ADMIN
trac-admin /opt/trac permission add Development TICKET_ADMIN
trac-admin /opt/trac permission add Operations TICKET_ADMIN

```

权限初始化

```
trac-admin /opt/trac permission add mgmt admin
trac-admin /opt/trac permission add neo Development
trac-admin /opt/trac permission add ken Operations

trac-admin /opt/trac permission list

```

#### 5.1. adduser script

```

#!/bin/bash

user=$1
trac-admin /opt/trac permission add $user Operations
htdigest -c /opt/trac/conf/passwd.digest localhost $user

```

### 6. FAQ

#### 6.1. TracError: Cannot load Python bindings for MySQL

检查 MySQLdb 是否安装

```

# /usr/bin/python -c 'import MySQLdb'
Traceback (most recent call last):
  File "<string>", line 1, in <module>
ImportError: No module named MySQLdb

```

安装 MySQLdb

```
# yum install python-devel
# pip install MySQL-python

```

或者

```
# yum install python-devel
# easy_install MySQL-python

```

再次测试，如果不出现任何提示表示成功。

```
# /usr/bin/python -c 'import MySQLdb'

```

### 7. Apache Bloodhound

Apache Bloodhound 是基于 Trac 的项目管理软件

## 第 171 章 Gitlab 项目管理

实施 DEVOPS 首先我们要有一个项目管理工具。

我建议使用 Gitlab，早年我倾向使用 Trac，但 Trac 项目一直处于半死不活状态，目前来看 Trac 对于 Ticket 管理强于 Gitlab，但 Gitlab 发展的很快，我们可以看到最近的一次升级中 Issue 加入了 Due date 选项。Gitlab 已经有风投介入，企业化运作，良性发展，未来会超越 Redmine 等项目管理软件，成为主流。所以我在工具篇采用 Gitlab，BTW 我没有使用 Redmine，我认为 Redmine 的发展方向更接近传统项目管理思维。

软件项目管管理，我需要那些功能，Ticket/Issue 管理、里程碑管理、内容管理 Wiki、版本管理、合并分支、代码审查等等

关于 Gitlib 的安装配置请参考 [`www.netkiller.cn/project/project/gitlab/index.html`](http://www.netkiller.cn/project/project/gitlab/index.html)

### 1. GitLab

[`github.com/gitlabhq`](https://github.com/gitlabhq)

GitLab 是一个利用 Ruby on Rails 开发的开源应用程序，实现一个自托管的 Git 项目仓库，可通过 Web 界面进行访问公开的或者私人项目。

它拥有与 Github 类似的功能，能够浏览源代码，管理缺陷和注释。可以管理团队对仓库的访问，它非常易于浏览提交过的版本并提供一个文件历史库。团队成员可以利用内置的简单聊天程序(Wall)进行交流。它还提供一个代码片段收集功能可以轻松实现代码复用，便于日后有需要的时候进行查找。

GitLab 5.0 以前版本要求服务器端采用 Gitolite 搭建，5.0 版本以后不再使用 Gitolite ，采用自己开发的 gitlab-shell 来实现。如果你觉得安装麻烦可以使用 GitLab Installers 一键安装程序。

#### 1.1. Yum 安装 GitLab

```
yum localinstall -y https://downloads-packages.s3.amazonaws.com/centos-6.6/gitlab-ce-7.10.0~omnibus.2-1.x86_64.rpm

gitlab-ctl reconfigure

cp /etc/gitlab/gitlab.rb{,.original}

```

停止 GitLab 服务

```
# gitlab-ctl stop
ok: down: logrotate: 1s, normally up
ok: down: nginx: 0s, normally up
ok: down: postgresql: 0s, normally up
ok: down: redis: 0s, normally up
ok: down: sidekiq: 1s, normally up
ok: down: unicorn: 0s, normally up

```

启动 GitLab 服务

```
# gitlab-ctl start
ok: run: logrotate: (pid 3908) 0s
ok: run: nginx: (pid 3911) 1s
ok: run: postgresql: (pid 3921) 0s
ok: run: redis: (pid 3929) 1s
ok: run: sidekiq: (pid 3933) 0s
ok: run: unicorn: (pid 3936) 1s

```

配置 gitlab

```
# vim /etc/gitlab/gitlab.rb
external_url 'http://gitlab.example.com'		

```

SMTP 配置

```
gitlab_rails['gitlab_email_enabled'] = true
gitlab_rails['gitlab_email_from'] = 'openunix@163.com'
gitlab_rails['gitlab_email_display_name'] = 'Neo'
gitlab_rails['gitlab_email_reply_to'] = 'noreply@example.com'

gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtp.163.com"
gitlab_rails['smtp_user_name'] = "openunix@163.com"
gitlab_rails['smtp_password'] = "password"
gitlab_rails['smtp_domain'] = "163.com"
gitlab_rails['smtp_authentication'] = "login"		

```

任何配置文件变化都需要运行 # gitlab-ctl reconfigure

WEB 管理员

```
# Username: root 
# Password: 5iveL!fe		

```

#### 1.2. Docker 方式安装 Gitlab

```

docker pull gitlab/gitlab-ce:latest		

```

```

docker run \
    -d \
    -p 443:443 \
    -p 80:80 \
    --name gitlab-ce \
    --restart unless-stopped \
    -v /opt/gitlab/etc:/etc/gitlab \
    -v /opt/gitlab/log:/var/log/gitlab \
    -v /opt/gitlab/data:/var/opt/gitlab \
    gitlab/gitlab-ce		

```

配置对外 url，域名或者 ip，公网能访问即可

```

vim /mnt/gitlab/etc/gitlab.rb
添加一下配置：
external_url	'http://127.0.0.1' （你的域名或者 ip 地址）	

```

配置邮箱

```

vim /mnt/gitlab/etc/gitlab.rb
gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtp.qq.com"
gitlab_rails['smtp_port'] = 465
gitlab_rails['smtp_user_name'] = "13721218@qq.com"    (替换成自己的 QQ 邮箱)
gitlab_rails['smtp_password'] = "xxxxx"
gitlab_rails['smtp_domain'] = "smtp.qq.com"
gitlab_rails['smtp_authentication'] = "login"
gitlab_rails['smtp_enable_starttls_auto'] = true
gitlab_rails['smtp_tls'] = true
gitlab_rails['gitlab_email_from'] = '13721218@qq.com'  (替换成自己的 QQ 邮箱，且与 smtp_user_name 一致)		

```

重新启动 gitlab

```

docker restart gitlab-ce	

```

#### 1.3. GitLab Runner

```
curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-ci-multi-runner/script.rpm.sh | sudo bash
sudo yum install gitlab-ci-multi-runner

```

进入 CI 配置页面 http://git.netkiller.cn/netkiller.cn/www.netkiller.cn/settings/ci_cd

Specific Runners 你将看到 CI 的 URL 和他的 Token

Specify the following URL during the Runner setup: http://git.netkiller.cn/ci

Use the following registration token during setup: wRoz1Y_6CXpNh2JbxN_s

现在回到 GitLab Runner

```
# gitlab-ci-multi-runner register
Running in system-mode.                            

Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
http://git.netkiller.cn/ci
Please enter the gitlab-ci token for this runner:
wRoz1Y_6CXpNh2JbxN_s
Please enter the gitlab-ci description for this runner:
[iZ62yln3rjjZ]: gitlab-ci-1
Please enter the gitlab-ci tags for this runner (comma separated):
test
Whether to run untagged builds [true/false]:
[false]: 
Registering runner... succeeded                     runner=wRoz1Y_6
Please enter the executor: docker, docker-ssh, shell, ssh, virtualbox, docker+machine, docker-ssh+machine, kubernetes, parallels:
shell
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded! 

```

回到 Gitlab 页你将看到 Pending 状态变成 Running 状态

升级 GitLab Runner

```
yum install gitlab-ci-multi-runner		

```

#### 1.4. 用户管理

初始化 GitLab，进入 Admin area，单击左侧菜单 Users，在这里为 gitlab 添加用户

#### 1.5. 组管理

初始化 GitLab 组，我比较喜欢使用“域名”作为组名，例如 example.com

#### 1.6. 项目管理

创建项目，我通常会在组下面创建项目，每个域名对应一个项目,例如 www.example.com,images.example.com

版本库 URL 如下

```
http: http://192.168.0.1/example.com/www.example.com.git
ssh: git@192.168.0.1:example.com/www.example.com.git

```

#### 1.7. 绑定 SSL 证书

编辑 /etc/gitlab/gitlab.rb 文件

```

external_url 'https://git.netkiller.cn'

nginx['enable'] = true
nginx['redirect_http_to_https'] = true
nginx['ssl_certificate'] = "/etc/gitlab/ssl/git.netkiller.cn.crt"
nginx['ssl_certificate_key'] = "/etc/gitlab/ssl/git.netkiller.cn.key"
nginx['listen_https'] = true
nginx['http2_enabled'] = true

```

#### 1.8. FAQ

##### 1.8.1. gitolite 向 gitlab 迁移

早期 gitlab 使用 gitolite 为用户提供 SSH 服务，新版 gitlab 有了更好的解决方案 gitlab-shell。安装新版本是必会涉及 gitolite 向 gitlab 迁移，下面是我总结的一些迁移经验。

第一步,将 gitolite 复制到 gitlab 仓库目录下

```
# cp -r /gitroot/gitolite/repositories/* /var/opt/gitlab/git-data/repositories/

```

执行导入处理程序

```
# gitlab-rake gitlab:import:repos

```

上面程序会处理一下目录结构，例如

进入 gitlab web 界面，创建仓库与导入的仓库同名，这样就完成了导入工作。

### 提示

转换最好在 git 用户下面操作，否则你需要运行

```
# chown git:git -R /var/opt/gitlab/git-data/repositories				

```

##### 1.8.2. 修改主机名

默认 Gitlab 采用主机名，给我使用代理一定麻烦

```
git@hostname:example.com/www.example.com.git
http://hostname/example.com/www.example.com.git

```

我们希望使用 IP 地址替代主机名

```
git@172.16.0.1:example.com/www.example.com.git
http://172.16.0.1/example.com/www.example.com.git

```

编辑 /etc/gitlab/gitlab.rb 配置文件

```
external_url 'http://172.16.0.1'

```

重新启动 Gitlab

```
# gitlab-ctl reconfigure
# gitlab-ctl restart

```

### 2. 创建用户

过程 171.1. 企业内部使用的 Gitlab 初始化

1.  关闭在线用户注册

2.  Step 3.

    1.  Substep a.

    2.  Substep b.

### 3. 创建组与项目

过程 171.2. Gitlab 初始化 - 创建组

1.  点击 New Group 按钮新建一个组，我习惯每个域一个组，所以我使用 netkiller.cn 作为组名称

    | ![](img/group.png) |

2.  输入 netkiller.cn 然后单击 Create group

    | ![](img/group.new.png) |

3.  组创建完毕

    | ![](img/groups.png) |

创建组后接下来创建项目

过程 171.3. Gitlab 初始化 - 创建项目

1.  单击 New Project 创建项目

    | ![](img/projects.png) |

2.  输入 www.netkiller.cn 并点击 Create project 按钮创建项目

    | ![](img/projects.new.png) |

3.  项目创建完毕

    | ![](img/projects.created.png) |

### 4. 分支管理

起初我们应对并行开发在 Subversion 上创建分支，每个任务一个分支，每个 Bug 一个分支，完成任务或修复 bug 后合并到开发分支(development)内部测试，然后再进入测试分支(testing)提交给测试组，测试组完成测试，最后进入主干(trunk)。对于 Subverion 来说每一个分支都是一份拷贝，SVN 版本库膨胀的非常快。

Git 解决了 Svn 先天不足的分支管理功能，分支在 GIT 类似快照，同时 GIT 还提供了 pull request 功能。

我们怎样使用 git 的分支功能呢？ 首先我们不再为每个任务创建一个分支，将任务分支放在用户自己的仓库下面，通过 pull request 合并，同时合并过程顺便 code review。

master：是主干，只有开发部主管/经理级别拥有权限，只能合并来自 testing 的代码

testing: 测试分支，测试部拥有权限，此分支不能修改，只能从开发分支合并代码。

development：开发组的分支，Team 拥有修改权限，可以合并，可以接受 pull request, 可以提交代码

tag 是 Release 本版，开发部主管/经理拥有权限

分支的权限管理：

master: 保护

testing：保护

development：保护

过程 171.4. Gitlab 分支应用 - 创建分支

1.  首先，点击左侧 Commits 按钮，然后点击 Branches 按钮进入分支管理

    | ![](img/branches.png) |

2.  点击 New branch 创建分支

    | ![](img/branches.new.png) |

    在 Branch name 中输入分支名称，然后点击 Create branch 创建分支

3.  分支已经创建

    | ![](img/branches.created.png) |

重复上面步骤，完成 development 分支的创建。

保护分支：锁定分支，只允允许合并，不允许提交

过程 171.5. 保护分支

1.  master

    testing

2.  Step 2.

    2.  Substep b.

### 5. Issue

Issues 任务

#### 5.1. Milestones 里程碑

敏捷开发中可以每周一个里程碑，或者每个月一个里程碑。

#### 5.2. Labels 标签

通常定义四个状态，开发，测试，升级，完成

### 6. 代码审查

### 7. 合并

### 8. WebHook

### 9. CI / CD

https://gitlab.com/gitlab-examples

#### 9.1. GitLab Runner

##### 9.1.1. Install GitLab Runner

```

[root@localhost ~]# wget -O /usr/local/bin/gitlab-runner https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
--2019-06-06 16:19:31--  https://gitlab-runner-downloads.s3.amazonaws.com/latest/binaries/gitlab-runner-linux-amd64
Resolving gitlab-runner-downloads.s3.amazonaws.com (gitlab-runner-downloads.s3.amazonaws.com)... 52.216.10.19
Connecting to gitlab-runner-downloads.s3.amazonaws.com (gitlab-runner-downloads.s3.amazonaws.com)|52.216.10.19|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 30832000 (29M) [application/octet-stream]
Saving to: '/usr/local/bin/gitlab-runner'

100%[==================================================================================================================================================================================================>] 30,832,000   113KB/s   in 7m 11s 

2019-06-06 16:26:44 (69.8 KB/s) - '/usr/local/bin/gitlab-runner' saved [30832000/30832000]

[root@localhost ~]# chmod +x /usr/local/bin/gitlab-runner
[root@localhost ~]# useradd --comment 'GitLab Runner' --create-home gitlab-runner --shell /bin/bash
[root@localhost ~]# gitlab-runner install --user=gitlab-runner --working-directory=/home/gitlab-runner
Runtime platform                                    arch=amd64 os=linux pid=113531 revision=ac2a293c version=11.11.2
[root@localhost ~]# gitlab-runner start
Runtime platform                                    arch=amd64 os=linux pid=113576 revision=ac2a293c version=11.11.2				

```

##### 9.1.2. Registering Runners

```

[root@localhost ~]# gitlab-runner register
Runtime platform                                    arch=amd64 os=linux pid=114182 revision=ac2a293c version=11.11.2
Running in system-mode.                            

Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
http://192.168.1.229/
Please enter the gitlab-ci token for this runner:
5iF88xJLfFgpvRJySam2
Please enter the gitlab-ci description for this runner:
[localhost.localdomain]: 
Please enter the gitlab-ci tags for this runner (comma separated):
my-tag,another-tag
Registering runner... succeeded                     runner=5iF88xJL
Please enter the executor: docker, docker-ssh, parallels, ssh, virtualbox, kubernetes, shell, docker+machine, docker-ssh+machine:
shell
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded! 				

```

##### 9.1.3. /etc/gitlab-runner/config.toml

```

[root@localhost ~]# cat /etc/gitlab-runner/config.toml
concurrent = 1
check_interval = 0

[session_server]
  session_timeout = 1800

[[runners]]
  name = "localhost.localdomain"
  url = "http://192.168.1.229/"
  token = "09936a09484934dec93d78ffa49b89"
  executor = "shell"
  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]				

```

#### 9.2. 配置 CI / CD

进入项目设置界面，点击 Settings，再点击 CI / CD

| ![](img/CI-CD.png) |

点击 Expand 按钮 展开 Runners

| ![](img/Runners.png) |

这时可以看到 Set up a specific Runner manually, 后面会用到 http://192.168.1.96/ 和 zASzWwffenos6Jbbfsgu

使用 SSH 登录 Gitlab runner 服务器，运行 gitlab-runner register

```

[root@localhost ~]# gitlab-runner register
Runtime platform                                    arch=amd64 os=linux pid=92925 revision=ac2a293c version=11.11.2
Running in system-mode.                            

Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
http://192.168.1.96/
Please enter the gitlab-ci token for this runner:
zASzWwffenos6Jbbfsgu
Please enter the gitlab-ci description for this runner:
[localhost.localdomain]: 
Please enter the gitlab-ci tags for this runner (comma separated):

Registering runner... succeeded                     runner=zASzWwff
Please enter the executor: docker, docker-ssh, shell, ssh, docker-ssh+machine, parallels, virtualbox, docker+machine, kubernetes:
shell
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded! 			

```

返回 gitlab 查看注册状态

| ![](img/Runners-status.png) |

#### 9.3. Pipeline

##### 9.3.1. cache

```

image: maven:3.5.0-jdk-8

variables:
  MAVEN_OPTS: "-Dmaven.repo.local=.m2/repository"

cache:
  paths:
    - .m2/repository/
    - target/

stages:
  - build
  - test
  - package

build:
  stage: build
  script: mvn compile

unittest:
  stage: test
  script: mvn test

package:
  stage: package
  script: mvn package
  artifacts:
    paths:
      - target/java-project-0.0.1-SNAPSHOT.jar

```

##### 9.3.2. before_script

##### 9.3.3. stages

```

image: mileschou/php-testing-base:7.0

stages:
  - build
  - test
  - deploy

build_job:
  stage: build
  script:
    - composer install
  cache:
    untracked: true
  artifacts:
    paths:
      - vendor/

test_job:
  stage: test
  script:
    - php vendor/bin/codecept run
  dependencies:
    - build_job

deploy_job:
  stage: deploy
  script:
    - echo Deploy OK
  only:
    - release
  when: manual

```

```

  only: 
    - master
  tags:
    - ansible

```

##### 9.3.4. services

```

services:
- mysql

variables:
  # Configure mysql service (https://hub.docker.com/_/mysql/)
  MYSQL_DATABASE: hello_world_test
  MYSQL_ROOT_PASSWORD: mysql

connect:
  image: mysql
  script:
  - echo "SELECT 'OK';" | mysql --user=root --password="$MYSQL_ROOT_PASSWORD" --host=mysql "$MYSQL_DATABASE"

```

#### 9.4. Java

```

#image: java:8
#image: maven:latest
image: maven:3.5.0-jdk-8

stages:
  - build
  - test
  - package

build:
  stage: build
  script: mvn compile

unittest:
  stage: test
  script: mvn test

package:
  stage: package
  script: mvn package
  artifacts:
    paths:
      - target/java-project-0.0.1-SNAPSHOT.jar

```

```

before_script:
 - echo "Execute scripts which are required to bootstrap the application. !"

after_script:
 - echo "Clean up activity can be done here !."

stages:
 - build
 - test
 - package
 - deploy

variables:
 MAVEN_CLI_OPTS: "--batch-mode"
 MAVEN_OPTS: "-Dmaven.repo.local=.m2/repository"

cache:
 paths:
  - .m2/repository/
  - target/

build:
 stage: build
 image: maven:latest
 script:
  - mvn $MAVEN_CLI_OPTS clean compile

test:
 stage: test
 image: maven:latest
 script:
  - mvn $MAVEN_CLI_OPTS test

package:
 stage: package
 image: maven:latest
 script:
  - mvn $MAVEN_CLI_OPTS package
 artifacts:
  paths: [target/test-0.0.1.war]

deploy_test:
 stage: deploy
 script:
  - echo "########   To be defined   ########"
 environment: staging

deploy_prod:
 stage: deploy
 script:
  - echo "########   To be defined   ########"
 only:
  - master
 environment: production			

```

#### 9.5. vue.js android

```

build site:
  image: node:6
  stage: build
  script:
    - npm install --progress=false
    - npm run build
  artifacts:
    expire_in: 1 week
    paths:
      - dist

unit test:
  image: node:6
  stage: test
  script:
    - npm install --progress=false
    - npm run unit

deploy:
  image: alpine
  stage: deploy
  script:
    - apk add --no-cache rsync openssh
    - mkdir -p ~/.ssh
    - echo "$SSH_PRIVATE_KEY" >> ~/.ssh/id_dsa
    - chmod 600 ~/.ssh/id_dsa
    - echo -e "Host *\n\tStrictHostKeyChecking no\n\n" > ~/.ssh/config
    - rsync -rav --delete dist/ user@server.com:/your/project/path/			

```

## 第 172 章 Redmine

http://www.redmine.org/

[redmine 一键安装包](http://bitnami.org/stack/redmine)

### 1. CentOS 安装

安装 MySQL 数据库

```
curl -s https://raw.githubusercontent.com/oscm/shell/master/database/mysql/mysql.server.sh | bash
curl -s https://raw.githubusercontent.com/oscm/shell/master/database/mysql/mysql.devel.sh | bash		

```

创建数据库账号

```
CREATE DATABASE redmine CHARACTER SET utf8;
GRANT ALL PRIVILEGES ON redmine.* TO 'redmine'@'localhost' IDENTIFIED BY 'my_password';		

```

安装

```

yum install -y ruby rubygems ruby-devel ImageMagick-devel

cd /usr/local/src/
wget http://www.redmine.org/releases/redmine-3.3.0.tar.gz
tar zxf redmine-3.3.0.tar.gz
mv redmine-3.3.0 /srv/
ln -s /srv/redmine-3.3.0 /srv/redmine
cd /srv/redmine

cat >> config/database.yml <<EOF
production:
  adapter: mysql2
  database: redmine
  host: localhost
  username: redmine
  password: my_password
  encoding: utf8
EOF

gem install bundler
bundle install --without development test
bundle exec rake generate_secret_token

RAILS_ENV=production bundle exec rake db:migrate
#bundle exec rake redmine:load_default_data
RAILS_ENV=production REDMINE_LANG=zh bundle exec rake redmine:load_default_data

mkdir -p tmp tmp/pdf public/plugin_assets
sudo chown -R redmine:redmine files log tmp public/plugin_assets
sudo chmod -R 755 files log tmp public/plugin_assets

bundle exec rails server webrick -e production

```

默认用户名与密码 login: admin，password: admin

### 2. Redmine 运行

```
# rails server -h
Usage: rails server [mongrel, thin etc] [options]
    -p, --port=port                  Runs Rails on the specified port.
                                     Default: 3000
    -b, --binding=IP                 Binds Rails to the specified IP.
                                     Default: localhost
    -c, --config=file                Uses a custom rackup configuration.
    -d, --daemon                     Runs server as a Daemon.
    -u, --debugger                   Enables the debugger.
    -e, --environment=name           Specifies the environment to run this server under (test/development/production).
                                     Default: development
    -P, --pid=pid                    Specifies the PID file.
                                     Default: tmp/pids/server.pid

    -h, --help                       Shows this help message.

```

绑定监听地址 -b

```
# bundle exec rails server webrick -e production -b 0.0.0.0

```

守护进程 -d

### 3. 插件

#### 3.1. workflow

http://www.redmine.org/plugins/redmine_workflow_enhancements

## 第 173 章 TUTOS

TUTOS is a tool to manage the organizational needs of small groups, teams, departments ...

[`www.tutos.org/`](http://www.tutos.org/)

过程 173.1. TUTOS

1.  extract

    ```
    tar jxvf TUTOS-php-1.3.20070317.tar.bz2
    sudo mv tutos /www/htdocs/

    ```

2.  database

    ```
    netkiller@shenzhen:/www/htdocs/tutos$ mysqladmin -uroot -p create tutos
    netkiller@shenzhen:/www/htdocs/tutos$ mysql -uroot -p
    Enter password:
    Welcome to the MySQL monitor.  Commands end with ; or \g.
    Your MySQL connection id is 846
    Server version: 5.0.45 Source distribution

    Type 'help;' or '\h' for help. Type '\c' to clear the buffer.

    mysql> grant all on tutos.* to tutos@% identified by "chen";
    Query OK, 0 rows affected (0.05 sec)

    mysql> grant all on tutos.* to tutos@localhost identified by "chen";
    Query OK, 0 rows affected (0.00 sec)

    mysql> FLUSH PRIVILEGES;
    Query OK, 0 rows affected (0.00 sec)

    mysql> quit
    Bye

    netkiller@shenzhen:/www/htdocs/tutos$ mysqladmin -uroot -p reload

    ```

3.  config

    ```
    mkdir /www/htdocs/tutos/repository

    ```

    http://192.168.1.7/tutos/php/admin/scheme.php

    or

    ```
    cp config_default.pinc  config.php

    ```

    <?php
    # remove this line when finsihed with config
    $tutos['CCSID'] = "10880f50567242006bf2c1a2c0b8b350";
    #
    # sessionpath
    #
    $tutos[sessionpath] = "/tmp";
    #
    # the next lines are a database definition
    #
    $tutos[dbname][0]     = "tutos";
    $tutos[dbhost][0]     = "localhost";
    $tutos[dbport][0]     = "5432";
    $tutos[dbuser][0]     = "tutos";
    $tutos[dbpasswd][0]   = "chen";
    $tutos[dbtype][0]     = "2";
    $tutos[dbalias][0]    = "Mysql database";
    $tutos[cryptpw][0]    = "";
    $tutos[repository][0] = "repository";
    $tutos[dbprefix][0]   = "";
    #
    # MAIL
    #
    $tutos[mailmode] = "2";
    $tutos[sendmail] = "/usr/lib/sendmail";
    $tutos[smtphost] = "localhost";
    #
    # demo mode
    #
    $tutos[demo] = 0;
    #
    # debug mode
    #
    $tutos[debug] = 0;
    $tutos[errlog] = "/tmp/debug.out";
    #
    $tutos[jpgraph] = "/www/htdocs/tutos/php/admin/jpgraph";
    #
    # EOF
    ?>

    sudo apt-get install perl libnet-ssleay-perl openssl libauthen-pam-perl libpam-runtime libio-pty-perl libmd5-perl

4.  login

    http://192.168.1.7/tutos/php/mytutos.php

    User: superuser Password: tutos

## 第 174 章 Open Source Requirements Management Tool

http://sourceforge.net/projects/osrmt/

```

<client directory>\v1_50\client\

copy connection.mysql.xml connection.xml

```

```

<client directory>\v1_50\client\upgrade.bat

Select configuration option 1,2,3 or 4
1) Define a new connection
2) Test the connection
3) Save the new connection
4) Initialize a new database
5) Upgrade 1.3 to 1.4 database
6) Migrate database contents
7) Export language file
8) Import language file
0) Exit
Enter option number [Exit]:

Enter option number [Exit]: 4
initializing database defined in connection.xml:
jdbc:mysql://localhost/osrmt?useUnicode=true&characterEncoding=UTF-8
osrmt
mysql
Target correct? Y|N [Y]: y
Empty schema located - initialize and populate schema? [Y]:

```

## 第 175 章 Jenkins

### 1. 安装 Jenkins

#### 1.1. OSCM 一键安装

```

yum install -y java-1.8.0-openjdk			
curl -s https://raw.githubusercontent.com/oscm/shell/master/project/jenkins/jenkins.sh | bash

```

#### 1.2. Mac

使用 pkg 方式安装，默认路径是 /Applications/Jenkins/jenkins.war

```

export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_92.jdk/Contents/Home
java -jar jenkins.war --httpPort=8080

```

浏览器访问：http://localhost:8080

查看默认密码 /Users/neo/.jenkins/secrets/initialAdminPassword

```

neo@MacBook-Pro ~ % cat /Users/neo/.jenkins/secrets/initialAdminPassword
6c7369afc6c1414586b6644657dd655a		

```

下载 cloudbees 插件

```

neo@MacBook-Pro ~ % cd ~/.jenkins/plugins
neo@MacBook-Pro ~/.jenkins/plugins % wget ftp://ftp.icm.edu.pl/packages/jenkins/plugins/cloudbees-folder//6.7/cloudbees-folder.hpi			

```

重启 Jenkens http://localhost:8080/restart

复制上面的密码，粘贴到浏览器中。

卸载 Jenkens

```

sudo rm -rf /var/root/.jenkins ~/.jenkins
sudo launchctl unload /Library/LaunchDaemons/org.jenkins-ci.plist
sudo rm /Library/LaunchDaemons/org.jenkins-ci.plist
sudo rm -rf /Applications/Jenkins "/Library/Application Support/Jenkins" /Library/Documentation/Jenkins

sudo rm -rf /Users/Shared/Jenkins
sudo dscl . -delete /Users/jenkins
sudo dscl . -delete /Groups/jenkins
sudo rm -f /etc/newsyslog.d/jenkins.conf
pkgutil --pkgs | grep 'org\.jenkins-ci\.' | xargs -n 1 sudo pkgutil --forget

```

由于我的 Mac 模式是 JDK 11，所以需要制定 JAVA_HOME 到 JDK 1.8，否则提示

```

Dec 27, 2018 9:20:33 AM Main main
SEVERE: Running with Java class version 55.0, but 52.0 is required.Run with the --enable-future-java flag to enable such behavior. See https://jenkins.io/redirect/java-support/
java.lang.UnsupportedClassVersionError: 55.0
	at Main.main(Main.java:139)

Jenkins requires Java 8, but you are running 11+28 from /Library/Java/JavaVirtualMachines/jdk-11.jdk/Contents/Home
java.lang.UnsupportedClassVersionError: 55.0
	at Main.main(Main.java:139)		

```

#### 1.3. CentOS

```

wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key

yum install -y jenkins			

```

cat /etc/sysconfig/jenkins

```

## Path:        Development/Jenkins
## Description: Jenkins Automation Server
## Type:        string
## Default:     "/var/lib/jenkins"
## ServiceRestart: jenkins
#
# Directory where Jenkins store its configuration and working
# files (checkouts, build reports, artifacts, ...).
#
JENKINS_HOME="/var/lib/jenkins"

## Type:        string
## Default:     ""
## ServiceRestart: jenkins
#
# Java executable to run Jenkins
# When left empty, we'll try to find the suitable Java.
#
JENKINS_JAVA_CMD=""

## Type:        string
## Default:     "jenkins"
## ServiceRestart: jenkins
#
# Unix user account that runs the Jenkins daemon
# Be careful when you change this, as you need to update
# permissions of $JENKINS_HOME and /var/log/jenkins.
#
JENKINS_USER="jenkins"

## Type:        string
## Default: "false"
## ServiceRestart: jenkins
#
# Whether to skip potentially long-running chown at the
# $JENKINS_HOME location. Do not enable this, "true", unless
# you know what you're doing. See JENKINS-23273.
#
#JENKINS_INSTALL_SKIP_CHOWN="false"

## Type: string
## Default:     "-Djava.awt.headless=true"
## ServiceRestart: jenkins
#
# Options to pass to java when running Jenkins.
#
JENKINS_JAVA_OPTIONS="-Djava.awt.headless=true"

## Type:        integer(0:65535)
## Default:     8080
## ServiceRestart: jenkins
#
# Port Jenkins is listening on.
# Set to -1 to disable
#
JENKINS_PORT="8080"

## Type:        string
## Default:     ""
## ServiceRestart: jenkins
#
# IP address Jenkins listens on for HTTP requests.
# Default is all interfaces (0.0.0.0).
#
JENKINS_LISTEN_ADDRESS=""

## Type:        integer(0:65535)
## Default:     ""
## ServiceRestart: jenkins
#
# HTTPS port Jenkins is listening on.
# Default is disabled.
#
JENKINS_HTTPS_PORT=""

## Type:        string
## Default:     ""
## ServiceRestart: jenkins
#
# Path to the keystore in JKS format (as created by the JDK 'keytool').
# Default is disabled.
#
JENKINS_HTTPS_KEYSTORE=""

## Type:        string
## Default:     ""
## ServiceRestart: jenkins
#
# Password to access the keystore defined in JENKINS_HTTPS_KEYSTORE.
# Default is disabled.
#
JENKINS_HTTPS_KEYSTORE_PASSWORD=""

## Type:        string
## Default:     ""
## ServiceRestart: jenkins
#
# IP address Jenkins listens on for HTTPS requests.
# Default is disabled.
#
JENKINS_HTTPS_LISTEN_ADDRESS=""

## Type:        integer(1:9)
## Default:     5
## ServiceRestart: jenkins
#
# Debug level for logs -- the higher the value, the more verbose.
# 5 is INFO.
#
JENKINS_DEBUG_LEVEL="5"

## Type:        yesno
## Default:     no
## ServiceRestart: jenkins
#
# Whether to enable access logging or not.
#
JENKINS_ENABLE_ACCESS_LOG="no"

## Type:        integer
## Default:     100
## ServiceRestart: jenkins
#
# Maximum number of HTTP worker threads.
#
JENKINS_HANDLER_MAX="100"

## Type:        integer
## Default:     20
## ServiceRestart: jenkins
#
# Maximum number of idle HTTP worker threads.
#
JENKINS_HANDLER_IDLE="20"

## Type:        string
## Default:     ""
## ServiceRestart: jenkins
#
# Pass arbitrary arguments to Jenkins.
# Full option list: java -jar jenkins.war --help
#
JENKINS_ARGS=""

```

Nginx 配置

```

[root@netkiller ~]# cat /etc/nginx/conf.d/jk.netkiller.cn.conf 
server {
    listen       80;
    server_name  jk.netkiller.cn;

    charset utf-8;

    location / {
    	proxy_pass   http://127.0.0.1:8080;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
}

```

查看管理员密码

```

cat /var/lib/jenkins/secrets/initialAdminPassword			

```

#### 1.4. Ubuntu

```

wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
deb https://pkg.jenkins.io/debian-stable binary/

sudo apt-get update
sudo apt-get install jenkins			

```

#### 1.5. Docker

[`github.com/jenkinsci/docker/blob/master/README.md`](https://github.com/jenkinsci/docker/blob/master/README.md)

8080 端口是 jenkins 的端口，5000 端口是 master 和 slave 通信端口

```

docker pull jenkins/jenkins:lts			
docker run -p 8080:8080 -p 50000:50000 --name jenkins jenkins/jenkins:lts

```

首次启动，不要使用 -d 参数，如果使用了 -d 参数可以通过 docker logs -f jenkins 查看控制台的密码

docker-compose 配置文件

```

version: '2'

services:
  jenkins:
    container_name: jenkins-lts
    ports: # 端口映射，9001 为宿主机上的端口，相应的 8080 是容器运行起来时候 jenkins 服务的端口
      - 9001:8080
      - 50000:50000
    image: jenkins/jenkins:lts # 指定运行用哪一个镜像来运行容器
    volumes:
      - /home/jenkins/jenkins_home:/var/jenkins_home # 挂载指令，目的在于销毁容器时，并不影响 jenkins 数据			

```

#### 1.6. Minikube

创建 jenkins-namespace.yaml

```

apiVersion: v1
kind: Namespace
metadata:
  name: jenkins-project

```

创建命名空间

```

$ kubectl create -f jenkins-namespace.yaml

```

创建 jenkins-volume.yaml

```

apiVersion: v1
kind: PersistentVolume
metadata:
  name: jenkins-pv
  namespace: jenkins-project
spec:
  storageClassName: jenkins-pv
  accessModes:
    - ReadWriteOnce
  capacity:
    storage: 20Gi
  persistentVolumeReclaimPolicy: Retain
  hostPath:
    path: /opt/jenkins-volume/	

```

创建卷

```

$ kubectl create -f jenkins-volume.yaml
persistentvolume “jenkins-pv” created			

```

创建 jenkins-values.yaml 文件

```

# Default values for jenkins.
# This is a YAML-formatted file.
# Declare name/value pairs to be passed into your templates.
# name: value

## Overrides for generated resource names
# See templates/_helpers.tpl
# nameOverride:
# fullnameOverride:

Master:
  Name: jenkins-master
  Image: "jenkins/jenkins"
  ImageTag: "2.141"
  ImagePullPolicy: "Always"
  Component: "jenkins-master"
  UseSecurity: true
  AdminUser: admin
  # AdminPassword: <defaults to random>
  Cpu: "200m"
  Memory: "256Mi"
  ServicePort: 8080
  # For minikube, set this to NodePort, elsewhere use LoadBalancer
  # <to set explicitly, choose port between 30000-32767>
  ServiceType: NodePort
  NodePort: 32000
  ServiceAnnotations: {}
  ContainerPort: 8080
  # Enable Kubernetes Liveness and Readiness Probes
  HealthProbes: true
  HealthProbesTimeout: 60
  SlaveListenerPort: 50000
  LoadBalancerSourceRanges:
    - 0.0.0.0/0
  # List of plugins to be install during Jenkins master start
  InstallPlugins:
    - kubernetes:1.12.4
    - workflow-aggregator:2.5
    - workflow-job:2.24
    - credentials-binding:1.16
    - git:3.9.1
    - greenballs:1.15
  # Used to approve a list of groovy functions in pipelines used the script-security plugin. Can be viewed under /scriptApproval
  ScriptApproval:
    - "method groovy.json.JsonSlurperClassic parseText java.lang.String"
    - "new groovy.json.JsonSlurperClassic"
    - "staticMethod org.codehaus.groovy.runtime.DefaultGroovyMethods leftShift java.util.Map java.util.Map"
    - "staticMethod org.codehaus.groovy.runtime.DefaultGroovyMethods split java.lang.String"
  CustomConfigMap: false
  NodeSelector: {}
  Tolerations: {}

Agent:
  Enabled: true
  Image: jenkins/jnlp-slave
  ImageTag: 3.10-1
  Component: "jenkins-slave"
  Privileged: false
  Cpu: "200m"
  Memory: "256Mi"
  # You may want to change this to true while testing a new image
  AlwaysPullImage: false
  # You can define the volumes that you want to mount for this container
  # Allowed types are: ConfigMap, EmptyDir, HostPath, Nfs, Pod, Secret
  volumes:
    - type: HostPath
      hostPath: /var/run/docker.sock
      mountPath: /var/run/docker.sock
  NodeSelector: {}

Persistence:
  Enabled: true
  ## A manually managed Persistent Volume and Claim
  ## Requires Persistence.Enabled: true
  ## If defined, PVC must be created manually before volume will be bound
  # ExistingClaim:
  ## jenkins data Persistent Volume Storage Class
  StorageClass: jenkins-pv

  Annotations: {}
  AccessMode: ReadWriteOnce
  Size: 20Gi
  volumes:
  #  - name: nothing
  #    emptyDir: {}
  mounts:
  #  - mountPath: /var/nothing
  #    name: nothing
  #    readOnly: true

NetworkPolicy:
  # Enable creation of NetworkPolicy resources.
  Enabled: false
  # For Kubernetes v1.4, v1.5 and v1.6, use 'extensions/v1beta1'
  # For Kubernetes v1.7, use 'networking.k8s.io/v1'
  ApiVersion: networking.k8s.io/v1

## Install Default RBAC roles and bindings
rbac:
  install: true
  serviceAccountName: default
  # RBAC api version (currently either v1beta1 or v1alpha1)
  apiVersion: v1beta1
  # Cluster role reference
  roleRef: cluster-admin			

```

使用 helm 安装 jenkins

```

$ cd ~/minikube-helm-jenkins
$ helm init
$ helm install --name jenkins -f helm/jenkins-values.yaml stable/jenkins --namespace jenkins-project

```

查看 jenkins 密码

```

$ printf $(kubectl get secret --namespace jenkins-project jenkins -o jsonpath="{.data.jenkins-admin-password}" | base64 --decode);echo			

```

### 2. 配置 Jenkins

配置 Jenkins

| ![](img/Getting-Started-1.png) |

输入管理员密码

| ![](img/Getting-Started-2.png) |

安装插件

| ![](img/Getting-Started-3.png) |

创建用户

| ![](img/Getting-Started-4.png) |

设置域名

| ![](img/Getting-Started-5.png) |

开始使用 Jenkins

| ![](img/Getting-Started-6.png) |

Jenkins 界面

| ![](img/Getting-Started-7.png) |

### 3. Jenkinsfile

#### 3.1. Jenkinsfile - Declarative Pipeline

[`jenkins.io/doc/pipeline/examples/`](https://jenkins.io/doc/pipeline/examples/)

##### 3.1.1. stages

```

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}			

```

##### 3.1.2. script

```

// Declarative //
pipeline {
    agent any
    stages {
        stage('Example') {
            steps {
                echo 'Hello World'
                script {
                    def browsers = ['chrome', 'firefox']
                    for (int i = 0; i < browsers.size(); ++i) {
                        echo "Testing the ${browsers[i]} browser"
                    }
                }
                script {
                    // 一个优雅的退出 pipeline 的方法，这里可执行任意逻辑
                    if( $VALUE1 == $VALUE2 ) {
                       currentBuild.result = 'SUCCESS'
                       return
                    }
                }
            }
        }
    }
}			

```

##### 3.1.3. junit

junit4

```

		stage("测试") {
            steps {
                echo "单元测试中..."
                // 请在这里放置您项目代码的单元测试调用过程，例如:
                sh 'mvn test' // mvn 示例
              	// sh './gradlew test'
                echo "单元测试完成."
                junit 'target/surefire-reports/*.xml' // 收集单元测试报告的调用过程
            }
        }			

```

junit5 测试报告路径与 junit4 的位置不同

```

		stage("测试") {
            steps {
                echo "单元测试中..."
              	sh './gradlew test'
                echo "单元测试完成."
              	junit 'build/test-results/test/*.xml'
            }
        }

```

##### 3.1.4. withEnv

```

env.PROJECT_DIR='src/netkiller'			
node {
    withEnv(["GOPATH=$WORKSPACE"]) {
        stage('Init gopath') {
            sh 'mkdir -p $GOPATH/{bin,pkg,src}'
        }

        stage('Build go proejct') {      
            sh 'cd ${PROJECT_DIR}; go test && go build && go install'
        }
    }
}

```

```

node {
     git 'https://github.com/netkiller/api.git'
     withEnv(["PATH+MAVEN=${tool 'm3'}/bin"]) {
          sh "mvn -B –Dmaven.test.failure.ignore=true clean package"
     }
     stash excludes: 'target/', includes: '**', name: 'source'
}			

```

##### 3.1.5. parameters

参数指令，触发这个管道需要用户指定的参数，然后在 step 中通过 params 对象访问这些参数。

```

// Declarative //
pipeline {
    agent any
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')
    }
    stages {
        stage('Example') {
            steps {
                echo "Hello ${params.PERSON}"
            }
        }
    }
}			

```

```

Jenkinsfile (Declarative Pipeline)
pipeline {
    agent any
    parameters {
        string(name: 'PERSON', defaultValue: 'Mr Jenkins', description: 'Who should I say hello to?')

        text(name: 'BIOGRAPHY', defaultValue: '', description: 'Enter some information about the person')

        booleanParam(name: 'TOGGLE', defaultValue: true, description: 'Toggle this value')

        choice(name: 'CHOICE', choices: ['One', 'Two', 'Three'], description: 'Pick something')

        password(name: 'PASSWORD', defaultValue: 'SECRET', description: 'Enter a password')

        file(name: "FILE", description: "Choose a file to upload")
    }
    stages {
        stage('Example') {
            steps {
                echo "Hello ${params.PERSON}"

                echo "Biography: ${params.BIOGRAPHY}"

                echo "Toggle: ${params.TOGGLE}"

                echo "Choice: ${params.CHOICE}"

                echo "Password: ${params.PASSWORD}"
            }
        }
    }
}			

```

##### 3.1.6. options

还能定义一些管道特定的选项，介绍几个常用的：

```

skipDefaultCheckout - 在 agent 指令中忽略源码 checkout 这一步骤。
timeout - 超时设置 options { timeout(time: 1, unit: 'HOURS') }
retry - 直到成功的重试次数 options { retry(3) }
timestamps - 控制台输出前面加时间戳 options { timestamps() }			

```

##### 3.1.7. triggers

触发器指令定义了这个管道何时该执行，就可以定义两种 cron 和 pollSCM

```

cron - linux 的 cron 格式 triggers { cron('H 4/* 0 0 1-5') }
pollSCM - jenkins 的 poll scm 语法，比如 triggers { pollSCM('H 4/* 0 0 1-5') }

// Declarative //
pipeline {
    agent any
    triggers {
        cron('H 4/* 0 0 1-5')
    }
    stages {
        stage('Example') {
            steps {
                echo 'Hello World'
            }
        }
    }
}			

```

一般我们会将管道和 GitHub、GitLab、BitBucket 关联， 然后使用它们的 webhooks 来触发，就不需要这个指令了。

##### 3.1.8. tools

定义自动安装并自动放入 PATH 里面的工具集合

```

// Declarative //
pipeline {
    agent any
    tools {
        maven 'apache-maven-3.5.0' ①
    }
    stages {
        stage('Example') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}		

```

注：① 工具名称必须预先在 Jenkins 中配置好了 → Global Tool Configuration.

##### 3.1.9. post

post section 定义了管道执行结束后要进行的操作。支持在里面定义很多 Conditions 块： always, changed, failure, success 和 unstable。 这些条件块会根据不同的返回结果来执行不同的逻辑。

```

always：不管返回什么状态都会执行
changed：如果当前管道返回值和上一次已经完成的管道返回值不同时候执行
failure：当前管道返回状态值为”failed”时候执行，在 Web UI 界面上面是红色的标志
success：当前管道返回状态值为”success”时候执行，在 Web UI 界面上面是绿色的标志
unstable：当前管道返回状态值为”unstable”时候执行，通常因为测试失败，代码不合法引起的。在 Web UI 界面上面是黄色的标志

// Declarative //
pipeline {
    agent any
    stages {
        stage('Example') {
            steps {
                echo 'Hello World'
            }
        }
    }
    post {
        always {
            echo 'I will always say Hello again!'
        }
    }
}			

```

失败发送邮件的例子

```

    post {
        failure {
            mail to: "${email}",
            subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
            body: "Something is wrong with ${env.BUILD_URL}"
        }
    }			

```

##### 3.1.10. when 条件判断

```

branch - 分支匹配才执行 when { branch 'master' }
environment - 环境变量匹配才执行 when { environment name: 'DEPLOY_TO', value: 'production' }
expression - groovy 表达式为真才执行 expression { return params.DEBUG_BUILD } }

// Declarative //
pipeline {
    agent any
    stages {
        stage('Example Build') {
            steps {
                echo 'Hello World'
            }
        }
        stage('Example Deploy') {
            when {
                branch 'production'
            }
            echo 'Deploying'
        }
    }
}			

```

##### 3.1.11. 抛出错误

```

error '执行出错'

```

##### 3.1.12. withCredentials

###### withCredentials: Bind credentials to variables

###### 3.1.12.1. token

```

node {
   withCredentials([string(credentialsId: 'token', variable: 'TOKEN')]) {
        sh('echo $TOKEN')
   }
}				

```

##### 3.1.13. withMaven

```

		withMaven(
            maven: 'M3') {
                sh "mvn test"
        }			

```

##### 3.1.14. isUnix() 判断操作系统类型

```

pipeline{

	agent any
	stages{
		stage("isUnix") {
			steps{
				script {
					if(isUnix() == true) {
						echo("this jenkins job running on a linux-like system")
					}else {
						error("the jenkins job running on a windows system")
					}
				}
			}
		}
	}
}			

```

##### 3.1.15. Jenkins pipeline 中使用 sshpass 实现 scp, ssh 远程运行

```

pipeline {
    agent {
        label "java-8"
    }
    stages  {

     	stage("环境") {
            steps {
                parallel "Maven": {
                  	script{
                      	sh 'mvn -version'
                  	}
                }, "Java": {
                    sh 'java -version'
                }, "sshpass": {
                  	sh 'apt install -y sshpass'
                    sh 'sshpass -v'
                }
            }

        }

        stage("检出") {
            steps {
                checkout(
                  [$class: 'GitSCM', branches: [[name: env.GIT_BUILD_REF]], 
                  userRemoteConfigs: [[url: env.GIT_REPO_URL]]]
                )
            }
        }

        stage("构建") {
            steps {
                echo "构建中..."
              	sh 'mvn package -Dmaven.test.skip=true'
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
                echo "构建完成."
            }
        }

        stage("测试") {
            steps {
                parallel "单元测试": {
                    echo "单元测试中..."
                    sh 'mvn test'
                    echo "单元测试完成."
                    junit 'target/surefire-reports/*.xml'
                }, "接口测试": {
                    echo "接口测试中..."
                    // 请在这里放置您项目代码的单元测试调用过程，例如 mvn test
                    echo "接口测试完成."
                }, "测试敏感词":{
                    echo "Username: ${env.username}"
            		echo "Password: ${env.password}"
                }

            }

        }
      	stage("运行"){
      		steps {
            	sh 'java -jar target/java-0.0.1-SNAPSHOT.jar'
    	    }
    	}
      	stage("部署"){
      		steps {
              echo "上传"
              sh 'sshpass -p Passw0rd scp target/*.jar root@dev.netkiller.cn:/root/'
              echo "运行"
              sh 'sshpass -p Passw0rd ssh root@dev.netkiller.cn java -jar /root/java-0.0.1-SNAPSHOT.jar'

    	    }
    	}
    }
}			

```

###### 3.1.15.1. 后台运行

```

		stage("部署"){
            parallel{
	            stage("sshpass") {
		        	steps{
		        	    sh 'apt install -y sshpass'
		                sh 'sshpass -v'
		        	}
		        }
            	stage('stop') {
			         steps {
			            sh 'sshpass -p passw0rd ssh -f dev.netkiller.cn pkill -f java-project-0.0.2-SNAPSHOT'
			         }
			    }
			    stage('start') {
			         steps {
			            sh 'sshpass -p passw0rd scp target/*.jar dev.netkiller.cn:/root/'
                	    sh 'sshpass -p passw0rd ssh -f dev.netkiller.cn java -jar /root/java-project-0.0.2-SNAPSHOT.jar'
			          }
			    }	                
    	    }
    	}				

```

#### 3.2. Jenkinsfile - Scripted Pipeline

```

// Jenkinsfile (Scripted Pipeline)
node {
    stage('Build') {
        echo 'Building....'
    }
    stage('Test') {
        echo 'Building....'
    }
    stage('Deploy') {
        echo 'Deploying....'
    }
}			

```

##### 3.2.1. git

```

node {

   stage('Checkout') {
      git 'https://github.com/bg7nyt/java.git'
   }

}

```

##### 3.2.2. 切换 JDK 版本

```

node('vagrant-slave') {
    env.JAVA_HOME="${tool 'jdk-8u45'}"
    env.PATH="${env.JAVA_HOME}/bin:${env.PATH}"
    sh 'java -version'
}			

```

##### 3.2.3. groovy

```

#!groovy
import groovy.json.JsonOutput
import groovy.json.JsonSlurper

/*
Please make sure to add the following environment variables:
HEROKU_PREVIEW=<your heroku preview app>
HEROKU_PREPRODUCTION=<your heroku pre-production app>
HEROKU_PRODUCTION=<your heroku production app>
Please also add the following credentials to the global domain of your organization's folder:
Heroku API key as secret text with ID 'HEROKU_API_KEY'
GitHub Token value as secret text with ID 'GITHUB_TOKEN'
*/

node {

     server = Artifactory.server "artifactory"
     buildInfo = Artifactory.newBuildInfo()
     buildInfo.env.capture = true

    // we need to set a newer JVM for Sonar
    env.JAVA_HOME="${tool 'Java SE DK 8u131'}"
    env.PATH="${env.JAVA_HOME}/bin:${env.PATH}"

    // pull request or feature branch
    if  (env.BRANCH_NAME != 'master') {
        checkout()
        build()
        unitTest()
        // test whether this is a regular branch build or a merged PR build
        if (!isPRMergeBuild()) {
            preview()
            sonarServer()
            allCodeQualityTests()
        } else {
            // Pull request
            sonarPreview()
        }
    } // master branch / production
    else { 
        checkout()
        build()
        allTests()
        preview()
        sonarServer()
        allCodeQualityTests()
        preProduction()
        manualPromotion()
        production()
    }
}

def isPRMergeBuild() {
    return (env.BRANCH_NAME ==~ /^PR-\d+$/)
}

def sonarPreview() {
    stage('SonarQube Preview') {
        prNo = (env.BRANCH_NAME=~/^PR-(\d+)$/)[0][1]
        mvn "org.jacoco:jacoco-maven-plugin:prepare-agent install -Dmaven.test.failure.ignore=true -Pcoverage-per-test"
        withCredentials([[$class: 'StringBinding', credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN']]) {
            githubToken=env.GITHUB_TOKEN
            repoSlug=getRepoSlug()
            withSonarQubeEnv('SonarQube Octodemoapps') {
                mvn "-Dsonar.analysis.mode=preview -Dsonar.github.pullRequest=${prNo} -Dsonar.github.oauth=${githubToken} -Dsonar.github.repository=${repoSlug} -Dsonar.github.endpoint=https://api.github.com/ org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar"
            }
        }
    } 
}

def sonarServer() {
    stage('SonarQube Server') {
        mvn "org.jacoco:jacoco-maven-plugin:prepare-agent install -Dmaven.test.failure.ignore=true -Pcoverage-per-test"
        withSonarQubeEnv('SonarQube Octodemoapps') {
            mvn "org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar"
        }

        context="sonarqube/qualitygate"
        setBuildStatus ("${context}", 'Checking Sonarqube quality gate', 'PENDING')
        timeout(time: 1, unit: 'MINUTES') { // Just in case something goes wrong, pipeline will be killed after a timeout
            def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
            if (qg.status != 'OK') {
                setBuildStatus ("${context}", "Sonarqube quality gate fail: ${qg.status}", 'FAILURE')
                error "Pipeline aborted due to quality gate failure: ${qg.status}"
            } else {
                setBuildStatus ("${context}", "Sonarqube quality gate pass: ${qg.status}", 'SUCCESS')
            }    
        }
    }
}

def checkout () {
    stage 'Checkout code'
    context="continuous-integration/jenkins/"
    context += isPRMergeBuild()?"pr-merge/checkout":"branch/checkout"
    checkout scm
    setBuildStatus ("${context}", 'Checking out completed', 'SUCCESS')
}

def build () {
    stage 'Build'
    mvn 'clean install -DskipTests=true -Dmaven.javadoc.skip=true -Dcheckstyle.skip=true -B -V'
}

def unitTest() {
    stage 'Unit tests'
    mvn 'test -B -Dmaven.javadoc.skip=true -Dcheckstyle.skip=true'
    if (currentBuild.result == "UNSTABLE") {
        sh "exit 1"
    }
}

def allTests() {
    stage 'All tests'
    // don't skip anything
    mvn 'test -B'
    step([$class: 'JUnitResultArchiver', testResults: '**/target/surefire-reports/TEST-*.xml'])
    if (currentBuild.result == "UNSTABLE") {
        // input "Unit tests are failing, proceed?"
        sh "exit 1"
    }
}

def allCodeQualityTests() {
    stage 'Code Quality'
    lintTest()
    coverageTest()
}

def lintTest() {
    context="continuous-integration/jenkins/linting"
    setBuildStatus ("${context}", 'Checking code conventions', 'PENDING')
    lintTestPass = true

    try {
        mvn 'verify -DskipTests=true'
    } catch (err) {
        setBuildStatus ("${context}", 'Some code conventions are broken', 'FAILURE')
        lintTestPass = false
    } finally {
        if (lintTestPass) setBuildStatus ("${context}", 'Code conventions OK', 'SUCCESS')
    }
}

def coverageTest() {
    context="continuous-integration/jenkins/coverage"
    setBuildStatus ("${context}", 'Checking code coverage levels', 'PENDING')

    coverageTestStatus = true

    try {
        mvn 'cobertura:check'
    } catch (err) {
        setBuildStatus("${context}", 'Code coverage below 90%', 'FAILURE')
        throw err
    }

    setBuildStatus ("${context}", 'Code coverage above 90%', 'SUCCESS')

}

def preview() {
    stage name: 'Deploy to Preview env', concurrency: 1
    def herokuApp = "${env.HEROKU_PREVIEW}"
    def id = createDeployment(getBranch(), "preview", "Deploying branch to test")
    echo "Deployment ID: ${id}"
    if (id != null) {
        setDeploymentStatus(id, "pending", "https://${herokuApp}.herokuapp.com/", "Pending deployment to test");
        herokuDeploy "${herokuApp}"
        setDeploymentStatus(id, "success", "https://${herokuApp}.herokuapp.com/", "Successfully deployed to test");
    }
    mvn 'deploy -DskipTests=true'
}

def preProduction() {
    stage name: 'Deploy to Pre-Production', concurrency: 1
    switchSnapshotBuildToRelease()
    herokuDeploy "${env.HEROKU_PREPRODUCTION}"
    buildAndPublishToArtifactory()
}

def manualPromotion() {
    // we need a first milestone step so that all jobs entering this stage are tracked an can be aborted if needed
    milestone 1
    // time out manual approval after ten minutes
    timeout(time: 10, unit: 'MINUTES') {
        input message: "Does Pre-Production look good?"
    }
    // this will kill any job which is still in the input step
    milestone 2
}

def production() {
    stage name: 'Deploy to Production', concurrency: 1
    step([$class: 'ArtifactArchiver', artifacts: '**/target/*.jar', fingerprint: true])
    herokuDeploy "${env.HEROKU_PRODUCTION}"
    def version = getCurrentHerokuReleaseVersion("${env.HEROKU_PRODUCTION}")
    def createdAt = getCurrentHerokuReleaseDate("${env.HEROKU_PRODUCTION}", version)
    echo "Release version: ${version}"
    createRelease(version, createdAt)
    promoteInArtifactoryAndDistributeToBinTray()
}

def switchSnapshotBuildToRelease() {
    def descriptor = Artifactory.mavenDescriptor()
    descriptor.version = '1.0.0'
    descriptor.pomFile = 'pom.xml'
    descriptor.transform()
}

def buildAndPublishToArtifactory() {       
        def rtMaven = Artifactory.newMavenBuild()
        rtMaven.tool = "Maven 3.x"
        rtMaven.deployer releaseRepo:'libs-release-local', snapshotRepo:'libs-snapshot-local', server: server
        rtMaven.resolver releaseRepo:'libs-release', snapshotRepo:'libs-snapshot', server: server
        rtMaven.run pom: 'pom.xml', goals: 'install', buildInfo: buildInfo
        server.publishBuildInfo buildInfo
}

def promoteBuildInArtifactory() {
        def promotionConfig = [
            // Mandatory parameters
            'buildName'          : buildInfo.name,
            'buildNumber'        : buildInfo.number,
            'targetRepo'         : 'libs-prod-local',

            // Optional parameters
            'comment'            : 'deploying to production',
            'sourceRepo'         : 'libs-release-local',
            'status'             : 'Released',
            'includeDependencies': false,
            'copy'               : true,
            // 'failFast' is true by default.
            // Set it to false, if you don't want the promotion to abort upon receiving the first error.
            'failFast'           : true
        ]

        // Promote build
        server.promote promotionConfig
}

def distributeBuildToBinTray() {
        def distributionConfig = [
            // Mandatory parameters
            'buildName'             : buildInfo.name,
            'buildNumber'           : buildInfo.number,
            'targetRepo'            : 'reading-time-dist',  
            // Optional parameters
            //'publish'               : true, // Default: true. If true, artifacts are published when deployed to Bintray.
            'overrideExistingFiles' : true, // Default: false. If true, Artifactory overwrites builds already existing in the target path in Bintray.
            //'gpgPassphrase'         : 'passphrase', // If specified, Artifactory will GPG sign the build deployed to Bintray and apply the specified passphrase.
            //'async'                 : false, // Default: false. If true, the build will be distributed asynchronously. Errors and warnings may be viewed in the Artifactory log.
            //"sourceRepos"           : ["yum-local"], // An array of local repositories from which build artifacts should be collected.
            //'dryRun'                : false, // Default: false. If true, distribution is only simulated. No files are actually moved.
        ]
        server.distribute distributionConfig
}

def promoteInArtifactoryAndDistributeToBinTray() {
    stage ("Promote in Artifactory and Distribute to BinTray") {
        promoteBuildInArtifactory()
        distributeBuildToBinTray()
    }
}

def mvn(args) {
    withMaven(
        // Maven installation declared in the Jenkins "Global Tool Configuration"
        maven: 'Maven 3.x',
        // Maven settings.xml file defined with the Jenkins Config File Provider Plugin

        // settings.xml referencing the GitHub Artifactory repositories
         mavenSettingsConfig: '0e94d6c3-b431-434f-a201-7d7cda7180cb',
        // we do not need to set a special local maven repo, take the one from the standard box
        //mavenLocalRepo: '.repository'
        ) {
        // Run the maven build
        sh "mvn $args -Dmaven.test.failure.ignore"
    }
}

def herokuDeploy (herokuApp) {
    withCredentials([[$class: 'StringBinding', credentialsId: 'HEROKU_API_KEY', variable: 'HEROKU_API_KEY']]) {
        mvn "heroku:deploy -DskipTests=true -Dmaven.javadoc.skip=true -B -V -D heroku.appName=${herokuApp}"
    }
}

def getRepoSlug() {
    tokens = "${env.JOB_NAME}".tokenize('/')
    org = tokens[tokens.size()-3]
    repo = tokens[tokens.size()-2]
    return "${org}/${repo}"
}

def getBranch() {
    tokens = "${env.JOB_NAME}".tokenize('/')
    branch = tokens[tokens.size()-1]
    return "${branch}"
}

def createDeployment(ref, environment, description) {
    withCredentials([[$class: 'StringBinding', credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN']]) {
        def payload = JsonOutput.toJson(["ref": "${ref}", "description": "${description}", "environment": "${environment}", "required_contexts": []])
        def apiUrl = "https://api.github.com/repos/${getRepoSlug()}/deployments"
        def response = sh(returnStdout: true, script: "curl -s -H \"Authorization: Token ${env.GITHUB_TOKEN}\" -H \"Accept: application/json\" -H \"Content-type: application/json\" -X POST -d '${payload}' ${apiUrl}").trim()
        def jsonSlurper = new JsonSlurper()
        def data = jsonSlurper.parseText("${response}")
        return data.id
    }
}

void createRelease(tagName, createdAt) {
    withCredentials([[$class: 'StringBinding', credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN']]) {
        def body = "**Created at:** ${createdAt}\n**Deployment job:** ${env.BUILD_NUMBER}\n**Environment:** [${env.HEROKU_PRODUCTION}](https://dashboard.heroku.com/apps/${env.HEROKU_PRODUCTION})"
        def payload = JsonOutput.toJson(["tag_name": "v${tagName}", "name": "${env.HEROKU_PRODUCTION} - v${tagName}", "body": "${body}"])
        def apiUrl = "https://api.github.com/repos/${getRepoSlug()}/releases"
        def response = sh(returnStdout: true, script: "curl -s -H \"Authorization: Token ${env.GITHUB_TOKEN}\" -H \"Accept: application/json\" -H \"Content-type: application/json\" -X POST -d '${payload}' ${apiUrl}").trim()
    }
}

void setDeploymentStatus(deploymentId, state, targetUrl, description) {
    withCredentials([[$class: 'StringBinding', credentialsId: 'GITHUB_TOKEN', variable: 'GITHUB_TOKEN']]) {
        def payload = JsonOutput.toJson(["state": "${state}", "target_url": "${targetUrl}", "description": "${description}"])
        def apiUrl = "https://api.github.com/repos/${getRepoSlug()}/deployments/${deploymentId}/statuses"
        def response = sh(returnStdout: true, script: "curl -s -H \"Authorization: Token ${env.GITHUB_TOKEN}\" -H \"Accept: application/json\" -H \"Content-type: application/json\" -X POST -d '${payload}' ${apiUrl}").trim()
    }
}

void setBuildStatus(context, message, state) {
  // partially hard coded URL because of https://issues.jenkins-ci.org/browse/JENKINS-36961, adjust to your own GitHub instance
  step([
      $class: "GitHubCommitStatusSetter",
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: context],
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: "https://octodemo.com/${getRepoSlug()}"],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}

def getCurrentHerokuReleaseVersion(app) {
    withCredentials([[$class: 'StringBinding', credentialsId: 'HEROKU_API_KEY', variable: 'HEROKU_API_KEY']]) {
        def apiUrl = "https://api.heroku.com/apps/${app}/dynos"
        def response = sh(returnStdout: true, script: "curl -s  -H \"Authorization: Bearer ${env.HEROKU_API_KEY}\" -H \"Accept: application/vnd.heroku+json; version=3\" -X GET ${apiUrl}").trim()
        def jsonSlurper = new JsonSlurper()
        def data = jsonSlurper.parseText("${response}")
        return data[0].release.version
    }
}

def getCurrentHerokuReleaseDate(app, version) {
    withCredentials([[$class: 'StringBinding', credentialsId: 'HEROKU_API_KEY', variable: 'HEROKU_API_KEY']]) {
        def apiUrl = "https://api.heroku.com/apps/${app}/releases/${version}"
        def response = sh(returnStdout: true, script: "curl -s  -H \"Authorization: Bearer ${env.HEROKU_API_KEY}\" -H \"Accept: application/vnd.heroku+json; version=3\" -X GET ${apiUrl}").trim()
        def jsonSlurper = new JsonSlurper()
        def data = jsonSlurper.parseText("${response}")
        return data.created_at
    }
}			

```

##### 3.2.4. Groovy code

###### 3.2.4.1. Groovy 函数

```

node {
    stage("Test") {
        test()
    }
}

def test() {
    echo "Start"
    sleep(5)
    echo "Stop"
}

```

##### 3.2.5. Ansi Color

```

// This shows a simple build wrapper example, using the AnsiColor plugin.
node {
    // This displays colors using the 'xterm' ansi color map.
    ansiColor('xterm') {
        // Just some echoes to show the ANSI color.
        stage "\u001B[31mI'm Red\u001B[0m Now not"
    }
}	

```

##### 3.2.6. 写文件操作

```

// This shows a simple example of how to archive the build output artifacts.
node {
    stage "Create build output"

    // Make the output directory.
    sh "mkdir -p output"

    // Write an useful file, which is needed to be archived.
    writeFile file: "output/usefulfile.txt", text: "This file is useful, need to archive it."

    // Write an useless file, which is not needed to be archived.
    writeFile file: "output/uselessfile.md", text: "This file is useless, no need to archive it."

    stage "Archive build output"

    // Archive the build output artifacts.
    archiveArtifacts artifacts: 'output/*.txt', excludes: 'output/*.md'
}			

```

##### 3.2.7. modules 实现模块

```

def modules = [
  'Java',
  'PHP',
  'Python',
  'Ruby'
]

node() {

  stage("checkout") {
    echo "checkout"
  }

  modules.each { module ->
    stage("build:${module}") {
      echo "${module}"
    }
  }
}			

```

##### 3.2.8. docker

```

node('master') {

    stage('Build') {
        docker.image('maven:3.5.0').inside {
            sh 'mvn --version'
        }
    }

    stage('Deploy') {
        if (env.BRANCH_NAME == 'master') {
            echo 'I only execute on the master branch'
        } else {
            echo 'I execute elsewhere'
        }
    }
}			

```

##### 3.2.9. input

```

node {
    stage('Git') {
        def branch = input message: 'input branch name for this job', ok: 'ok', parameters: [string(defaultValue: 'master', description: 'branch name', name: 'branch')]
        echo branch
    }
}			

```

```

node {
    stage('Git') {
        def result = input message: 'input branch name for this job', ok: 'ok', parameters: [string(defaultValue: 'master', description: 'branch name', name: 'branch'), string(defaultValue: '', description: 'commit to switch', name: 'commit')]

        echo result.branch
        echo result.commit
    }
}

node {
    stage('Git') {
        def result = input message: 'input branch name for this job', ok: 'ok', parameters: [string(defaultValue: 'master', description: 'branch name', name: 'branch'), string(defaultValue: '', description: 'commit to switch', name: 'commit')]

        sh "echo ${result.branch}"
        sh "echo ${result.commit}"
    }
}

```

##### 3.2.10. if 条件判断

```

node {
    dir('/var/www') {
        stage('Git') {
            if(fileExists('project')) {
                dir('project') {
                    sh 'git fetch origin'
                    sh 'git checkout master'
                    sh 'git pull'
                }
            } else {
                sh 'git clone git@git.netkiller.cn:neo/project.git project'
            }
        }
    }
}			

```

##### 3.2.11. Docker

```

node {
	stage("Checkout") {
		checkout([$class: 'GitSCM', branches: [[name: env.GIT_BUILD_REF]],userRemoteConfigs: [[url: env.GIT_REPO_URL]]])
	}

	docker.image('ruby').inside {
		stage("Init") {
			sh 'pwd && ls'		
			sh 'gem install rails'
			// sh 'gem install ...'		
		}	
		stage("Test") {
			sh 'ruby tc_simple_number.rb'
		}
		stage("Build") {
			sh 'ruby --version'
			archiveArtifacts artifacts: 'bin/*', fingerprint: true
		}
		stage("Deploy") {
			sh 'rsync -auzv --delete * www@host.netkiller.cn:/path/to/dir'
		}
	}
}			

```

##### 3.2.12. conditionalSteps

```

def projectName = 'myProject'

def jobClosure = {
  steps {
    conditionalSteps {
      condition {
        fileExists(projectName+'/target/test.jar', BaseDir.WORKSPACE)
      }
      runner('Fail')
      steps {
        batchFile('echo Found some tests')
      }
    }
  }
}

freeStyleJob('AAA-Test', jobClosure)			

```

##### 3.2.13. nexus

```

stage("Deploy") {
    nexusArtifactUploader artifacts: [
        [artifactId: 'java11', type: 'jar', file: 'target/java11.jar']
    ],
    groupId: 'org.springframework.samples',
    nexusUrl: 'netkiller.cn/repository/maven/',
    nexusVersion: 'nexus3',
    protocol: 'http',
    repository: 'maven',
    version: '2.0.0.BUILD'
}			

```

#### 3.3. 设置环境变量

environment 定义键值对的环境变量

```

// Declarative //
pipeline {
    agent any
    environment {
        CC = 'clang'
    }
    stages {
        stage('Example') {
            environment {
                DEBUG_FLAGS = '-g'
            }
            steps {
                sh 'printenv'
            }
        }
    }
}			

```

```

// Declarative //
pipeline {
    agent any
    environment {
        CC = 'clang'
    }
    stages {
        stage('Example') {
            environment { 
                AN_ACCESS_KEY = credentials('my-prefined-secret-text') ③
            }
            steps {
                sh 'printenv'
            }
        }
    }
}			

```

##### 3.3.1. 系统环境变量

```

echo "Running ${env.BUILD_ID} on ${env.JENKINS_URL}"			

```

```

${env.WORKSPACE}	
println env.JOB_NAME
println env.BUILD_NUMBER	

```

打印所有环境变量

```

node {
    echo sh(returnStdout: true, script: 'env')
    echo sh(script: 'env|sort', returnStdout: true)
    // ...
}		

```

```

pipeline {
  	agent any
    stages {
        stage('Environment Example') {
            steps {
              script{
              	def envs = sh(returnStdout: true, script: 'env').split('\n')
				envs.each { name  ->
    				println "Name: $name"
				}	
              }
            }
        }
    }
}	

```

#### 3.4. agent

agent 指令指定整个管道或某个特定的 stage 的执行环境。它的参数可用使用：

```

agent: 
any - 任意一个可用的 agent
none - 如果放在 pipeline 顶层，那么每一个 stage 都需要定义自己的 agent 指令
label - 在 jenkins 环境中指定标签的 agent 上面执行，比如 agent { label 'my-defined-label' }
node - agent { node { label 'labelName' } } 和 label 一样，但是可用定义更多可选项
docker - 指定在 docker 容器中运行
dockerfile - 使用源码根目录下面的 Dockerfile 构建容器来运行				

```

##### 3.4.1. label

```

	agent {
        label "java-8"
    }

```

##### 3.4.2. docker

[`jenkins.io/doc/book/pipeline/docker/`](https://jenkins.io/doc/book/pipeline/docker/)

添加 jenkins 用户到 docker 组

```

[root@localhost ~]# gpasswd -a jenkins docker
Adding user jenkins to group docker

[root@localhost ~]# cat /etc/group | grep ^docker
docker:x:993:jenkins	

```

###### 3.4.2.1. 指定 docker 镜像

```

pipeline {
    agent { docker { image 'maven:3.3.3' } }
    stages {
        stage('build') {
            steps {
                sh 'mvn --version'
            }
        }
    }
}

```

```

pipeline {
    agent { docker { image 'php' } }
    stages {
        stage('build') {
            steps {
                sh 'php --version'
            }
        }
    }
}

pipeline {
    agent {
        docker { image 'php:latest' }
    }
    stages {
        stage('Test') {
            steps {
                sh 'php --version'
            }
        }
    }
}

```

###### 3.4.2.2. args 参数

挂在 /root/.m2 目录

```

pipeline {
    agent {
        docker {
            image 'maven:latest'
            args '-v $HOME/.m2:/root/.m2'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B'
            }
        }
    }
}			

```

###### 3.4.2.3. Docker outside of Docker (DooD)

```

	docker.image('maven').inside("-v /var/run/docker.sock:/var/run/docker.sock -v /usr/bin/docker:/usr/bin/docker") {
		sh 'docker images'
	}				

```

###### 3.4.2.4. 挂在宿主主机目录

```

node {
    stage("Checkout") {
        checkout(
          [$class: 'GitSCM', branches: [[name: env.GIT_BUILD_REF]], userRemoteConfigs: [[url: env.GIT_REPO_URL]]]
        )
		sh 'pwd'
    }
	docker.image('maven:latest').inside("-v /root/.m2:/root/.m2") {
        stage("Build") {
                sh 'java -version'
                sh 'mvn package -Dmaven.test.failure.ignore -Dmaven.test.skip=true'
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true
        }

        stage("Test") {
                sh 'java -jar target/webflux-0.0.1-SNAPSHOT.jar &'
                sleep 20
                sh 'mvn test -Dmaven.test.failure.ignore'
                junit 'target/surefire-reports/*.xml'
        }
    }
}				

```

###### 3.4.2.5. 构建镜像

```

node {
    checkout scm

    docker.withRegistry('http://hub.netkiller.cn:5000') {

        def customImage = docker.build("project/api:1.0")

        /* Push the container to the custom Registry */
        customImage.push()
    }
}				

```

容器内运行脚本

```

node {
    checkout scm

    def customImage = docker.build("my-image:${env.BUILD_ID}")

    customImage.inside {
        sh 'make test'
    }
}

```

```

		dir ('example') {
            /* 构建镜像 */
            def customImage = docker.build("example-group/example:${params.VERSION}")

            /* hub.netkiller.cn 是你的 Docker Registry */
            docker.withRegistry('https://hub.netkiller.cn/', 'docker-registry') {
                /* Push the container to the custom Registry */
                // push 指定版本
                customImage.push('latest')
            }
        }				

```

```

stage('DockerBuild') {
    sh """
    rm -f src/docker/*.jar
    cp target/*.jar src/docker/*.jar
    """

    dir ("src/docker/") {
        def image = docker.build("your/demo:1.0.0")
        image.push()
    }
}				

```

##### 3.4.3. Dockerfile

创建 Dockerfile 文件

```

FROM node:7-alpine

RUN apk add -U subversion

```

创建 Jenkinsfile 文件

```

// Jenkinsfile (Declarative Pipeline)
pipeline {
    agent { dockerfile true }
    stages {
        stage('Test') {
            steps {
                sh 'node --version'
                sh 'svn --version'
            }
        }
    }
}

```

#### 3.5. Steps

##### 3.5.1. parallel 平行执行

```

	stage('test') {
      parallel {
        stage('test') {
          steps {
            echo 'hello'
          }
        }
        stage('test1') {
          steps {
            sleep 1
          }
        }
        stage('test2') {
          steps {
            retry(count: 5) {
              echo 'hello'
            }

          }
        }
      }			

```

##### 3.5.2. echo

```

    stage('Deploy') {
        echo 'Deploying....'
    }

```

##### 3.5.3. catchError 捕获错误

```

node {
    catchError {
        sh 'might fail'
    }
    step([$class: 'Mailer', recipients: 'admin@somewhere'])
}

	stage('teatA') {
      steps {
        catchError() {
          sh 'make'
        }

        mail(subject: 'Test', body: 'aaaa', to: 'netkiller@msn.com')
      }
    }

```

##### 3.5.4. 睡眠

```

node {
    sleep 10
    echo 'Hello World'
}			

```

```

sleep(time:3,unit:"SECONDS")			

```

##### 3.5.5. 限制执行时间

```

		stage('enforce') {
          steps {
            timeout(activity: true, time: 1) {
              echo 'test'
            }

          }
        }

```

##### 3.5.6. 时间截

```

	stage('timestamps') {
          steps {
            timestamps() {
              echo 'test'
            }
          }
    }			

```

#### 3.6. 版本控制

##### 3.6.1. checkout

https://github.com/jenkinsci/workflow-scm-step-plugin/blob/master/README.md

下面配置适用与 Webhook 方式

```

	stage('checkout') {
      steps {
        checkout(scm: [$class: 'GitSCM', branches: [[name: env.GIT_BUILD_REF]], 
                          userRemoteConfigs: [[url: env.GIT_REPO_URL]]], changelog: true, poll: true)
      }
    }			

```

从 URL 获取代码

```

node {
   checkout ([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: '', url: 'https://github.com/bg7nyt/java.git']]])   
}			

```

##### 3.6.2. Git

```

		stage('Git') {
          	steps {
            	git(url: 'https://git.dev.tencent.com/netkiller/java.git', branch: 'master', changelog: true, poll: true)
          	}
    	}

```

#### 3.7. 节点与过程

##### 3.7.1. sh

```

		stage("build") {
            steps {
                sh "mvn package -Dmaven.test.skip=true"
            }
        }

```

```

		steps {
        	script{
            	sh 'find /etc/'
			}
		}

```

例 175.1. Shell Docker 示例

Shell Docker 使用 docker 命令完成构建过程

```

registryUrl='127.0.0.1:5000'	    # 私有镜像仓库地址
imageName='netkiller/project'	# 镜像名称
imageTag=$BRANCH	            # 上面设置 Branch 分支，这里可以当做环境变量使用

echo ' >>> [INFO] enter workspace ...'

cd $WORKSPACE/				# 进入到 jenkins 的工作区，jenkins 会将 gitlab 仓库代码 pull 到这里，用于制作镜像文件

# 根据不同的 Branch 生成不同的 swoft 的配置文件，区分测试还是生成等 
echo ' >>> [INFO] create startup.sh ...'
(
cat << EOF

# 启动 Shell 写在此处

EOF
) > ./entrypoint.sh

# 生成 Dockerfile
echo ' >>> [INFO] begin create Dockerfile ...'
rm -f ./Dockerfile
(
cat << EOF
FROM netkiller/base
LABEL maintainer=netkiller@msn.com

COPY . /var/www/project
WORKDIR /var/www/project
EXPOSE 80
ENV PHP_ENVIRONMENT $BRANCH
ENTRYPOINT [ "bash", "/var/www/project/entrypoint.sh" ]
EOF
) > ./Dockerfile

#删除无用镜像
echo ' >>> [INFO] begin cleaning image ...'
for image in `docker images | grep -w $imageName | grep -i -w $imageTag | awk '{print $3}'`
do
    docker rmi $image -f
done

#制作镜像
echo ' >>> [INFO] begin building image ...'
docker build --tag $imageName:$imageTag --rm .

#给镜像打标签
img=`docker images | grep -w $imageName | grep -i -w $imageTag | awk '{print $3}'`
docker tag $img $registryUrl/$imageName:$imageTag

#push 到私有镜像仓库
echo ' >>> [INFO] begin publishing image ...'
docker push $registryUrl/$imageName:$imageTag

#删除刚刚制作的镜像, 释放存储空间
echo ' >>> [INFO] cleaning temporary building ...'
docker rmi -f $imageName:$imageTag
docker rmi -f $registryUrl/$imageName:$imageTag	

```

##### 3.7.2. Windows 批处理脚本

```

	stage('bat') {
        steps {
            bat(returnStatus: true, returnStdout: true, label: 'aa', encoding: 'utf-8', script: 'dir')
    	}
    }			

```

##### 3.7.3. 分配工作空间

```

	stage('alocate') {
        steps {
            ws(dir: 'src') {
                echo 'aaa'
            }

    	}
    }

```

##### 3.7.4. node

```

	stage('node') {
    	steps {
            node(label: 'java-8') {
              sh 'mvn package'
            }

        }
	}				

```

#### 3.8. 工作区

##### 3.8.1. 变更目录

```

	stage('subtask') {
        steps {
            dir(path: '/src') {
             	echo 'begin'
              	sh '''mvn test'''
            	echo 'end'
        	}
		}
	}

```

##### 3.8.2. 判断文件是否存在

```

	stage('exists') {
          steps {
            fileExists '/sss'
          }
    }	

```

```

def exists = fileExists 'file'

if (exists) {
    echo 'Yes'
} else {
    echo 'No'
}

```

```

if (fileExists('file')) {
    echo 'Yes'
} else {
    echo 'No'
}			

```

```

pipeline{
	agent any
	stages{
      stage("Checkout") {
            steps {
                checkout(
                  [$class: 'GitSCM', branches: [[name: env.GIT_BUILD_REF]], 
                  userRemoteConfigs: [[url: env.GIT_REPO_URL]]]
                )
            }
        }

		stage("fileExists") {
			steps{
              	echo pwd()
              	sh 'ls -1'
				script {
					def exists = fileExists 'README.md'
                    if (exists) {
                        echo 'Yes'
                    } else {
                        echo 'No'
                    }
				}
			}
		}
	}
}			

```

##### 3.8.3. 分配工作区

```

    stage('alocate') {
        steps {
            ws(dir: 'src') {
                echo 'aaa'
            }

    	}
    }			

```

##### 3.8.4. 清理工作区

```

	stage('test') {
        steps {
            cleanWs(cleanWhenAborted: true, cleanWhenFailure: true, cleanWhenNotBuilt: true, cleanWhenSuccess: true, cleanWhenUnstable: true, cleanupMatrixParent: true, deleteDirs: true, disableDeferredWipeout: true, notFailBuild: true, skipWhenFailed: true, externalDelete: '/aa')
    	}
    }			

```

##### 3.8.5. 递归删除目录

```

	stage('deldir') {
          steps {
            deleteDir()
          }
    }			

```

##### 3.8.6. 写文件

```

		stage('write') {
          steps {
            writeFile(file: 'hello.txt', text: 'Helloworld')
          }
        }

```

##### 3.8.7. 读文件

```

        stage('read') {
          steps {
            readFile 'hello.txt'
          }
        }				

```

### 4. Jenkins Job DSL / Plugin

```

def gitUrl = 'git://github.com/jenkinsci/job-dsl-plugin.git'

job('PROJ-unit-tests') {
    scm {
        git(gitUrl)
    }
    triggers {
        scm('*/15 * * * *')
    }
    steps {
        maven('-e clean test')
    }
}

job('PROJ-sonar') {
    scm {
        git(gitUrl)
    }
    triggers {
        cron('15 13 * * *')
    }
    steps {
        maven('sonar:sonar')
    }
}

job('PROJ-integration-tests') {
    scm {
        git(gitUrl)
    }
    triggers {
        cron('15 1,13 * * *')
    }
    steps {
        maven('-e clean integration-test')
    }
}

job('PROJ-release') {
    scm {
        git(gitUrl)
    }
    // no trigger
    authorization {
        // limit builds to just Jack and Jill
        permission('hudson.model.Item.Build', 'jill')
        permission('hudson.model.Item.Build', 'jack')
    }
    steps {
        maven('-B release:prepare release:perform')
        shell('cleanup.sh')
    }
}			

```

```

job('PROJ-unit-tests') {
    scm {
        git('https://github.com/bg7nyt/java.git')
    }
    triggers {
        scm('*/15 * * * *')
    }
    steps {
        maven('-e clean test')
    }
}	

```

### 5. Jenkins Plugin

#### 5.1. Blue Ocean

[`jenkins.io/doc/book/blueocean/getting-started/`](https://jenkins.io/doc/book/blueocean/getting-started/)

[`jk.netkiller.cn/blue/`](http://jk.netkiller.cn/blue/)

#### 5.2. Locale Plugin (国际化插件)

安装 Locale Plugin, 重启生效。

```

配置【Manage Jenkins】>【Configure System】> 【Locale】		

```

| ![](img/locale.png) |

Default Language 填写 zh_CN，勾选忽略浏览器设置强制设置语言

#### 5.3. github-plugin 插件

[`github.com/jenkinsci/github-plugin`](https://github.com/jenkinsci/github-plugin)

```

git clone https://github.com/jenkinsci/github-plugin.git
mkdir target/classes

```

修改 rest-assured 去掉 exclusions 配置项

```

        <dependency>
            <groupId>com.jayway.restassured</groupId>
            <artifactId>rest-assured</artifactId>
            <!--1.7.2 is the last version that use a compatible groovy version-->
            <version>1.7.2</version>
            <scope>test</scope>
            <exclusions>
                <exclusion>
                    <groupId>org.apache.httpcomponents</groupId>
                    <artifactId>*</artifactId>
                </exclusion>
            </exclusions>
        </dependency>		

```

编译插件

```

[root@netkiller github-plugin]# mvn hpi:hpi
[INFO] Scanning for projects...
[INFO]                                                                         
[INFO] ------------------------------------------------------------------------
[INFO] Building GitHub plugin 1.29.4-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] 
[INFO] --- maven-hpi-plugin:1.120:hpi (default-cli) @ github ---
[INFO] Generating /srv/github-plugin/target/github/META-INF/MANIFEST.MF
[INFO] Checking for attached .jar artifact ...
[INFO] Generating jar /srv/github-plugin/target/github.jar
[INFO] Building jar: /srv/github-plugin/target/github.jar
[INFO] Exploding webapp...
[INFO] Copy webapp webResources to /srv/github-plugin/target/github
[INFO] Assembling webapp github in /srv/github-plugin/target/github
[INFO] Generating hpi /srv/github-plugin/target/github.hpi
[INFO] Building jar: /srv/github-plugin/target/github.hpi
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time: 4.161s
[INFO] Finished at: Mon Jan 07 12:03:45 CST 2019
[INFO] Final Memory: 29M/290M
[INFO] ------------------------------------------------------------------------

```

进入 github --> Settings --> Developer settings --> Personal Access Token --> Generate new token

repo 和 admin:repo_hook

Settings -> Webhooks -> Add webhook

系统管理 --> 系统设置 --> GitHub --> Add GitHub Sever

#### 5.4. Docker

##### This plugin integrates Jenkins with Docker

[`jenkins.io/doc/book/pipeline/docker/`](https://jenkins.io/doc/book/pipeline/docker/)

```

vim /lib/systemd/system/docker.service

ExecStart=/usr/bin/dockerd -H fd://

改为

ExecStart=/usr/bin/dockerd -H fd:// -H unix:///var/run/docker.sock -H tcp://0.0.0.0:2375

```

吧 jenkins 用户添加到 docker 组

```

gpasswd -a jenkins docker		

```

重启 docker

```

systemctl daemon-reload
systemctl restart docker

如果是 Docker 方式运行 Jenkins 需要启动 jenkins
docker start jenkins	

```

参考例子

```

root@ubuntu:~# cat /lib/systemd/system/docker.service
[Unit]
Description=Docker Application Container Engine
Documentation=https://docs.docker.com
After=network-online.target docker.socket firewalld.service
Wants=network-online.target
Requires=docker.socket

[Service]
Type=notify
# the default is not to use systemd for cgroups because the delegate issues still
# exists and systemd currently does not support the cgroup feature set required
# for containers run by docker
ExecStart=/usr/bin/dockerd -H fd:// -H unix:///var/run/docker.sock -H tcp://0.0.0.0:2375
ExecReload=/bin/kill -s HUP $MAINPID
LimitNOFILE=1048576
# Having non-zero Limit*s causes performance problems due to accounting overhead
# in the kernel. We recommend using cgroups to do container-local accounting.
LimitNPROC=infinity
LimitCORE=infinity
# Uncomment TasksMax if your systemd version supports it.
# Only systemd 226 and above support this version.
TasksMax=infinity
TimeoutStartSec=0
# set delegate yes so that systemd does not reset the cgroups of docker containers
Delegate=yes
# kill only the docker process, not all processes in the cgroup
KillMode=process
# restart the docker process if it exits prematurely
Restart=on-failure
StartLimitBurst=3
StartLimitInterval=60s

[Install]
WantedBy=multi-user.target	

```

##### 5.4.1. 设置 Docker 主机和代理

| ![](img/docker-add.png) |

输入 Docker 主机的 IP 地址，类似 tcp://172.16.0.10:2375

| ![](img/docker-host.png) |

| ![](img/docker-name.png) |

| ![](img/docker-test.png) |

| ![](img/docker-agent.png) |

##### 5.4.2. 持久化

例如持续集成过程中，我们不希望每次都从 maven 镜像下载编译依赖的包，或者构建物我们需要永久保留等等，这时就需要做持久化

| ![](img/container.png) |

例如我们将宿主主机的 /opt/maven 挂载到 Docker 容器的 /root/.m2 目录。这样就实现了 maven 的持久化。只需写入 /opt/maven:/root/.m2 即可

| ![](img/volumes.png) |

当持续机构运行完毕 docker 容器被清理，但是 /opt/maven 并不会被清理，下次构建时，在将它挂载到 /root/.m2 即可。

#### 5.5. JaCoCo

##### This plugin integrates JaCoCo code coverage reports to Jenkins.

https://jenkins.io/doc/pipeline/steps/jacoco/

##### 5.5.1. Pipeline

```

stage('Build') {
     steps {
        sh 'mvn test'
        junit '*/build/test-results/*.xml'
        step( [ $class: 'JacocoPublisher' ] )
     }
}

```

配置 jacoco

```

The jacoco pipeline step configuration uses this format:

step([$class: 'JacocoPublisher', 
      execPattern: 'target/*.exec',
      classPattern: 'target/classes',
      sourcePattern: 'src/main/java',
      exclusionPattern: 'src/test*'
])
Or with a simpler syntax for declarative pipeline:

jacoco( 
      execPattern: 'target/*.exec',
      classPattern: 'target/classes',
      sourcePattern: 'src/main/java',
      exclusionPattern: 'src/test*'
)			

```

完整的例子

```

node {
    stage('Checkout') {
        git 'https://github.com/bg7nyt/junit4-jacoco.git'
    }
    stage('Build') {
        sh "mvn -Dmaven.test.failure.ignore clean package"
    }
    stage('Test') {
        sh "mvn test"
    }
    stage('Results') {
        junit '**/target/surefire-reports/TEST-*.xml'
        archive 'target/*.jar'
        step( [ $class: 'JacocoPublisher' ] )
    }
}			

```

#### 5.6. SSH Pipeline Steps

使用说明: https://github.com/jenkinsci/ssh-steps-plugin#pipeline-steps

```

!groovy
def getHost(){
    def remote = [:]
    remote.name = 'mysql'
    remote.host = '192.168.8.108'
    remote.user = 'root'
    remote.port = 22
    remote.password = 'qweasd'
    remote.allowAnyHosts = true
    return remote
}
pipeline {
    agent {label 'master'}
    environment{
        def server = ''
    }   
    stages {
        stage('init-server'){
            steps {
                script {                 
                   server = getHost()                                   
                }
            }
        }
        stage('use'){
            steps {
                script {
                  sshCommand remote: server, command: """                 
                  if test ! -d aaa/ccc ;then mkdir -p aaa/ccc;fi;cd aaa/ccc;rm -rf ./*;echo 'aa' > aa.log
                  """
                }
            }
        }
    }
}
####################################
node {
  def remote = [:]
  remote.name = 'test'
  remote.host = 'test.domain.com'
  remote.user = 'root'
  remote.password = 'password'
  remote.allowAnyHosts = true
  stage('Remote SSH') {
    sshCommand remote: remote, command: "ls -lrt"
    sshCommand remote: remote, command: "for i in {1..5}; do echo -n \"Loop \$i \"; date ; sleep 1; done"
  }
}
node {
  def remote = [:]
  remote.name = 'test'
  remote.host = 'test.domain.com'
  remote.user = 'root'
  remote.password = 'password'
  remote.allowAnyHosts = true
  stage('Remote SSH') {
    writeFile file: 'abc.sh', text: 'ls -lrt'
    sshScript remote: remote, script: "abc.sh"
  }
}
node {
  def remote = [:]
  remote.name = 'test'
  remote.host = 'test.domain.com'
  remote.user = 'root'
  remote.password = 'password'
  remote.allowAnyHosts = true
  stage('Remote SSH') {
    writeFile file: 'abc.sh', text: 'ls -lrt'
    sshPut remote: remote, from: 'abc.sh', into: '.'
  }
}
node {
  def remote = [:]
  remote.name = 'test'
  remote.host = 'test.domain.com'
  remote.user = 'root'
  remote.password = 'password'
  remote.allowAnyHosts = true
  stage('Remote SSH') {
    sshGet remote: remote, from: 'abc.sh', into: 'abc_get.sh', override: true
  }
}
node {
  def remote = [:]
  remote.name = 'test'
  remote.host = 'test.domain.com'
  remote.user = 'root'
  remote.password = 'password'
  remote.allowAnyHosts = true
  stage('Remote SSH') {
    sshRemove remote: remote, path: "abc.sh"
  }
}
def remote = [:]
remote.name = "node-1"
remote.host = "10.000.000.153"
remote.allowAnyHosts = true
node {
    withCredentials([sshUserPrivateKey(credentialsId: 'sshUser', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
        remote.user = userName
        remote.identityFile = identity
        stage("SSH Steps Rocks!") {
            writeFile file: 'abc.sh', text: 'ls'
            sshCommand remote: remote, command: 'for i in {1..5}; do echo -n \"Loop \$i \"; date ; sleep 1; done'
            sshPut remote: remote, from: 'abc.sh', into: '.'
            sshGet remote: remote, from: 'abc.sh', into: 'bac.sh', override: true
            sshScript remote: remote, script: 'abc.sh'
            sshRemove remote: remote, path: 'abc.sh'
        }
    }
}
################################		

```

#### 5.7. Rancher

[`plugins.jenkins.io/rancher`](https://plugins.jenkins.io/rancher)

[`jenkins.io/doc/pipeline/steps/rancher/`](https://jenkins.io/doc/pipeline/steps/rancher/)

创建 Rancher API

在 Jenkins 的 Credentials 中添加一个类型为 Username with password 的认证，username 和 password 分别对应于上一步生成的 Access Key 和 Secret Key，如下图

然后在语法生成器中，找到 rancher 进行如下图的配置：

设置 environmentId ，找到你的集群，点击进入，看到 URL https://rancher.netkiller.cn/v3/clusters/c-mx88f，“c-mx88f” 就是 environmentId

```

stage('Rancher') {
    rancher confirm: false, credentialId: 'b56bd9b2-3277-4072-baae-08d73aa26549', endpoint: 'https://rancher.netkiller.cn/v2', environmentId: 'test', environments: '', image: '*/demo:1.0.0', ports: '', service: 'jenkins/demo', timeout: 50
}		

```

#### 5.8. Kubernetes 插件

##### 5.8.1. Kubernetes

[`plugins.jenkins.io/kubernetes`](https://plugins.jenkins.io/kubernetes)

[`github.com/jenkinsci/kubernetes-plugin`](https://github.com/jenkinsci/kubernetes-plugin)

```

def label = "mypod-${UUID.randomUUID().toString()}"
podTemplate(label: label) {
    node(label) {
        stage('Run shell') {
            sh 'echo hello world'
        }
    }
}			

```

##### 5.8.2. Kubernetes :: Pipeline :: Kubernetes Steps

##### 5.8.3. Kubernetes Continuous Deploy

[`plugins.jenkins.io/kubernetes-cd`](https://plugins.jenkins.io/kubernetes-cd)

##### 5.8.4. Kubernetes Cli

:

#### 5.9. HTTP Request Plugin

```

GET https://<rancher_server>/v3/project/<project_id>/workloads/deployment:<rancher_namespace>:<rancher_service> # 获取一个服务的详细信息
GET https://<rancher_server>/v3/project/<project_id>/pods/?workloadId=deployment:<rancher_namespace>:<rancher_service> # 获取服务的所有容器信息
DELETE https://<rancher_server>/v3/project/<project_id>/pods/<rancher_namespace>:<container_name> # 根据容器名删除容器
PUT https://<rancher_server>/v3/project/<project_id>/workloads/deployment:<rancher_namespace>:<rancher_service> # 更新服务	

```

```

// 查询服务信息
def response = httpRequest acceptType: 'APPLICATION_JSON', authentication: "${RANCHER_API_KEY}", contentType: 'APPLICATION_JSON', httpMode: 'GET', responseHandle: 'LEAVE_OPEN', timeout: 10, url: "${rancherUrl}/workloads/deployment:${rancherNamespace}:${rancherService}"
def serviceInfo = new JsonSlurperClassic().parseText(response.content)
response.close()

def dockerImage = imageName+":"+imageTag
if (dockerImage.equals(serviceInfo.containers[0].image)) {
    // 如果镜像名未改变，直接删除原容器
    // 查询容器名称
    response = httpRequest acceptType: 'APPLICATION_JSON', authentication: "${RANCHER_API_KEY}", contentType: 'APPLICATION_JSON', httpMode: 'GET', responseHandle: 'LEAVE_OPEN', timeout: 10, url: "${rancherUrl}/pods/?workloadId=deployment:${rancherNamespace}:${rancherService}"
    def podsInfo = new JsonSlurperClassic().parseText(response.content)
    def containerName = podsInfo.data[0].name
    response.close()
    // 删除容器
    httpRequest acceptType: 'APPLICATION_JSON', authentication: "${RANCHER_API_KEY}", contentType: 'APPLICATION_JSON', httpMode: 'DELETE', responseHandle: 'NONE', timeout: 10, url: "${rancherUrl}/pods/${rancherNamespace}:${containerName}"

} else {
    // 如果镜像名改变，使用新镜像名更新容器
    serviceInfo.containers[0].image = dockerImage
    // 更新
    def updateJson = new JsonOutput().toJson(serviceInfo)
    httpRequest acceptType: 'APPLICATION_JSON', authentication: "${RANCHER_API_KEY}", contentType: 'APPLICATION_JSON', httpMode: 'PUT', requestBody: "${updateJson}", responseHandle: 'NONE', timeout: 10, url: "${rancherUrl}/workloads/deployment:${rancherNamespace}:${rancherService}"
}		

```

#### 5.10. Skip Certificate Check plugin

[`wiki.jenkins.io/display/JENKINS/Skip+Certificate+Check+plugin`](https://wiki.jenkins.io/display/JENKINS/Skip+Certificate+Check+plugin)

```

[root@localhost ~]# tail -f /var/log/jenkins/jenkins.log

javax.net.ssl.SSLPeerUnverifiedException: peer not authenticated

```

#### 5.11. Android Sign Plugin

Android Sign Plugin 依赖 Credentials Plugin，因为 Credentials Plugin 只支持 PKCS#12 格式的证书，所以先需要将生成好的 JKS 证书转换为 PKCS#12 格式：

```

keytool -importkeystore -srckeystore netkiller.jks -srcstoretype JKS -deststoretype PKCS12 -destkeystore netkiller.p12

```

复制代码添加类型为 credential，选择上传证书文件，将 PKCS#12 证书上传到并配置好 ID，本项目中使用了 ANDROID_SIGN_KEY_STORE 作为 ID。

```

pipeline {
    ...

    stages {
        ...

        stage("Sign APK") {
            steps {
                echo 'Sign APK'
                signAndroidApks(
                    keyStoreId: "ANDROID_SIGN_KEY_STORE",
                    keyAlias: "tomczhen",
                    apksToSign: "**/*-prod-release-unsigned.apk",
                    archiveSignedApks: false,
                    archiveUnsignedApks: false
                )
            }
        }

        ...
    }

    ...
}		

```

### 6. Jenkinsfile Pipeline Example

#### 6.1. Maven 子模块范例

Maven 子模块创建方法 [`www.netkiller.cn/java/build/maven.html#maven.module`](https://www.netkiller.cn/java/build/maven.html#maven.module)

目录结构

```

Project
    |
    |--- common (Shared)
    |     | ---pom.xml
    |--- project1 (depend common)
    |     |--- pom.xml
    |--- project2 (depend common)
    |     |--- pom.xml
    |---pom.xml	

```

构建 父项目

```

pipeline {
    agent {
        label "default"
    }
    stages  {

        stage("检出") {
            steps {
                checkout(
                  [$class: 'GitSCM', branches: [[name: env.GIT_BUILD_REF]], 
                  userRemoteConfigs: [[url: env.GIT_REPO_URL]]]
                )
            }
        }

        stage("构建") {
            steps {
                echo "构建中..."
              	sh 'mvn package -Dmaven.test.skip=true' // mvn 示例
                archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true // 收集构建产物              	
                echo "构建完成."
            }
        }

        stage("测试") {
            steps {
                echo "单元测试中..."
                // 请在这里放置您项目代码的单元测试调用过程，例如:
                sh 'mvn test' // mvn 示例
                echo "单元测试完成."
                junit '**/target/surefire-reports/*.xml' // 收集单元测试报告的调用过程
            }
        }

        stage("部署") {
            steps {
                echo "部署中..."
                echo "部署完成"
            }
        }
    }
}

```

构建共享项目

```

pipeline {
    agent {
        label "default"
    }
    stages  {

        stage("检出") {
            steps {
                checkout(
                  [$class: 'GitSCM', branches: [[name: env.GIT_BUILD_REF]], 
                  userRemoteConfigs: [[url: env.GIT_REPO_URL]]]
                )
            }
        }

        stage("构建") {
            steps {
                echo "构建中..."
                dir(path: 'common') {
              		sh 'mvn package -Dmaven.test.skip=true' // mvn 示例
              		archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true // 收集构建产物
              	}
                echo "构建完成."
            }
        }

        stage("测试") {
            steps {
                echo "单元测试中..."
                sh 'mvn test' // mvn 示例
                echo "单元测试完成."
                junit 'target/surefire-reports/*.xml' // 收集单元测试报告的调用过程
            }
        }

        stage("部署") {
            steps {
                echo "部署中..."
                dir(path: 'common') {
                	sh 'mvn install'
                }
                echo "部署完成"
            }
        }
    }
}			

```

构建 project1 和 project2

```

pipeline {
    agent {
        label "default"
    }
    stages  {

        stage("检出") {
            steps {
                checkout(
                  [$class: 'GitSCM', branches: [[name: env.GIT_BUILD_REF]], 
                  userRemoteConfigs: [[url: env.GIT_REPO_URL]]]
                )
            }
        }
		stage("共享库") {
            steps {
                echo "构建中..."
                dir(path: 'common') {
              		sh 'mvn install -Dmaven.test.skip=true' // mvn 示例
              		archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true // 收集构建产物
              	}
                echo "构建完成."
            }
        }
        stage("构建") {
            steps {
                echo "构建中..."
	            dir(path: 'project1') {
    	            sh 'mvn package -Dmaven.test.skip=true' // mvn 示例
   		            archiveArtifacts artifacts: '**/target/*.jar', fingerprint: true // 收集构建产物
                }
                echo "构建完成."

            }
        }

        stage("测试") {
            steps {
                echo "单元测试中..."
                sh 'mvn test' // mvn 示例
                echo "单元测试完成."
                junit 'target/surefire-reports/*.xml' // 收集单元测试报告的调用过程
            }
        }

        stage("部署") {
            steps {
                echo "部署中..."
                // 部署脚本
                echo "部署完成"
            }
        }
    }
}			

```

#### 6.2. 使用指定镜像构建

```

pipeline {
    agent any
    stages {
        stage("Checkout") {
            steps {
                sh 'ci-init'
                checkout(
                        [$class           : 'GitSCM', branches: [[name: env.GIT_BUILD_REF]],
                         userRemoteConfigs: [[url: env.GIT_REPO_URL]]]
                )
            }
        }

        stage("Compile") {

            // 构建的 docker 镜像
            agent {
                docker { image 'maven' }
            }

            steps {
                echo "构建中..."
                sh 'mvn -v'
                sh 'mvn compile'
            }
        }

        stage('Test') {

            agent {
                docker { image 'maven' }
            }

            steps {
                echo '单元测试...'
                sh 'mvn test'
                junit 'target/surefire-reports/*.xml'
            }

        }

        stage("Deploy") {
            steps {
                echo "部署中..."
                echo "部署完成"
            }
        }
    }
}		

```

#### 6.3. 命令行制作 Docker 镜像

```

pipeline {
    agent any
    stages {

        stage('Build') {

            steps {
                echo '编译中...'
                // 编译 docker 镜像
                sh "docker build $tag $contextPath"
            }

        }

        stage('Push Image') {

            steps {

                sh "echo ${REGISTRY_PASS} | docker login -u ${REGISTRY_USER} --password-stdin ${REGISTRY_URL}"
                sh "docker tag ${image} ${registry_image}"
                sh "docker push ${registry_image}"

            }

        }

    }
}		

```

```

pipeline {

    agent any

    stages {
        stage("Checkout") {
          steps {
            checkout([
              $class: 'GitSCM',
              branches: [[name: env.GIT_COMMIT]],
              extensions: [[$class: 'PruneStaleBranch']],
              userRemoteConfigs: [[
                url: env.GIT_REPO_URL,
                refspec: "+refs/heads/*:refs/remotes/origin/*"
              ]]
            ])

            sh '''
                #!/bin/bash
                echo ${GIT_COMMIT}
                echo ${REF}
                echo ${GIT_LOCAL_BRANCH}
            '''
          }
        }

        stage('Build') {
            steps{
                echo "Building begin"
                script{
                    // 设置镜像名                
                    env.BUILD_MODULE = "common"
                    env.DOCKER_IMAGE_TAG = env.BUILD_MODULE + ':' + env.GIT_COMMIT
                    env.DOCKER_REMOTE_IMAGE_TAG = "${env.REGISTRY_URL}/${env.DOCKER_IMAGE_TAG}"

                    sh "docker login ${DOCKER_REGISTER_URL} -u ${DOCKER_REPOSITORY_USERNAME} -p ${DOCKER_REPOSITORY_PASSWORD}"

                    def statusCode = sh(script:"docker pull ${DOCKER_REMOTE_IMAGE_TAG}", returnStatus:true)

					// 判断该镜像在仓库是否存在
                    if (statusCode != 0) {

                        sh '''
                        #!/bin/bash

                        # build docker image
                        docker build . -f Dockerfile -t ${DOCKER_IMAGE_TAG}

                        # tag docker image
                        docker tag ${DOCKER_IMAGE_TAG} ${DOCKER_REMOTE_IMAGE_TAG}

                    }
                }
                echo "Build end"
            }
        }

        stage('Deploy') {
            steps{
                echo "Deploying begin"
                script{
                        # push to
                        docker push ${DOCKER_REMOTE_IMAGE_TAG}

                        # rm
                        docker rmi ${DOCKER_IMAGE_TAG}
                        docker rmi ${DOCKER_REMOTE_IMAGE_TAG}
                        '''
                }
                echo "Deploy end"
            }
        }
    }
}		

```

#### 6.4. Yarn

```

pipeline {
    agent {
        label "default"
    }
    stages  {

        stage("检出") {
            steps {
                checkout(
                  [$class: 'GitSCM', branches: [[name: env.GIT_BUILD_REF]], 
                  userRemoteConfigs: [[url: env.GIT_REPO_URL]]]
                )
            }
        }
		stage("环境") {
          	steps {
              	sh 'apt install -y apt-transport-https'
              	sh "curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -"
              	sh 'echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list'
              	sh 'cat /etc/apt/sources.list.d/yarn.list'
              	sh 'apt update && apt install -y yarn'
              	sh 'yarn --version'
            }
        }
        stage("构建") {
            steps {
                echo "构建中..."
                sh 'yarn add webpack'
                sh 'node -v'
            }
        }

        stage("测试") {
            steps {
                echo "单元测试中..."
            }
        }

        stage("部署") {
            steps {
                // sh './deploy.sh'
            }
        }
    }
}		

```

#### 6.5. Android

进入项目目录，找到 local.properties 文件，打开文件

```

## This file is automatically generated by Android Studio.
# Do not modify this file -- YOUR CHANGES WILL BE ERASED!
#
# This file should *NOT* be checked into Version Control Systems,
# as it contains information specific to your local configuration.
#
# Location of the SDK. This is only used by Gradle.
# For customization when using a Version Control System, please read the
# header note.
sdk.dir=/Users/neo/Library/Android/sdk		

```

sdk.dir 是 Android SDK 存放目录，进入该目录

```

neo@MacBook-Pro ~ % ll /Users/neo/Library/Android/sdk/        
total 0
drwxr-xr-x   3 neo  staff    96B Oct 23 09:56 build-tools
drwxr-xr-x  18 neo  staff   576B Oct 23 09:55 emulator
drwxr-xr-x   6 neo  staff   192B Oct 23 10:21 extras
drwxr-xr-x   3 neo  staff    96B Oct 23 11:35 fonts
drwxr-xr-x   4 neo  staff   128B Oct 23 11:00 licenses
drwxr-xr-x   3 neo  staff    96B Oct 23 09:55 patcher
drwxr-xr-x  19 neo  staff   608B Oct 23 09:56 platform-tools
drwxr-xr-x   4 neo  staff   128B Oct 23 10:23 platforms
drwxr-xr-x  24 neo  staff   768B Oct 23 10:57 skins
drwxr-xr-x   4 neo  staff   128B Oct 23 10:23 sources
drwxr-xr-x   4 neo  staff   128B Oct 24 15:06 system-images
drwxr-xr-x  14 neo  staff   448B Oct 23 09:55 tools

neo@MacBook-Pro ~ % ll /Users/neo/Library/Android/sdk/licenses
total 16
-rw-r--r--  1 neo  staff    41B Oct 23 10:23 android-sdk-license
-rw-r--r--  1 neo  staff    41B Oct 23 11:00 android-sdk-preview-license

neo@MacBook-Pro ~ % cat /Users/neo/Library/Android/sdk/licenses/android-sdk-license 

d56f5187479451eabf01fb78af6dfcb131a6481e	

```

/Users/neo/Library/Android/sdk/licenses/android-sdk-license 便是当前 Android SDK License 文件

如果你安装了多个版本的 SDK，例如 android-26， android-27， android-28 可以看到三行字串。

```

24333f8a63b6825ea9c5514f83c2829b004d1fee 这是 Android 8.0 - android-26
d56f5187479451eabf01fb78af6dfcb131a6481e 这是 Android 9.0 - android-28

```

```

pipeline {
  	agent any
    stages  {

        stage("Checkout") {
            steps {
                checkout(
                  [$class: 'GitSCM', branches: [[name: env.GIT_BUILD_REF]], 
                  userRemoteConfigs: [[url: env.GIT_REPO_URL]]]
                )
            }
        }

      	stage("Android SDK") {
            steps {
              	script{
	                if (fileExists('sdk-tools-linux-4333796.zip')) {
	    				echo 'Android SDK 已安装'
					} else {
	    				echo '安装 Android SDK'

                		sh '''
# rm -rf sdk-tools-linux-4333796.* tools platforms platform-tools
wget https://dl.google.com/android/repository/sdk-tools-linux-4333796.zip
unzip sdk-tools-linux-4333796.zip
         				'''

              			sh 'yes|tools/bin/sdkmanager --licenses'
              			//sh 'yes|tools/bin/sdkmanager "platform-tools" "build-tools;26.0.3" "platforms;android-26"'	// andorid 8.0	
	              		//sh 'yes|tools/bin/sdkmanager "platform-tools" "platforms;android-27"' // andorid 8.1
	              		sh 'yes|tools/bin/sdkmanager "platform-tools" "platforms;android-28"'	// andorid 9.0
	              		sh '(while sleep 3; do echo "y"; done) | tools/android update sdk -u'

	              		sh 'tools/bin/sdkmanager --list'
                	}
               	}	
              	echo '安装 Android SDK License'
              	writeFile(file: 'platforms/licenses/android-sdk-license', text: '''
8933bad161af4178b1185d1a37fbf41ea5269c55
24333f8a63b6825ea9c5514f83c2829b004d1fee
d56f5187479451eabf01fb78af6dfcb131a6481e
 					''')
				sh 'ls -1 platforms'
            }
        }

        stage("Build") {
            steps {
              	echo "构建中..."
                sh './gradlew'
              	echo "构建完成."
            }
        }
        stage("Test") {
            steps {
                echo "单元测试中..."
                sh './gradlew test'
                echo "单元测试完成."
                //junit 'app/build/test-results/*/*.xml'
            }
        }      
      	stage("Package") {
            steps {
                sh './gradlew assemble'
                // 收集构建产物
                archiveArtifacts artifacts: 'app/build/outputs/apk/*/*.apk', fingerprint: true 
            }
        }

        stage("Deploy") {
            steps {
                echo "部署中..."
                // sh './deploy.sh' // 自研部署脚本
                echo "部署完成"
            }
        }
    }
}		

```

## 第 176 章 SonarQube

[`www.sonarqube.org`](https://www.sonarqube.org)

## 第 177 章 Phabricator - an open source, software engineering platform

### *Phabricator is a collection of open source web applications that help software companies build better software.*

Phabricator 是 Facebook 开源的 Code Review 工具

## 第 178 章 Gerrit

Gerrit 是 Google 开发的 Code Review 工具

## 第 179 章 TeamCity