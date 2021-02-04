---
layout: post
title: Upgrade Ruby and Jekyll
previousurl: null
nexturl: null
excerpt: "Upgrade Ruby and Jekyll"
date: "2021-02-03"
modified: "2021-02-03"
categories: "blog"
tags:
  - Blog setup
  - update
  - Ruby
  - Jekyll
image: rntwi-delta_RNTWI_java_2001-2016_mk-z-ts-model
comments: true
share: true
---

## Introduction

After [updating and upgrading Homebrew](https://karttur.github.io/setup-ide/blog/postgres-error-reinstall/) I got problems that subsequently caused a complete crash of Homebrew and I had to remove Homebrew and then reinstall. This, in turn, caused Jekyll and Ruby to crash. This post summarises my efforts in getting Rudy and Jekyll up and running again.

Maybe the problems stem from the _openssl_ version. _openssl_ is the suspected culprit for the cause of the problems with postgres.

### Updated RVM

To update the [Ruby Version Manager (RVM)](https://rvm.io) I just ran the terminal install command for RVM:

<span class='terminal'>$ curl -L https://get.rvm.io | bash -s stable</span>

```
$ curl -L https://get.rvm.io | bash -s stable
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100   194  100   194    0     0    419      0 --:--:-- --:--:-- --:--:--   420
100 24535  100 24535    0     0  37174      0 --:--:-- --:--:-- --:--:-- 37174
Downloading https://github.com/rvm/rvm/archive/1.29.12.tar.gz
Downloading https://github.com/rvm/rvm/releases/download/1.29.12/1.29.12.tar.gz.asc
Found PGP signature at: 'https://github.com/rvm/rvm/releases/download/1.29.12/1.29.12.tar.gz.asc',
but no GPG software exists to validate it, skipping.
Upgrading the RVM installation in /Users/thomasgumbricht/.rvm/
    RVM PATH line found in /Users/thomasgumbricht/.mkshrc /Users/thomasgumbricht/.profile /Users/thomasgumbricht/.bashrc /Users/thomasgumbricht/.bash_profile /Users/thomasgumbricht/.zshrc.
    RVM sourcing line found in /Users/thomasgumbricht/.profile /Users/thomasgumbricht/.bash_profile /Users/thomasgumbricht/.zlogin.
Upgrade of RVM in /Users/thomasgumbricht/.rvm/ is complete.
```

You can check the PATH command, using e.g. <span class='terminalapp'>pico</span> from your home (~) directory:

<span class='terminal'>$ pico .profile</span>

```
# Add RVM to PATH for scripting. Make sure this is the last PATH variable change.
export PATH="$PATH:$HOME/.rvm/bin"

[[ -s "$HOME/.rvm/scripts/rvm" ]] && source "$HOME/.rvm/scripts/rvm" # Load RVM into a shell session *as a function*
```

If all seems OK, check the RVM version:

<span class='terminal'>$ rvm version</span>

```
$ rvm version
RVM version 1.29.12 (latest) is installed, yet version 1.29.10 (latest) is loaded.

Please open a new shell or run one of the following commands:

    rvm reload
    echo rvm_auto_reload_flag=1 >> ~/.rvmrc # OR for auto reload with msg
    echo rvm_auto_reload_flag=2 >> ~/.rvmrc # OR for silent auto reload
```

This indicates that the installation worked, but you need to reload.

<span class='terminal'>$ rvm reload</span>

### Update Ruby

Check out the installed version on your machine:

 <span class='terminal'>$ ruby -v</span>

 And compare that with the latest version on the [Ruby homepage](https://www.ruby-lang.org/en/downloads/). To upgrade Ruby to a predefined version:

 <span class='terminal'>$ rvm install x.y.z</span>

 or to install the latest stable version:

<span class='terminal'>$ rvm get stable --ruby</span>

I opted for installing version 2.7.2:

```
$ rvm install 2.7.2
Searching for binary rubies, this might take some time.
No binary rubies available for: osx/10.14/x86_64/ruby-2.7.2.
Continuing with compilation. Please read 'rvm help mount' to get more information on binary rubies.
Checking requirements for osx.
Installing requirements for osx.
Updating system.......
Installing required packages: autoconf, automake, pkg-config, coreutils, libyaml, libksba, zlib........
Certificates bundle '/usr/local/etc/openssl@1.1/cert.pem' is already up to date.
Requirements installation successful.
Installing Ruby from source to: /Users/thomasgumbricht/.rvm/rubies/ruby-2.7.2, this may take a while depending on your cpu(s)...
ruby-2.7.2 - #downloading ruby-2.7.2, this may take a while depending on your connection...
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 14.0M  100 14.0M    0     0  10.3M      0  0:00:01  0:00:01 --:--:-- 10.3M
ruby-2.7.2 - #extracting ruby-2.7.2 to /Users/thomasgumbricht/.rvm/src/ruby-2.7.2.....
ruby-2.7.2 - #configuring.........................................................................
ruby-2.7.2 - #post-configuration.
ruby-2.7.2 - #compiling..........................................................................
ruby-2.7.2 - #installing............
ruby-2.7.2 - #making binaries executable...
Installed rubygems 3.1.4 is newer than 3.0.9 provided with installed ruby, skipping installation, use --force to force installation.
ruby-2.7.2 - #gemset created /Users/thomasgumbricht/.rvm/gems/ruby-2.7.2@global
ruby-2.7.2 - #importing gemset /Users/thomasgumbricht/.rvm/gemsets/global.gems................................................................
ruby-2.7.2 - #generating global wrappers........
ruby-2.7.2 - #gemset created /Users/thomasgumbricht/.rvm/gems/ruby-2.7.2
ruby-2.7.2 - #importing gemsetfile /Users/thomasgumbricht/.rvm/gemsets/default.gems evaluated to empty gem list
ruby-2.7.2 - #generating default wrappers........
ruby-2.7.2 - #adjusting #shebangs for (gem irb erb ri rdoc testrb rake).
Install of ruby-2.7.2 - #complete
Ruby was built without documentation, to build it run: rvm docs generate-ri
```

All went through and I could als0 generate the documentation:

<span class='terminal'>$ rvm docs generate-ri</span>

Set the default Ruby version in RVM:

<span class='terminal'>$ rvm default use x.y.z</span>

And verify the default version:

<span class='terminal'>$ ruby -v</span>

### System restored!

The crash caused by the postgres upgrade and the subsequent attempts to save the database also affected Ruby. After updating RVM and then Ruby as outlined above, I got Jekyll to work. Thus no need to also update Jekyll in order to the system up and running.

## Resources

[GitHub supported themes]([supported themes](https://pages.github.com/themes/))

[Set up blog tools: Jekyll and Atom](https://karttur.github.io/setup-blog/) another blog by me

[Setup GitHub pages](https://karttur.github.io/setup-github/) another blog by me
