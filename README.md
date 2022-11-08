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

# Dentro del VI ejecutar el comando **1000dd** para borrar todas las lineas y copiar y pegar esta

```
ddns-update-style none;
subnet 192.168.27.0 netmask 255.255.255.0 {
        # default gateway
        option routers 192.168.27.1;
        option subnet-mask 255.255.255.0;

        # DNS
        option domain-name "domain.org";
        option domain-name-servers 192.168.27.5;

        # Pool
        allow unknown-clients; 
        range 192.168.27.10 192.168.27.200;

        default-lease-time 21600;
        max-lease-time 43200;
}
```

# Ya esta funcionando el DHCP con la configuracion que le hemos puesto.

```
root@server-dhcp:/home/usuario# systemctl restart isc-dhcp-server
root@server-dhcp:/home/usuario# systemctl status isc-dhcp-server
● isc-dhcp-server.service - ISC DHCP IPv4 server
     Loaded: loaded (/lib/systemd/system/isc-dhcp-server.service; enabled; vendor preset: enabled)
     Active: active (running) since Tue 2022-11-08 13:32:17 CET; 936ms ago
       Docs: man:dhcpd(8)
   Main PID: 1566 (dhcpd)
      Tasks: 4 (limit: 1034)
     Memory: 4.5M
        CPU: 22ms
     CGroup: /system.slice/isc-dhcp-server.service
             └─1566 dhcpd -user dhcpd -group dhcpd -f -4 -pf /run/dhcp-server/dhcpd.pid -cf /etc/dhcp/dhcpd.conf

nov 08 13:32:17 server-dhcp dhcpd[1566]:    you want, please write a subnet declaration
nov 08 13:32:17 server-dhcp sh[1566]:    you want, please write a subnet declaration
nov 08 13:32:17 server-dhcp dhcpd[1566]:    in your dhcpd.conf file for the network segment
nov 08 13:32:17 server-dhcp sh[1566]:    in your dhcpd.conf file for the network segment
nov 08 13:32:17 server-dhcp dhcpd[1566]:    to which interface enp0s3 is attached. **
nov 08 13:32:17 server-dhcp sh[1566]:    to which interface enp0s3 is attached. **
nov 08 13:32:17 server-dhcp dhcpd[1566]: 
nov 08 13:32:17 server-dhcp dhcpd[1566]: Sending on   Socket/fallback/fallback-net
nov 08 13:32:17 server-dhcp sh[1566]: Sending on   Socket/fallback/fallback-net
nov 08 13:32:17 server-dhcp dhcpd[1566]: Server starting service.
root@server-dhcp:/home/usuario# 
```

-----------------

# Aqui esta la perte de cliente
```
usuario@cliente:~$ sudo -s
[sudo] password for usuario: 
root@cliente:/home/usuario# vi /etc/netplan/00-installer-config.yaml 
```

# Configuracion del netplan

```
# This is the network config written by 'subiquity'
network:
  ethernets:
    enp0s3:
      dhcp4: true
      dhcp-identifier: mac
    enp0s8:
      dhcp4: true
      dhcp-identifier: mac
  version: 2

```

# Se aplica el netplan y miramos el estado de la interfaz

```
root@cliente:/home/usuario# netplan apply
root@cliente:/home/usuario# networkctl status enp0s8
● 3: enp0s8                                                                                      
                     Link File: /usr/lib/systemd/network/99-default.link
                  Network File: /run/systemd/network/10-netplan-enp0s8.network
                          Type: ether
                         State: routable (configured)
                  Online state: online                                                           
                          Path: pci-0000:00:08.0
                        Driver: e1000
                        Vendor: Intel Corporation
                         Model: 82540EM Gigabit Ethernet Controller (PRO/1000 MT Desktop Adapter)
                    HW Address: 08:00:27:6e:46:7e (PCS Systemtechnik GmbH)
                           MTU: 1500 (min: 46, max: 16110)
                         QDisc: fq_codel
  IPv6 Address Generation Mode: eui64
          Queue Length (Tx/Rx): 1/1
              Auto negotiation: yes
                         Speed: 1Gbps
                        Duplex: full
                          Port: tp
                       Address: 192.168.27.10 (DHCP4 via 192.168.27.5)
                                fe80::a00:27ff:fe6e:467e
                       Gateway: 192.168.27.1
                           DNS: 192.168.27.5
                Search Domains: domain.org
             Activation Policy: up
           Required For Online: yes
               DHCP4 Client ID: 08:00:27:6e:46:7e
             DHCP6 Client DUID: DUID-EN/Vendor:0000ab11e93fad0925a6f8690000

nov 08 13:45:46 cliente systemd-networkd[473]: enp0s8: Re-configuring with /run/systemd/network/10-netplan-enp0s8.network
nov 08 13:45:46 cliente systemd-networkd[473]: enp0s8: Re-configuring with /run/systemd/network/10-netplan-enp0s8.network
nov 08 13:45:46 cliente systemd-networkd[473]: enp0s8: DHCPv6 lease lost
nov 08 13:45:46 cliente systemd-networkd[473]: enp0s8: Link UP
nov 08 13:45:46 cliente systemd-networkd[473]: enp0s8: Gained carrier
nov 08 13:45:47 cliente systemd-networkd[473]: enp0s8: DHCPv4 address 192.168.27.10/24 via 192.168.27.1
nov 08 13:45:48 cliente systemd-networkd[473]: enp0s8: Gained IPv6LL
root@cliente:/home/usuario# 
```
