# Environment-based Privilege Escalation

- [Escaping Restricted Shells]()


## Escaping Restricted Shells
`Types`
> **RBASH** [Restricted Bourne shell](https://www.gnu.org/software/bash/manual/html_node/The-Restricted-Shell.html)
> **RKSH** [Restricted Korn shell](https://www.ibm.com/docs/en/aix/7.2?topic=r-rksh-command)
> **RZSH** [Restricted Z shell](https://manpages.debian.org/experimental/zsh/rzsh.1.en.html)

**Escape**
```bash
ssh htb-user@10.129.27.68 -t "bash --noprofile"
# don't forget to hit ^C 
```
`Sources`
- [echo](https://stackoverflow.com/questions/22377792/how-to-use-echo-command-to-print-out-content-of-a-text-file)
- [source-1](https://0xffsec.com/handbook/shells/restricted-shells/)
