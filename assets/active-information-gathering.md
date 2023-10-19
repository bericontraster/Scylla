# Active Information Gathering
`The Guide`

- [Infrastructure Identification](#)
  - [WhatWeb](#WhatWeb)
  - [Wappalyzer](#Wappalyzer)
  - [WafW00f](#WafW00f)
  - [Aquatone](#Aquatone)


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