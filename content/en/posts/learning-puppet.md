+++
title = "Learning Puppet"
tags = ["ruby"]
categories = ["puppet", "devops"]
date = "2018-05-07"
+++

# Learning Puppet
Hi! Recently I was reviewing two courses about Jenkins, so I had to use Vagrant for configure my environment, also I used other tools as Puppet.

In the developer day is very common configure linux servers for a lot of things: install a Jenkins, prepare the deploy environments, etc… Well, Puppet can help us to configure this servers, as a improve script shell. In this post I’m going to talk about the first steps for use Puppet.

> I recommend start to learning using Vagrant for create virtual machines ( By default Vagrant machines have installed an old Puppet version), you can know more in the Making Devs Blog: [Creating virtual machines with Vagrant · Blog de Making Devs](http://blog.makingdevs.com/2017/06/01/creating-virtual-machines-with-vagrant/)

### Benefits of use Puppet for configure servers

- Keeping the configuration synchronized
- Repeating changes across many servers
- Self-updating documentation
- Version Control

Puppet is a great tool for make this, instead of use manual configuration, shell scripts (fragile, hard to maintain, very specific), or use container as Docker (require more configuration management).

Puppet is more! Puppet is a language and an engine for applies configuration and managements.

### Installing Puppet

The most easy way for install puppet is download the TAR version [here](http://downloads.puppetlabs.com/puppet/) and run the ruby script.

For install Puppet 5.5.0 in ubuntu:
```
$ wget http://downloads.puppetlabs.com/puppet/puppet-5.5.0.tar.gz
$ tar -xzf puppet-5.5.0.tar.gz
$ cd puppet-5.5.0/
$ sudo ./install.rb
$ puppet --version
5.5.0
```

### Starting With Puppet

Puppet can manage three basic resource types:

- Package: for install software.
- File: for deploy configuration files.
- Service: for runs software itself.

I’m going to create my hello world example. For this I created the follow script:

**hello-world.pp**
```
file { '/tmp/hello.txt':
  ensure  => file,
  content => "hello, world Vagrant with Puppet!!!\n",
}
```

For run this in the empty server:
> sudo puppet apply hello-world.pp

```
vagrant@vagrant-ubuntu-trusty-64:~$ sudo puppet apply puppet1.pp

Warning: Could not retrieve fact fqdn
Notice: Compiled catalog for vagrant-ubuntu-trusty-64 in environment production in 0.05 seconds
Notice: /Stage[main]/Main/File[/tmp/hello.txt]/ensure: defined content as '{md5}4f4e8edd5c7cd9dc6c2d3aaca1baf2c1'
Notice: Finished catalog run in 0.01 seconds

vagrant@vagrant-ubuntu-trusty-64:~$ cat /tmp/hello.txt
hello, world Vagrant with Puppet!!!
```

The term **File** is a puppet declaration, **ensure** is for specify the resource type, and **content** is an attribute for set the content in the file.

### Install Packages

We can create a puppet script for install packages, it’s very easy:

**package.pp**
```
package { 'fish':
	ensure => installed,
}
```

```
vagrant@vagrant-ubuntu-trusty-64:~$ sudo puppet apply package.pp
Warning: Could not retrieve fact fqdn
Notice: Compiled catalog for vagrant-ubuntu-trusty-64 in environment production in 0.09 seconds
Notice: /Stage[main]/Main/Package[fish]/ensure: ensure changed 'purged' to 'present'
Notice: Finished catalog run in 7.97 seconds
```
### Running Services

We can start services too:

**puppet3.pp**
```
service { 'jenkins':
  ensure => running,
  enable => true,
}
```

```
vagrant@vagrant-ubuntu-trusty-64 ~> sudo puppet apply service.pp
Warning: Could not retrieve fact fqdn
Notice: Compiled catalog for vagrant-ubuntu-trusty-64 in environment production in 0.06 seconds
Notice: Finished catalog run in 0.06 seconds
```

### Resources for learn more:

- [Infrastructure as Code (IAC) Cookbook | PACKT Books](https://www.packtpub.com/virtualization-and-cloud/infrastructure-code-iac-cookbook)
- [Puppet 5 Beginner’s Guide - Third Edition | PACKT Books](https://www.packtpub.com/networking-and-servers/puppet-5-beginner%E2%80%99s-guide-third-edition)

## Thanks for reading!
This are the basic uses with this tool. I hope write more about this.
