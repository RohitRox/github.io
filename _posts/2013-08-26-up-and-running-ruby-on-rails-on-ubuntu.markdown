---
layout: post
title: Up and running Ruby on Rails on Ubuntu
date: 2013-08-26 22:27
comments: true
categories: [Rails, Linux]
tags: [rails, ubuntu]
---

### 6 steps for setting ruby on rails development environment in Ubuntu.

### Step 1: Setting up RVM

``` bash
    $ sudo apt-get update
    $ sudo apt-get install curl
    $ curl -L https://get.rvm.io | bash -s stable
    $ source ~/.rvm/scripts/rvm
```

In order to work, RVM has some of its own dependancies that need to be installed. You can see what these are:

``` bash
    $ rvm requirements
    # In the text that RVM shows you, look for this paragraph.
    # Additional Dependencies:
    # For ruby:
    # apt-get --no-install-recommends install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev  libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev libgdbm-dev ncurses-dev automake libtool bison subversion pkg-config libffi-dev
    # Just follow the instructions to get your system up to date with all of the required dependancies.
    $ sudo apt-get --no-install-recommends install build-essential openssl libreadline6 libreadline6-dev curl git-core zlib1g zlib1g-dev libssl-dev libyaml-dev libsqlite3-dev sqlite3 libxml2-dev libxslt-dev autoconf libc6-dev libgdbm-dev ncurses-dev automake libtool bison subversion pkg-config libffi-dev
```
Then

``` bash
    $ echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" >> ~/.bash_profile
    # or
    $ echo '[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" >> ~/.bashrc
```

### Step 2: Install ruby

``` bash
    $ rvm install 1.9.3
    # after success you can verify your installation by
    $ rvm list
    # should output something like
    # rvm rubies
    # =* ruby-1.9.3-p362 [ i686 ]
```

### Step 3: RubyGems

``` bash
    $ rvm rubygems current
```


### Step 4: Rails

``` bash
    # a Javascript runtime, NodeJs
    $ sudo add-apt-repository ppa:chris-lea/node.js
    $ sudo apt-get update
    $ sudo apt-get install nodejs

    $ gem install bundler
    $ gem install rails
```

### Step 5: First Rails app

```
    $ rails new my_app
    $ cd my_app
    $ bundle install
    $ rails s
    # open browser and visit localhost:3000
    # you should see the famous welcome abroad page
```
### Step 6: Development Extras

#### Sublime Text 2

``` bash
    $ sudo add-apt-repository ppa:webupd8team/sublime-text-2
    $ sudo apt-get update
    $ sudo apt-get install sublime-text
```
Install sublime package manager from [here](http://wbond.net/sublime_packages/package_control/installation) for other sublime goodies.

#### Configure Vim for Rails

``` bash
    $ curl -Lo- https://bit.ly/janus-bootstrap | bash
```
