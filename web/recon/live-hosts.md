---
description: Kiểm tra webserver
---

# Live Hosts

## httpx

```bash
~/go/bin/httpx -l all_subs.txt \ 
  -silent \
  -title -td -sc -cl -fr \
  -cdn -favicon -tls-grab \
  -ip -cname -asn \
  -j -o httpx.json
```

