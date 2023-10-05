## Tools
`The Guide` :world_map:

### Passive information gathering.
- [Whois](#WHOIS)
- [Nslookup](#Nslookup)
- [Dig](#Dig)
- [Virustotal](#Virustotal)
- [Certificates](#Certificates)
  - [Curl](#Curl)
  - [Openssl](#Openssl)
- [The Harvester](/assets/the-harvester-passive.md)

## WHOIS
Whois basic scan
```bash
whois google.com
```
Whois basic scan on windows
```bash
whois.exe google.com
```

## Nslookup

Querying: A Records
```bash
nslookup google.com
```

Querying: A Records for a Subdomain
```bash
nslookup -query=A google.com
```

Querying: PTR Records for an IP Address
```bash
nslookup -query=PTR 31.13.92.36
```

Querying: ANY Existing Records
```bash
nslookup -query=ANY google.com
```

Querying: TXT Records
```bash
nslookup -query=TXT google.com
```

Querying: MX Records
```bash
nslookup -query=MX google.com
```

## Dig

Querying: A Records
```bash
dig google.com @8.8.8.8
```

Querying: A Records for a Subdomain 
```bash
dig a google.com @8.8.8.8
```

Querying: PTR Records for an IP Address
```bash
dig -x 31.13.92.36 @1.1.1.1
```

Querying: ANY Existing Records
```bash
dig any google.com @8.8.8.8
```

Querying: TXT Records
```bash
dig txt google.com @8.8.8.8
```

Querying: MX Records
```bash
dig mx facebook.com @1.1.1.1
```

## Virustotal
Click [here](https://www.virustotal.com/gui/home/upload).

## Certificates
- [Censys](https://search.censys.io/)
- [Crt](https://crt.sh/)
### Curl
```bash
curl -s "https://crt.sh/?q=${TARGET}&output=json" | jq -r '.[] | "\(.name_value)\n\(.common_name)"' | sort -u > "${TARGET}_crt.sh.txt"
head -n20 facebook.com_crt.sh.txt
```
| CMD | Explain |
|:----|---------|
| `curls -s` | Issue the request with minimal output. |
| `https://crt.sh/?q=<DOMAIN>&output=json` | Ask for the json output. |
| `jq -r '.[]' "\(.name_value)\n\(.common_name)"'` | Process the json output and print certificate's name vale and common name one per line.|
| `sort -u` | Sort alphabetically the output provided and removes duplicates.|

### Openssl
```bash
openssl s_client -ign_eof 2>/dev/null <<<$'HEAD / HTTP/1.0\r\n\r' -connect "${TARGET}:${PORT}" | openssl x509 -noout -text -in - | grep 'DNS' | sed -e 's|DNS:|\n|g' -e 's|^\*.*||g' | tr -d ',' | sort -u*.facebook.com
```