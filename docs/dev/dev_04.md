# 部分 II. Ruby

## 第 17 章 Ruby

```
sudo apt-get install ruby1.9.1

```

## rubygems

```
wget http://production.cf.rubygems.org/rubygems/rubygems-1.6.2.tgz
tar zxvf rubygems-1.6.2.tgz
cd rubygems-1.6.2/
sudo  ruby setup.rb
# gem1.8

```

ubuntu

```
$ sudo apt-get install ruby1.9.1
$ sudo apt-get install rubygems1.8

```

## 第 18 章 Variable

## String

## Date

使用 strftime 获得具体的年份，月份或日期等等，常用：

```

CODE	OUTPUT	DESCRIPTION
t.strftime("%H")	=> "22"	# Gives Hour of the time in 24 hour clock format
t.strftime("%I")	=> "10"	# Gives Hour of the time in 12 hour clock format
t.strftime("%M")	=> "49"	# Gives Minutes of the time
t.strftime("%S")	=> "27"	# Gives Seconds of the time
t.strftime("%Y")	=> "2013"	# Gives Year of the time
t.strftime("%m")	=> "09"	# Gives month of the time
t.strftime("%d")	=> "12"	# Gives day of month of the time
t.strftime("%w")	=> "4"	# Gives day of week of the time
t.strftime("%a")	=> "Thu"	# Gives name of week day in short form of the
t.strftime("%A")	=> "Thursday"	# Gives week day in full form of the time
t.strftime("%b")	=> "Sep"	# Gives month in short form of the time
t.strftime("%B")	=> "September"	# Gives month in full form of the time
t.strftime("%y")	=> "13"	# Gives year without century of the time
t.strftime("%Y")	=> "2013"	# Gives year without century  of the time
t.strftime("%Z")	=> "IST"	# Gives Time Zone of the time
t.strftime("%p")	=> "PM"	# Gives AM / PM of the time		

```

```

# 2012-03-06 15:28:08
puts Time.now.strftime("%Y-%m-%d %H:%M:%S")

# 03/06/12 03:39 PM
puts Time.now.strftime("%x %I:%M %p") 		

```

## Array

## 第 19 章 Class

## rmagick

```
sudo gem install rmagick

```

## 第 20 章 Ruby on Rails

## Getting Started

```
$ sudo apt-get install ruby1.9.1
$ sudo ln -s /usr/bin/ruby1.9.1 /usr/bin/ruby

$ sudo apt-get install libopenssl-ruby1.9.1

```

```
$ sudo apt-get install rubygems1.9.1
$ sudo ln -s /usr/bin/gem1.9.1 /usr/bin/gem

```

Installing Rails

```
$ sudo apt-get install rails

```

or

```
$ sudo gem install rails

Successfully installed rake-0.8.7
Successfully installed activesupport-2.3.5
Successfully installed activerecord-2.3.5
Successfully installed rack-1.0.1
Successfully installed actionpack-2.3.5
Successfully installed actionmailer-2.3.5
Successfully installed activeresource-2.3.5
Successfully installed rails-2.3.5
8 gems installed
Installing ri documentation for rake-0.8.7...
Installing ri documentation for activesupport-2.3.5...
Installing ri documentation for activerecord-2.3.5...
Installing ri documentation for rack-1.0.1...
Installing ri documentation for actionpack-2.3.5...
Installing ri documentation for actionmailer-2.3.5...
Installing ri documentation for activeresource-2.3.5...
Installing ri documentation for rails-2.3.5...
Updating class cache with 0 classes...
Installing RDoc documentation for rake-0.8.7...
Installing RDoc documentation for activesupport-2.3.5...
Installing RDoc documentation for activerecord-2.3.5...
Installing RDoc documentation for rack-1.0.1...
Installing RDoc documentation for actionpack-2.3.5...
Installing RDoc documentation for actionmailer-2.3.5...
Installing RDoc documentation for activeresource-2.3.5...
Installing RDoc documentation for rails-2.3.5...

```

