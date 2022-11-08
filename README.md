```
usuario@server-dhcp:~$ sudo -s
[sudo] password for usuario: 
root@server-dhcp:/home/usuario# 
```

```
root@server-dhcp:/home/usuario# vi /etc/netplan/00-installer-config.yaml 
```

```
# This is the network config written by 'subiquity'
network:
  ethernets:
    enp0s3:
      dhcp4: true
      dhcp-identifier: mac
    enp0s8:
      addresses: [192.168.27.5/24]
      nameservers:
        addresses: [192.168.27.5]
      gateway4: 192.168.27.1
  version: 2
```

```
root@server-dhcp:/home/usuario# netplan apply
```

```
root@server-dhcp:/home/usuario# apt update && apt install isc-dhcp-server --yes
```

```
root@server-dhcp:/home/usuario# systemctl status isc-dhcp-server
× isc-dhcp-server.service - ISC DHCP IPv4 server
     Loaded: loaded (/lib/systemd/system/isc-dhcp-server.service; enabled; vend>
     Active: failed (Result: exit-code) since Tue 2022-11-08 13:00:00 CET; 6min>
       Docs: man:dhcpd(8)
   Main PID: 617 (code=exited, status=1/FAILURE)
        CPU: 13ms

nov 08 12:59:59 server-dhcp dhcpd[617]: 
nov 08 12:59:59 server-dhcp dhcpd[617]: Not configured to listen on any interfa>
nov 08 12:59:59 server-dhcp dhcpd[617]: 
nov 08 12:59:59 server-dhcp dhcpd[617]: If you think you have received this mes>
nov 08 12:59:59 server-dhcp dhcpd[617]: than a configuration issue please read >
nov 08 12:59:59 server-dhcp dhcpd[617]: bugs on either our web page at www.isc.>
nov 08 12:59:59 server-dhcp dhcpd[617]: before submitting a bug.  These pages e>
nov 08 12:59:59 server-dhcp dhcpd[617]: process and the information we find hel>
nov 08 12:59:59 server-dhcp dhcpd[617]: 
nov 08 12:59:59 server-dhcp dhcpd[617]: exiting.
lines 1-17/17 (END)
root@server-dhcp:/home/usuario# 
```

```
root@server-dhcp:/home/usuario# vi /etc/dhcp/dhcpd.conf
```

```
aaa
```

```
aaa
```

```
aaa
```

```
aaa
```

```
aaa
```

```
aaa
```