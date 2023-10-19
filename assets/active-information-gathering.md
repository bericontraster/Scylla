# Active Information Gathering
`The Guide`

- [Infrastructure Identification](#)
  - [WhatWeb](#WhatWeb)
  - [Wappalyzer](#Wappalyzer)
  - [WafW00f](#WafW00f)
  - [Aquatone](#Aquatone)
  - [Gobuster](#gobuster)
  - [FFuF](#ffuf)


## WhatWeb
[Whatweb](https://www.morningstarsecurity.com/research/whatweb) recognizes web technologies, including content management systems (CMS), blogging platforms, statistic/analytics packages, JavaScript libraries, web servers, and embedded devices.
```bash
whatweb -a3 https://www.facebook.com -v
```

## Wappalyzer
[Wappalyzer](https://www.wappalyzer.com/) is a browser extension. It has similar functionality to Whatweb, but the results are displayed while navigating the target URL.

## WafW00f
[WafW00f](https://github.com/EnableSecurity/wafw00f) is a web application firewall (WAF) fingerprinting tool that sends requests and analyses responses to determine if a security solution is in place. We can install it with the following command.
```bash
sudo apt install wafw00f -y
```
We can use options like `-a` to check all possible WAFs in place instead of stopping scanning at the first match, read targets from an input file via the `-i` flag, or proxy the requests using the `-p` option.
```bash
wafw00f -v https://www.tesla.com 
```

## Aquatone
[Aquatone](https://github.com/michenriksen/aquatone) is a tool for automatic and visual inspection of websites across many hosts and is convenient for quickly gaining an overview of `HTTP-based` attack surfaces by scanning a list of configurable ports, visiting the website with a headless Chrome browser, and taking and screenshot.
```bash
sudo apt install golang chromium-driver
go get github.com/michenriksen/aquatone
export PATH="$PATH":"$HOME/go/bin"
```
Use cat in our subdomain list and pipe the command to aquatone.
```bash
cat facebook_aquatone.txt | aquatone -out ./aquatone -screenshot-timeout 1000
```

## Gobuster
`Directory/File Fuzzing`
The commands that we have given allready have the best wordlists. Wordlists [source](https://github.com/danielmiessler/SecLists).
```bash
gobuster dir -u URL -w wordlist
gobuster dir -u http://IP -w /usr/share/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
```

`Virtual Host Fuzzing`
```bash
gobuster vhost -u URL -w wordlist
gobuster vhost -u http://IP -w /usr/share/wordlists/seclists/Discovery/DNS/subdomains-top1million-110000.txt
```

# FFUF
`VHost Fuzzing with FFuF`  
```bash
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt:FUZZ -u http://mailroom.htb/ -H 'Host: FUZZ.mailroom.htb' -ac -c -ic
```
Here is a breakdown of the command and its options:

    -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt:FUZZ specifies a wordlist to use for fuzzing. In this case, it is using the "subdomains-top1million-20000.txt" file located in the "/usr/share/seclists/Discovery/DNS" directory, and replacing the "FUZZ" placeholder with each word in the file.
    -u http://mailroom.htb/ specifies the target URL to fuzz.
    -H 'Host: FUZZ.mailroom.htb' adds a custom Host header to the HTTP request. This will replace the "FUZZ" placeholder in the header with each word in the wordlist.
    -ac prints all HTTP responses, including those with a non-2xx status code.
    -c colorizes the output.
    -ic ignores HTTP response codes with the 401 status code.

In summary, this command is using a wordlist to fuzz the subdomains of "mailroom.htb" by sending HTTP requests with a custom Host header. The output will be colorized and will include all HTTP responses except those with a 401 status code.

