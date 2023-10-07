## Virtual Hosts
`The Guide`

- [Curl](#Curl)
- [FFuF](#FFuF)

## Curl

```bash
curl -s http://192.168.10.10
```
```bash
curl -s http://192.168.10.10 -H "Host: randomtarget.com"
```
We can also automate this by using dictionary file of possible 
vhost `/SecLists/Discovery/DNS/namelist.txt` wordlist. <br>
`vHost Fuzzing`
```bash
cat ./vhosts | while read vhost;do echo "\n********\nFUZZING: ${vhost}\n********";curl -s -I http://192.168.10.10 -H "HOST: ${vhost}.randomtarget.com" | grep "Content-Length: ";done********
```
Accessing indentified vhosts.
```bash
curl -s http://192.168.10.10 -H "Host: dev-admin.randomtarget.com"
```

## FFuF
vHost fuzzing using ffuf.
```bash
ffuf -w ./vhosts -u http://192.168.10.10 -H "HOST: FUZZ.randomtarget.com" -fs 612
```
| CMD | EXPLAIN |
|:----|---------|
| `-w` | Path to our wordlist. |
| `-u` | URL we want to fuzz. |
| `-H` | "HOST: FUZZ.randomtarget.com": This is the `HOST` Header, and the word `FUZZ` will be used as the fuzzing point. |
| `-fs 612` | Filter responses with a size of 612, default response size in this case. |