## Creating a New Rails Project

export PATH=$PATH:/home/neo/.gem/ruby/1.8/bin

### Creating the Blog Application

```
$ rails blog
      create
      create  app/controllers
      create  app/helpers
      create  app/models
      create  app/views/layouts
      create  config/environments
      create  config/initializers
      create  config/locales
      create  db
      create  doc
      create  lib
      create  lib/tasks
      create  log
      create  public/images
      create  public/javascripts
      create  public/stylesheets
      create  script/performance
      create  test/fixtures
      create  test/functional
      create  test/integration
      create  test/performance
      create  test/unit
      create  vendor
      create  vendor/plugins
      create  tmp/sessions
      create  tmp/sockets
      create  tmp/cache
      create  tmp/pids
      create  Rakefile
      create  README
      create  app/controllers/application_controller.rb
      create  app/helpers/application_helper.rb
      create  config/database.yml
      create  config/routes.rb
      create  config/locales/en.yml
      create  db/seeds.rb
      create  config/initializers/backtrace_silencers.rb
      create  config/initializers/inflections.rb
      create  config/initializers/mime_types.rb
      create  config/initializers/new_rails_defaults.rb
      create  config/initializers/session_store.rb
      create  config/environment.rb
      create  config/boot.rb
      create  config/environments/production.rb
      create  config/environments/development.rb
      create  config/environments/test.rb
      create  script/about
      create  script/console
      create  script/dbconsole
      create  script/destroy
      create  script/generate
      create  script/runner
      create  script/server
      create  script/plugin
      create  script/performance/benchmarker
      create  script/performance/profiler
      create  test/test_helper.rb
      create  test/performance/browsing_test.rb
      create  public/404.html
      create  public/422.html
      create  public/500.html
      create  public/index.html
      create  public/favicon.ico
      create  public/robots.txt
      create  public/images/rails.png
      create  public/javascripts/prototype.js
      create  public/javascripts/effects.js
      create  public/javascripts/dragdrop.js
      create  public/javascripts/controls.js
      create  public/javascripts/application.js
      create  doc/README_FOR_APP
      create  log/server.log
      create  log/production.log
      create  log/development.log
      create  log/test.log

```

### Configuring a Database

default database

```
$ gem install sqlite3-ruby

```

```
$ rails blog -d mysql
or
$ rails blog -d postgresql

```

#### Creating the Database

```
$ cd blog
$ rake db:create

```

### Hello world

home controller

```
$ script/generate controller home index
      exists  app/controllers/
      exists  app/helpers/
      create  app/views/home
      exists  test/functional/
      create  test/unit/helpers/
      create  app/controllers/home_controller.rb
      create  test/functional/home_controller_test.rb
      create  app/helpers/home_helper.rb
      create  test/unit/helpers/home_helper_test.rb
      create  app/views/home/index.html.erb

```

edit view

```
$ vim app/views/home/index.html.erb

```

### Starting up the Web Server

```
$ script/server
=> Booting WEBrick
=> Rails 2.3.5 application starting on http://0.0.0.0:3000
=> Call with -d to detach
=> Ctrl-C to shutdown server
[2010-05-22 16:24:04] INFO  WEBrick 1.3.1
[2010-05-22 16:24:04] INFO  ruby 1.9.1 (2010-01-10) [x86_64-linux]
[2010-05-22 16:24:04] INFO  WEBrick::HTTPServer#start: pid=30711 port=3000

```

### Setting the Application Home Page

The first step to doing this is to delete the default page from your application

```
$ rm -rf public/index.html
or
$ mv public/index.html public/index.html.off

```

```
$ vim config/routes.rb

  map.connect ':controller/:action/:id'
  map.connect ':controller/:action/:id.:format'
  map.root :controller => "home"

```

### FAQ

http://rbjl.net/20-rubybuntu-2-troubleshooting-common-ruby-ubuntu-problems

## capistrano

## 第 21 章 FAQ

## no such file to load — mkmf

```
sudo apt-get install ruby-dev

```