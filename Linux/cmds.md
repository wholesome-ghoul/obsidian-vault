---
tags: linux cmd
deck: linux
---

# loop over curl request in bash #card
<!-- 1700192244031 99c6cc910d2c8dcb65a1a5edc973332e -->

```bash
for i in {1..6}; do curl http://localhost:8080;done
```

```bash
taskset --cpu-list 0-2 <cmd>

# gpu info
sudo lshw -c display

# ls open files
lsof -t -i:3000
```
