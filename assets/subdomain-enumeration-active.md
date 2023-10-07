# Subdomain Enumeration
`The Guide`

- [Zontransfer.me](#Zontransfer.me)
- [Nslookup](#Nslookup)

## Zontransfer.me
The [zone transfer](https://hackertarget.com/zone-transfer/) is how a secondary DNS server receives information from the primary DNS server and updates it. The master-slave approach is used to organize DNS servers within a domain, with the slaves receiving updated DNS information from the master DNS. The master DNS server should be configured to enable zone transfers from secondary (slave) DNS servers, although this might be misconfigured.

![transfer.me-image](/media/transfer-me.png)

## Nslookup

A manual approach will be the following.
`Identifying Nameservers`
```bash
nslookup -type=NS zonetransfer.me
```
`Testing for ANY and AXFR Zone Transfer`
```bash
nslookup -type=any -query=AXFR zonetransfer.me nsztm1.digi.ninja
```

> **NOTE:** If we manage to perform a successful zone transfer for a domain, there is no need to continue enumerating this particular domain as this will extract all the available information.

