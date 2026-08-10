---
description: Kiểm tra subdomain thật sự resolve (có ip)
---

# DNS

## dnsx

```bash
dnsx -l all_subs.txt -silent -o resolved.txt
```



Tìm các bản ghi&#x20;

```bash
dnsx -l all_subs.txt -silent \
     -a -aaaa -cname -mx -ns -txt \
     -resp \
     -o all_dns_records.txt
```



