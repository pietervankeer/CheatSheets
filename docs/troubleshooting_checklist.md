# Troubleshooting checklist

Bottum-up troubleshooting.

- [Troubleshooting checklist](#troubleshooting-checklist)
  - [Physical layer](#physical-layer)
    - [Bare metal](#bare-metal)
    - [VM](#vm)
  - [Internet network access layer](#internet-network-access-layer)
    - [_local_ network configuration](#local-network-configuration)
      - [IP address](#ip-address)
        - [Example: DHCP](#example-dhcp)
        - [Example: Fixed IP](#example-fixed-ip)
        - [Common causes (DHCP)](#common-causes-dhcp)
        - [Common causes (Fixed IP)](#common-causes-fixed-ip)
      - [Default gateway](#default-gateway)
      - [DNS service](#dns-service)
      - [Verify connection](#verify-connection)
    - [Routing within the _lan_](#routing-within-the-lan)
  - [Transport layer](#transport-layer)
    - [Is the service running?](#is-the-service-running)
    - [Correct ports/interfaces?](#correct-portsinterfaces)
    - [Firewall settings](#firewall-settings)
  - [Application layer](#application-layer)
    - [Check the log files](#check-the-log-files)
    - [Check config file syntax](#check-config-file-syntax)
    - [Read the F*cking Manual](#read-the-fcking-manual)
  - [SELinux Troubleshooting](#selinux-troubleshooting)
    - [Do Not disable SELinux](#do-not-disable-selinux)
    - [Check file context](#check-file-context)
    - [Check booleans](#check-booleans)
    - [Creating a policy](#creating-a-policy)
    - [Editing the policy](#editing-the-policy)
  - [General guidelines](#general-guidelines)

---

## Physical layer

Check the hardware!

- Is the power on?
- Are the cables connected?
- `ip link`

### Bare metal

- Test the cables
- Check switch/NIC LEDs

### VM

- Check virtual network adapter type & settings

## Internet network access layer

__Know the expected configuration!__

### _local_ network configuration

Check following:

1. IP address
2. Default gateway
3. DNS service

#### IP address

Commands:

- `ip a`

Check:

- Correct IP address?
- Correct Subnet?
- DHCP or fixed IP?
- Check configuration:
  - `/etc/sysconfig/network-scripts/ifcfg-*`

> netwerk herstarten; `sudo systemctl restart network`

##### Example: DHCP

![example: DHCP](images/dhcp.JPG)

##### Example: Fixed IP

![example: Fixed IP](images/fixedip.JPG)

##### Common causes (DHCP)

- No IP
  - DHCP unreachable
  - DHCP won't give an IP
- `169.254.x.x`
  - No DHCP offer, "link-local" address
- Unexpected subnet
  - Bad config (fixed IP set?)

Watch the logs: `sudo journalctl -f`

##### Common causes (Fixed IP)

- Unexpected subnet
  - check config
- Correct IP, "network unreachable"
  - Check network mask

#### Default gateway

Commands:

- `ip r`

Check:

- Default gateway present?
- In correct subnet?
- Check network configuration

#### DNS service

Commands:

- `etc/resolv.conf`

Check:

- `nameserver` option present?
- Expected IP?

#### Verify connection

- Ping between hosts
- Ping GW/DNS (Gateway / Domain name server)
- Query DNS (`dig`, `nslookup`, `getent`)

### Routing within the _lan_

Commands:

- `ip route`

## Transport layer

1. Service running?
   1. `sudo systemctl status SERVICE`
2. Correct port/interface?
   1. `sudo ss -tulpn`
3. Firewall settings?
   1. `sudo firewall-cmd --lists-all`

### Is the service running?

`systemctl status httpd.service`

### Correct ports/interfaces?

- Use `ss` (not `netstat`)
  - TCP service: `sudo ss -tlnp`
  - UDP service: `sudo ss -ulnp`
- Correct port number
  - check `/etc/services`
- Correct interface?

### Firewall settings

`sudo firewall-cmd --list-all`

- Is the service or port listed?
- User `--add-service` if possible
- add `--permanent`
- `--reload` firewall rules

```bash
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --add-service=https --permanent
sudo firewall-cmd --reload
```

## Application layer

- Check the _logs:_ `journalctl`
- Validate the config file syntax
- Use (command line) _client_ tools
  - _vb:_ `curl`, `smbclient`, `dig`
  - Netcat (`netcat`, `nc`)
- Other checks are application dependent
  - read the Reference manuals

### Check the log files

- Either `journalctl`: `journalctl -f -u httpd.service`
- or `/var/log/`:
  - `tail -f /var/log/httpd/error_log`

### Check config file syntax

Application dependent, for Apache: `apachectl configtest`

### Read the F*cking Manual

- [RedHat manuals](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/9-beta):
  - System administrator's guide
  - Networking guide
  - SELinux guide
- Reference manuals, e.g:
  - <https://httpd.apache.org/docs/2.4/configuring.html>
- Man pages
  - smb.conf(5), dhcpd.conf(5), named.conf(5),...

## SELinux Troubleshooting

SELinux is Mandatory Access Control in the Linux kernel.  
Settings:
  
- Booleans: `getsebool`, `setsebool`
- Context, labels: `ls -Z`, `chron`, `restorecon`
- Policy modules: `sepolicy`

### Do Not disable SELinux

<https://stopdisablingselinux.com/>

### Check file context

- Is the file context as expected?
  - `ls -Z /var/www/html`
- Set file context to default value
  - `sudo restorecon -R /var/www`
- Set file context to specified value
  - `sudo chron -t httpd_sys_content_t test.php`

### Check booleans

`getsebool -a | grep http`

- Know the relevant booleans! (Redhat manuals)
- Enable booleans:
  - `sudo setsebool -P httpd_can_network_connec_db on`

### Creating a policy

let's try to set `DocumentRoot "/vagrant/www"`

```bash
sudo vi /etc/httpd/conf/httpd.conf
ls -Z /vagrant/www/
-rw-rw-r--. vagrant vagrant system_u:object_r:vmblock_t:s0   test.php
sudo chcon -R -t httpd_sys_content_t /vagrant/www/
chcon: failed to change context of ‘test.php’ to ‘system_u:object_r:httpd_sys_content_t:s0’: Operation not supported
chcon: failed to change context of ‘/vagrant/www/’ to ‘system_u:object_r:httpd_sys_content_t:s0’: Operation not supported
```

Instead of setting the files to the expected context, allow httpd to access files with `vmblock_t` context

1. Allow Apache to run in "permissive" mode:
   1. `sudo semanage permissive -a httpd_t`
2. Generate "Type Enforcement" file (.te)
   1. `sudo audit2allow -a -m httpd-vboxsf > httpd-vboxsf.te`
3. If necessary, edit the policy
   1. `sudo vi httpd-vboxsf.te`

### Editing the policy

1. Convert to policy module (.pp)
   1. `checkmodule -M -m -o httpd-vboxsf.mod httpd-vboxsf.te`
   2. `semodule_package -o httpd-vboxsf.pp -m httpd-vboxsf.mod`
2. Install module
   1. `sudo semodule -i httpd-vboxsf.pp`
3. Remove permissive domain exception
   1. `semanage permissive -d httpd_t`

> Tip: Automate this!

## General guidelines

- Backup config files before changing
- Be systematic, bottom-up
- Be thorough, don't skip steps
- Do not assume --> test
- Know your environment
- Know your log files
- Read the log files
- Open logs in seperate terminal
- Small steps
- Verify each change
- Validate the syntax of config files
- Reload service after config change
- Keep a cheat sheet/checklist
- Use a configuration management system
- Automate tests
- never ping google
