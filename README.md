# Linux Setup
This is almost certainly not the best way to do it, but it's what worked for me.


## Install Linux Mint from USB thumbstick
Following [this guide](https://linuxmint-installation-guide.readthedocs.io/en/latest/)

* Set up initial account as parent account
* Follow startup guide, especially installing updates

## Create parent account
* Log into the kid account
* ```bash
  $ sudo adduser parents
  $ sudo usermod -aG sudo parents
  ```

## Set privileges
* Log into parent account
* To check groups, run `groups [parents]`
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

## Set up DNS with dnsmasq
* Log into parent account
* add `DNSStubListener=no` in `/etc/systm/resolved.conf`.
* add `dns=none` in `[main]` section of `/etc/NetworkManager/NetworkManager.conf`.
* <May have to replace `/etc/resolv.conf` with `nameserver 127.0.0.53` (be careful to save a backup of the original symlink)>
* `$ sudo systemctl restart NetworkManager`
  * Optionally can check if this has removed systemd-resolved from listening on port 53 with `sudo ss -lp "sport = :domain"`
* `$ sudo apt-get install dnsmasq`

_May need to set up dnsmasq to start at boot, not sure_


## Configure dnsmasq to function as an allowlist
* Log into parent account
* Get the wifi interface name by running `ifconfig` and parsing output, should be something like `wlan0` or `wlx...`
* add the following to `/etc/dnsmasq.conf`:
  ```bash
  domain-needed
  bogus-priv
  log-queries
  log-facility=/var/log/dnsmasq.log
  no-resolv
  interface=[INTERFACE NAME FOUND IN PRIOR STEP]
  
  server=/google.com/8.8.8.8
  server=/mozilla.org/8.8.8.8
  server=/mozilla.com/8.8.8.8
  server=/bitwarden.net/8.8.8.8
  server=/github.com/8.8.8.8
  <etc>
  ```


* Add kid account to backups
