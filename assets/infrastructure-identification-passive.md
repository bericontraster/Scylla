# Infrastructure Identification
`The Guide`

[Netcraft](https://www.netcraft.com/) can offer us information about the servers without even interacting with them, and this is something valuable from a passive information gathering point of view. We can use the service by visiting `https://sitereport.netcraft.com` and entering the target domain.

Some interesting details we can observe from the report are

| Name  | Explain  |
|:------|----------|
| `Background` | General information about the domain, including the date it was first seen by Netcraft crawlers. |
| `Network` | Information about the netblock owner, hosting company, nameservers, etc. | 
| `Hosting history` | Latest IPs used, webserver, and target OS. |

We need to pay special attention to the latest IPs used. Sometimes we can spot the actual IP address from the webserver before it was placed behind a load balancer, web application firewall, or IDS, allowing us to connect directly to it if the configuration allows it. This kind of technology could interfere with or alter our future testing activities.