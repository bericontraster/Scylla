# Crawling
`The Guide`

- [ZAP](#zap)
- [FFuF](#ffuf)
- [Sensitive Information Disclosure](#sensitive-information-disclosure)
  - [Cewl](#cewl)

# ZAP
**COMING SOON**

# FFuF
Web crawling with ffuf.
```bash
ffuf -recursion -recursion-depth 1 -u http://192.168.10.10/FUZZ -w /opt/useful/SecLists/Discovery/Web-Content/raft-small-directories-lowercase.txt
```
| CMD | EXPLAIN |
|:----|---------|
| `-recursion` | Activates the recursive scan. |
| `-recursion-depth` | Specifies the maximum depth to scan. |
| `-u` |  Our target URL, and `FUZZ` will be the injection point. |
| `-w` | Path to our wordlist. |

## Sensitive Information Disclosure

### CeWL
Extracting keywords from website using [CeWL](https://github.com/digininja/CeWL).
```bash
cewl -m5 --lowercase -w wordlist.txt http://192.168.10.10
```

## FFuF
we will use the following parameters in `ffuf`:

- `-w`: We separate the wordlists by coma and add an alias to them to inject them as fuzzing points later
- `-u`: Our target URL with the fuzzing points.
```bash
ffuf -w ./folders.txt:FOLDERS,./wordlist.txt:WORDLIST,./extensions.txt:EXTENSIONS -u http://192.168.10.10/FOLDERS/WORDLISTEXTENSIONS
```