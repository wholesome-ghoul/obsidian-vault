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
# mem info
sudo lshw -c memory

# ls open files
lsof -t -i:3000

# get info about pid
ps -p (lsof -t -i:50053) -o pid,vsz=MEMORY -o user,group=GROUP -o comm,args=ARGS

# cuda version
nvcc --version

# nvidia gpu watch
watch -n 1 nvidia-smi
watch -n 1 nvidia-smi --query-gpu=timestamp,pstate,temperature.gpu,utilization.gpu,utilization.memory,memory.total,memory.free,memory.used --format=csv

# terminal size
stty size

# iproute2 contains ss utility
sudo apt install iproute2 netcat-openbsd socat

# fork ensures that socat continues to run after it handles a connection
socat TCP4-LISTEN:8080,fork /dev/null&
socat TCP6-LISTEN:8080,ipv6only=1,fork /dev/null&

# examine IPv4 sockets
# tcp only, listening sockets only, port numbers instead of service names
ss -4 -tln

# -z flag ensures to connect to a socket without sending any data
nc -4 -vz 127.0.0.1 8080
nc -6 -vz ::1 8080

# run fg command for each socat process we've created

sudo socat UDP4-LISTEN:8080,fork /dev/null&
sudo socat UDP6-LISTEN:8080,ipv6only=1,fork /dev/null&
ss -4 -uln
ss -6 -uln
nc -4 -u -vz 127.0.0.1 123
nc -6 -u -vz ::1 123

socat unix-listen:/tmp/stream.sock,fork /dev/null&
socat unix-recvfrom:/tmp/datagram.sock,fork /dev/null&
ss -xln
stat /tmp/stream.sock /tmp/datagram.sock
nc -U -z /tmp/stream.sock
nc -uU -z /tmp/datagram.sock

# show current keyboard layout gui
gkbd-keyboard-display -l <layout>
```
