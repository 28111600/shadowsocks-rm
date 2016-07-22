## supervisor

> sudo apt-get install supervisor
> sudo nano /etc/supervisor/conf.d/shadowsocks.conf

````
[program:shadowsocks]
command=python /root/shadowsocks/shadowsocks/servers.py
user=root
autostart = true
autoresart = true
stderr_logfile = /var/log/shadowsocks.log
stdout_logfile = /var/log/shadowsocks.log
stderr_logfile_maxbytes=4MB
stderr_logfile_backups=10
startsecs=3

````
> sudo nano /etc/default/supervisor

````
ulimit -n 51200
ulimit -Sn 4096
ulimit -Hn 8192
````

> sudo service supervisor restart

## fail2ban
> sudo nano /etc/fail2ban/filter.d/shadowsocks.conf

````
[INCLUDES]
before = common.conf

[Definition]
_daemon = shadowsocks
failregex = ^\s+ERROR\s+can not parse header when handling connection from <HOST>:\d+$
ignoreregex =
````


> sudo nano /etc/fail2ban/jail.local

```
[shadowsocks]
enabled = true
filter = shadowsocks
port = 5000:10000
logpath = /var/log/shadowsocks.log
maxretry = 5
bantime = 43200
```
> sudo service fail2ban restart

> sudo fail2ban-client status shadowsocks
