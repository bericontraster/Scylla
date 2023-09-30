## Tools
`The Guide`

## Information Gathering
- [Passive](#)
  - [whois](#WHOIS)
  - [Nslookup](#Nslookup)
  - [Dig](#Dig)

# Passive

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