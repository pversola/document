## Alpine Install Network Tools

- telnet：`busybox-extras`
- net-tools: `net-tools`
- tcpdump: `tcpdump`
- wget: `wget`
- dig nslookup: `bind-tools`
- curl: `curl`
- nmap: `nmap`
- wget ifconfig nc traceroute.. : `busybox`
- ssh: `openssh-client`
- ss iptables: `iproute2`
- ethtool: `ethtool`

```
docker exec -it <CONTAINER NAME> apk update
docker exec -it <CONTAINER NAME> apk add iputils busybox-extras
```

```
FROM  alpine
MAINTAINER
RUN sed -i 's@http://dl-cdn.alpinelinux.org/@https://mirrors.aliyun.com/@g' /etc/apk/repositories
RUN apk add --no-cache --virtual .persistent-deps \
                curl \
                tcpdump \
                iproute2 \
                bind-tools \
                ethtool \
                busybox-extras \
                libressl \
                openssh-client \
                busybox
CMD  [ "tail", "-f" ]
```
