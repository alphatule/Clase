# **Cliente DNS** ~Servidor DNS~

## **1. Revise el fichero /etc/host.conf de su estación. ¿Cómo está configurado el equipo? ¿Qué sentido tiene la sentencia order?** 

### ***RESPUESTA:***

```
smx2a@ada-105:~$ cat /etc/host.conf 
# The "order" line is only used by old versions of the C library.
order hosts,bind
multi on
smx2a@ada-105:~$
```
La orden **order** lo que hace primero de todo es revisar el fichero de *hosts* y si no encuentra nada se va al DNS de *bind*.

---
## **2. Revise el fichero /etc/resolv.conf de su estación. ¿Qué servidor DNS se utiliza? ¿Cuál es el dominio de búsqueda por defecto? ¿Está ejecutando systemd-resolved?** 

### ***RESPUESTA:***
```
smx2a@ada-105:~$ cat /etc/resolv.conf 
# This is /run/systemd/resolve/stub-resolv.conf managed by man:systemd-resolved(8).
# Do not edit.
#
# This file might be symlinked as /etc/resolv.conf. If you're looking at
# /etc/resolv.conf and seeing this text, you have followed the symlink.
#
# This is a dynamic resolv.conf file for connecting local clients to the
# internal DNS stub resolver of systemd-resolved. This file lists all
# configured search domains.
#
# Run "resolvectl status" to see details about the uplink DNS servers
# currently in use.
#
# Third party programs should typically not access this file directly, but only
# through the symlink at /etc/resolv.conf. To manage man:resolv.conf(5) in a
# different way, replace this symlink by a static file or a different symlink.
#
# See man:systemd-resolved.service(8) for details about the supported modes of
# operation for /etc/resolv.conf.

nameserver 127.0.0.53
options edns0 trust-ad
search ada.elpuig.xeill.net
smx2a@ada-105:~$ 
```
El servidor DNS que utiliza actualmente es **127.0.0.53**, el dominio de búsqueda por defecto es **ada.elpuig.xeill.net**. Si, como podemos observar en la primera linea de la consola el comentario nos dice que esta arrancando con **systemd-resolved**.

---
## **3. Utilice el comando netstat para comprobar si el puerto 53 TCP y UDP está abierto por alguna herramienta.** 
```
root@luna:/home/usuario# netstat -putan | grep ":53\ "
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      624/systemd-resolve 
udp        0      0 127.0.0.53:53           0.0.0.0:*                           624/systemd-resolve 
root@luna:/home/usuario#
```

### ***RESPUESTA:***
```
smx2a@ada-105:~$ netstat -putan | grep ":53\ "
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
tcp        0      0 172.16.0.1:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      -                   
tcp        0      0 192.168.122.1:53        0.0.0.0:*               LISTEN      -                   
udp        0      0 172.16.0.1:53           0.0.0.0:*                           -                   
udp        0      0 192.168.122.1:53        0.0.0.0:*                           -                   
udp        0      0 127.0.0.53:53           0.0.0.0:*                           -                   
smx2a@ada-105:~$ 
```
---
## **4. Utilice el comando resolvectl dns para obtener los servidores DNS que utiliza la máquina. ¿Qué servidores son? ¿De dónde los ha obtenido la máquina? ¿Son servidores recursivos o no recursivos?** 

### ***RESPUESTA:***
```
smx2a@ada-105:~$ resolvectl dns
Global:
Link 2 (enp2s0): 192.168.12.10
Link 3 (virbr0):
Link 4 (lxdbr0):
smx2a@ada-105:~$ 
```
En la interfaz de red **enp2s0** utiliza el servidor DNS **192.168.12.10**. El servidor DNS que tiene configurado lo a obtenido mediante una concesion de DHCP y es un servidor no recursivo.

---
## **5. Utilice el comando resolvectl status. ¿Qué información muestra?** 

### ***RESPUESTA:***
```
smx2a@ada-105:~$ resolvectl status
Global
       Protocols: -LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
resolv.conf mode: stub

Link 2 (enp2s0)
    Current Scopes: DNS
         Protocols: +DefaultRoute +LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
Current DNS Server: 192.168.12.10
       DNS Servers: 192.168.12.10
        DNS Domain: ada.elpuig.xeill.net

Link 3 (virbr0)
Current Scopes: none
     Protocols: -DefaultRoute +LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported

Link 4 (lxdbr0)
Current Scopes: none
     Protocols: -DefaultRoute +LLMNR -mDNS -DNSOverTLS DNSSEC=no/unsupported
smx2a@ada-105:~$ 
```
Muestra la información sobre cada interfaz sobre el sistema DNS.

---
## **6. ¿Qué comando muestra información sobre la cache de systemd-resolved? ¿Qué porcentaje de aciertos ha obtenido su máquina?** 

### ***RESPUESTA:***


---
## **7. Utilice el comando host para resolver los siguientes nombres de dominio, utilizando tanto el servidor DNS de la LAN como el servidor DNS 1.1.1.1:** 
- proxy
- valinor.iespuigcastellar.xeill.net
- es.wikipedia.org

### ***RESPUESTA:***
---
## **8. Consulte la página de manual del comando dig. ¿De qué manera se puede especificar el servidor? ¿De qué manera se puede especificar que se realice la consulta utilizando TCP? ¿De qué manera se especifica que se quieren obtener registros de cualquier tipo?** 

### ***RESPUESTA:***
---
## **9. Averigüe qué servidores DNS de Internet son responsables de:** 
- xeill.net
- blog.elpuig.xeill.net
- wikipedia.org
- wiktionary.org
- fsf.org

### ***RESPUESTA:***
---
## **10. Compruebe que los servidores DNS responsables de los dominios indicados resuelven consultas sobre registros que están en su zona de autoridad. Compruebe si también resuelven consultas sobre registros que están fuera de su zona de autoridad. Por tanto, ¿se trata de servidores DNS recursivos?** 

### ***RESPUESTA:***
---
## **11. Realice algunas consultas DNS a 1.1.1.1, 195.88.154.11 y 101.110.32.3. Tome nota del tiempo de ### ***respuesta:***. ¿Qué servidor es más rápido? ¿Dónde se encuentra cada uno de ellos (puede utilizar https://geoiptool.com/es)? ¿Se trata de servidores recursivos?** 

### ***RESPUESTA:***
---
## **12. Los nombres de dominio internacionalizados pueden contener carácteres de diferentes lenguas, pero gracias a la codificación punycode también se pueden representar utilizando una cadena de carácteres ASCII. Resuelva el nombre de dominio xn--j1ay.xn--p1ai. Observe cómo representa el navegador web ese nombre de dominio.** 

### ***RESPUESTA:***
---
## **13. Se desea que el comando ping superproxy permita realizar pings al servidor del aula. ¿Qué se debe hacer? Compruébelo.** 

### ***RESPUESTA:***
---
## **14. ¿Dónde puede obtener las direcciones de los root servers?** 

### ***RESPUESTA:***
---
## **15. Confeccione una lista con algunos DNS recursivos públicos que puedan utilizar los clientes. ¿Quién mantiene estos servidores? ¿Con qué propósito lo hace? ¿Qué características tienen?** 

### ***RESPUESTA:***
---
## **16. Abra una MV de escritorio. Compruebe la diferencia entre navegar utilizando un DNS que responde rápido y uno con alta latencia. Repita el experimento configurando /etc/resolv.conf para que no se utilice systemd-resolved.** 

### ***RESPUESTA:***



    
   
