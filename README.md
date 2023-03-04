# Linux Setup
This is almost certainly not the best way to do it, but it's what worked for me.


## Install Linux Mint from USB thumbstick
Following [this guide](https://linuxmint-installation-guide.readthedocs.io/en/latest/)

* Set up initial account as parent account
* Set download mirrors to (If you don't you'll need to update what's in the dnsmasq conf to whatever you do choose):
  * mirror.cs.jmu.edu
  * James Madison
* Follow startup guide, especially installing updates and firewall.
* Install additional language packs by clicking on system reports in the taskbar.
* Reboot

## Install Stuff
* Log into parent account
* ```bash
  $ sudo apt-get update
  $ sudo apt-get install emacs
  $ sudo apt-get install ktouch
  ```

## Set up DNS with dnsmasq
* add `DNSStubListener=no` in `/etc/systemd/resolved.conf`.
* Add `dns=none` in `[main]` section of `/etc/NetworkManager/NetworkManager.conf`. (May not be necessary)
* Replace `/etc/resolv.conf` with `nameserver 127.0.0.53` (be careful to save a backup of the original symlink).
* `$ sudo systemctl restart systemd-resolved`.
* `$ sudo systemctl restart NetworkManager`
  * Optionally can check if this has removed systemd-resolved from listening on port 53 with `sudo ss -lp "sport = :domain"`
* `$ sudo apt-get install dnsmasq`

_May need to set up dnsmasq to start at boot, not sure_


## Configure dnsmasq to function as an allowlist
* Get the wifi interface name by running `ifconfig` and parsing output, should be something like `wlan0` or `wlx...`
* add the following to `/etc/dnsmasq.conf`:
  ```bash
  domain-needed
  bogus-priv
  log-queries
  log-facility=/var/log/dnsmasq.log
  no-resolv
  interface=[INTERFACE NAME FOUND IN PRIOR STEP]
  
  # apt-get server (Modify if changed in initial setup)
  server=/jmu.edu/8.8.8.8
  server=/google.com/8.8.8.8
  # Google static assets
  server=/gstatic.com/8.8.8.8
  server=/mozilla.org/8.8.8.8
  server=/mozilla.com/8.8.8.8
  server=/mozilla.net/8.8.8.8
  server=/firefox.com/8.8.8.8
  server=/example.org/8.8.8.8
  server=/mozaws.net/8.8.8.8
  server=/mozgcp.net/8.8.8.8
  server=/ipv4only.arpa/8.8.8.8
  server=/bitwarden.net/8.8.8.8
  server=/wikipedia.org/8.8.8.8
  server=/github.com/8.8.8.8
  server=/stackoverflow.com/8.8.8.8
  # Pocket thing used by firefox
  server=/getpocket.com/8.8.8.8
  <etc>
  ```
  
* `$ sudo systemctl restart dnsmasq`

## Create kid account
* Follow [these instructions](https://www.technipages.com/linux-mint-how-to-add-a-new-user). The kid account is a standard user (not admin).
* Set an initial password, then `$ sudo passwd -e [username]` to force them to change it next time they log in. 
* Add kid account to Timeshift (backups), if using. 
* 
