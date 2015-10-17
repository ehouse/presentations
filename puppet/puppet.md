% Intro to Puppet
% Ethan House (ehouse@csh.rit.edu)

# What is Puppet?

## Lazy Stateful Configuration Managment 
### What does that mean

## Lazy
- It runs peridodicly across all nodes

## Stateful
- Puppet can only set state

    service { 'apache': ensure => running }

## Config Managment
- Configure Services and Configurations across nodes on a network

# How can I use Puppet?

## Config Managment
- As either whole files
- Templates with configurable parameters
- Augeas which breaks configs down into trees of values

## Service Managment
- Set services as running or disabled
- Puppet will ensure that the service is ALWAYS running/disabled

## Running Scheduled Jobs

    schedule { 'daily':
      period => daily,
      range  => "2 - 4",
      period => 1,
    }

    exec { "yum update":
      schedule => 'daily',
    }

- MAY run yum update between 0200 and 0400 once
- Only a problem if range is too small

# Puppet in Action

## BASIC SAMPLE CONFIG
Install some packages
add a user

## ADVANCED SAMPLE CONFIG
Set a template w/ parameters
Set timed updates

# Master of Puppets

## Augeas

    augeas { "sshd_config":
      changes => [
        "set /files/etc/ssh/sshd_config/PermitRootLogin no",
      ],
    }

- Edit sshd\_config and disallow root logins
- Runs only once and only modifies one line so safer then taples and files

## Foreman Web Interface
- Manages hosts and hostgroups
- Apply and modify modules on the fly
- Set template and module parameters

## BASIC FOREMAN W/ SCREENSHOTS
Set hosts in hostgroups
Set modules to hostgroups

## ADVANCED FOREMAN W/ SCREENSHOTS
Set parameters

## Central Git Server
- Use git to manage puppet modules
- Central git server as source of truth
- Standing up new puppet server is trivial
