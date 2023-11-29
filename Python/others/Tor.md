---
date: 2022-10-01 14:47:53.461125
---
# Introduction

Tor(The Onion Router)

# install

```bash
sudo apt-get install tor
sudo vi /etc/tor/torrc 
## add config
	Socks5Proxy 127.0.0.1:1086
## 
sudo apt-get install privoxy
sudo vi /etc/privoxy/config
## add config
	forward-socks5   /   127.0.0.1:9050 .
## 
sudo service privoxy start
sudo service tor start
```



