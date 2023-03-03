# Linux Setup
This is almost certainly not the best way to do it, but it's what worked for me.


## Install Linux Mint from USB thumbstick
Set up initial account as child account

## Create parent account
* Log into the kid account
* ```bash
  $ sudo adduser parents
  $ sudo usermod -aG sudo parents
  ```

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
