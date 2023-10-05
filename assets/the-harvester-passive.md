# The Harvester
`The Guide` :world_map:

[TheHarvester](https://github.com/laramies/theHarvester) is a simple-to-use yet powerful and effective tool for early-stage penetration testing and red team engagements. We can use it to gather information to help identify a company's attack surface. The tool collects emails, names, subdomains, IP addresses, and URLs from various public data sources for passive information gathering.

| Name | Source |
|:-----|--------|
| [Baidu](http://www.baidu.com/) | Baidu search engine. |
| [Bufferoverun](www.rapid7.com/research/project-sonar/) | Uses data from Rapid7's Project Sonar. |
| [Crtsh](https://crt.sh/) | Comodo Certificate search. |
| [Hackertarget](https://hackertarget.com/) | Online vulnerability scanners and network intelligence to help organizations. |
| [Otx](https://otx.alienvault.com/) | AlienVault Open Threat Exchange. |
| [Rapiddns](https://rapiddns.io/) | DNS query tool, which makes querying subdomains or sites using the same IP easy. |
| [Sublist3r](https://api.sublist3r.com/search.php?domain=example.com) | Fast subdomains enumeration tool for penetration testers. |
| [Threatcrowd](http://www.threatcrowd.org/) | Open source threat intelligence. | 
| [Threatminer](https://www.threatminer.org/) | Data mining for threat intelligence. | 
| `Trello` | Search Trello boards (Uses Google search). |
| [Urlscan](https://urlscan.io/) | A sandbox for the web that is a URL and website scanner. |
| `Vhost` | Bing virtual hosts search. | 
| [Virustotal](https://www.virustotal.com/gui/home/search) | Domain search. |
| [Zoomeye](https://www.zoomeye.org/) | A Chinese version of Shodan. |



To automate this, we will create a file called sources.txt with the following contents.
```bash
Scylla[/guide]$ cat sources.txtbaidu
bufferoverun
crtsh
hackertarget
otx
projecdiscovery
rapiddns
sublist3r
threatcrowd
trello
urlscan
vhost
virustotal
zoomeye
```

Once the file is created, we will execute the following commands to gather information from these sources.

## TheHarvester
```bash
cat sources.txt | while read source; do theHarvester -d "${TARGET}" -b 
$source -f "${source}_${TARGET}";done
```

When the process finishes, we can extract all the subdomains found and sort them via the following command.

```bash
cat *.json | jq -r '.hosts[]' 2>/dev/null | cut -d':' -f 1 | sort -u > "${TARGET}_theHarvester.txt"
```
Now we can merge all the passive reconnaissance files via.

```bash
cat facebook.com_*.txt | sort -u > facebook.com_subdomains_passive.txt
cat facebook.com_subdomains_passive.txt | wc -l11947
```