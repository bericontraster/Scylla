# Linux Reconnaissance
`The Guide`

- [Network File System - NFS](#network-file-system)

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
