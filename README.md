# Linux Setup

## Install Linux Mint from USB thumbstick
Set up initial account as child account

## Create parent account

## Set privileges
* Log into parent account
* To check groups, run `groups [parents]
* ```bash
  $ sudo adduser parents adm
  $ sudo adduser parents cdrom
  $ sudo adduser parents lpadmin
  $ sudo adduser parents plugdev
  $ sudo deluser danielle sudo
  ```
 
## Install Stuff
* Log into parent account
* ```bash
  $ sudo apt-get update
  $ sudo apt-get install emacs
  $ sudo apt-get install ktouch
  ```
