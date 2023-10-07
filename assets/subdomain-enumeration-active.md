# Subdomain Enumeration
`The Guide`

- [Zone Transfers](#zontransferme)
  - [Zontransfer.me](#Zontransfer.me)
  - [Nslookup](#Nslookup)
- [Gobuster](#Gobuster)
- [Subdomain Finder](#subdomain)

## Zontransfer.me
The [zone transfer](https://hackertarget.com/zone-transfer/) is how a secondary DNS server receives information from the primary DNS server and updates it. The master-slave approach is used to organize DNS servers within a domain, with the slaves receiving updated DNS information from the master DNS. The master DNS server should be configured to enable zone transfers from secondary (slave) DNS servers, although this might be misconfigured.

![transfer.me-image](/media/transfer-me.png)

## Nslookup

A manual approach will be the following. <br>
`Identifying Nameservers`
```bash
nslookup -type=NS zonetransfer.me
```
`Testing for ANY and AXFR Zone Transfer`
```bash
nslookup -type=any -query=AXFR zonetransfer.me nsztm1.digi.ninja
```

> **NOTE:** If we manage to perform a successful zone transfer for a domain, there is no need to continue enumerating this particular domain as this will extract all the available information.

## Gobuster
We can use a wordlist from [Seclists](https://github.com/danielmiessler/SecLists) repository.

| CMD | EXPLAIN |
|:----|---------|
| `dns` | Launch the DNS module. |
| `q` | Don't print the banner and other noise. |
| `r` | Use custom DNS server. |
| `d` | A target domain name. |
| `p` | Path to the patterns file. |
| `w` | Path to the wordlist. |
| `o` | Output file. |

**Gobuster - DNS**
```bash
gobuster dns -q -r "${NS}" -d "${TARGET}" -w "${WORDLIST}" -p ./patterns.txt -o "gobuster_${TARGET}.txt"
```

## Subdomain Finder {id:subdomain}
Redirect [Link](https://subdomainfinder.c99.nl/). 