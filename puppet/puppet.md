% Intro to Puppet
% Ethan House (ehouse@csh.rit.edu)

# What is Puppet?

## Lazy, Stateful, Configuration Management 
### What does that mean?

## Lazy
- It runs periodically across all nodes

## Stateful
- Puppet can only set state

`service { 'apache': ensure => running }`

## Config Management
- Configure Services and Configurations across nodes on a network

# How can I use Puppet?

## Config Management
- As either whole files
- Templates with configurable parameters
- Augeas which breaks configs down into trees of values

## Service Management
- Set services as running or disabled
- Puppet will ensure that the service is running/disabled when puppet runs

## Scheduled Jobs
- Schedule jobs to run withing a period of time N times
- EX. Will run `yum update` twice between 2-4AM

# Puppet in Action

## 
### Let's add a user and a package
- First define the user we want to create
- We want to ensure it's always present
- We create a SHA2 hash and set that as the password
- Finally we ensure tmux is at the latest release 

## 
``` ruby
user { 'ehouse':
    ensure = present,
    password = "$6$LDh8dZMym27I4raQ$JUVucmVQ3t8aVI5T4U4WQhGUN9J6wxAg.RX/m3H67F8dhPPvJRbm4a0IcGhpaSU9q732rLxrmSXzAWoaYnTwV.",
}

package { 'tmux':
    ensure = lastest,
}
```

##
### Let's set templates and Augeas
- We disallow password root logins
- We want to notify puppet to reload the sshd configs
- Lastly, we'll overwrite the ntp config with our own template

## 
``` ruby
augeas { "sshd_config":
  context => "/files/etc/ssh/sshd_config",
  changes => [
    "set PermitRootLogin without-password",
  ],
  notify => Service["sshd"],
}

file { '/etc/ntp.conf':
  ensure  => file,
  content => template('ntp/ntp.conf.erb'),
}
```

##
### Let's set a scheduled event
- The event will run twice between the hours of 2-4AM every day called 'daily'
- We will set `yum update` to the 'daily' schedule
- Multiple resource's can be set to the same schedule

##
``` ruby
schedule { "daily":
  period => daily,
  range  => "2 - 4",
  period => 2,
}

exec { "yum update":
  schedule => 'daily',
}
```

# Master of Puppets

## Augeas
``` ruby
augeas { "sshd_config":
  changes => [
    "set /files/etc/ssh/sshd_config/PermitRootLogin no",
  ],
}
```
- Edit sshd\_config and disallow root logins
- Runs only once and only modifies one line so safer then templates and files

## Foreman Web Interface
- Manages hosts and host groups
- Apply and modify modules on the fly
- Set template and module parameters

## BASIC FOREMAN W/ SCREEN SHOTS
Set hosts in host groups
Set modules to host groups

## ADVANCED FOREMAN W/ SCREEN SHOTS
Set parameters

## Central Git Server
- Use git to manage puppet modules
- Central git server as source of truth
- Standing up new puppet server is trivial

## Additional Resources
- [Generating Secure SHA2 Hashes](https://gist.github.com/ehouse/7247884bfd30fae98bef)
- [Built-in Type Reference](https://docs.puppetlabs.com/references/latest/type.html)
