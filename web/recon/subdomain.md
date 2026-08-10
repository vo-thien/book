---
description: Tìm subdomain
---

# Subdomain

## subfinder

```bash
subfinder -d target.com -all -silent | tee subs1.txt
```

## amass

```bash
aamass enum -passive -d target.com -silent | tee subs2.txt
```



Merge

```bash
cat subs_subfinder.txt subs_amass.txt | sort -u > all_subs.txt
```
