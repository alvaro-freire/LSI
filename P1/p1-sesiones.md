# PRÁCTICA 1 - CONFIGURACIÓN BÁSICA (3 SESIONES - 6H)

> ip: `10.11.48.50`, máquina: `1.2.10-48.50`, ip compañero -> `10.11.49.106`

En esta primera parte se muestran los apuntes de las 3 sesiones. En [`p1-enunciado`](./p1-enunciado.md) se mostrarán los apartados con sus soluciones.

## SESIÓN 0

`su`: access to super user

`passwd`: change password

#### Directorios importantes:

* `/etc/resolv.config`

* `/etc/network/interfaces`

* `/etc/hosts (change debian ip)`

#### Detener y deshabilitar los siguientes servicios:

* `systemctl stop avahi.daemon.socket` y `systemctl disable avahi.daemon.socket`
* `systemctl stop avahi-daemon.service` y `systemctl disable avahi.daemon.service`
* `systemctl stop NetworkManager.service` y `systemctl disable NetworkManager.service`

---

`ps -eaf`: 

  * Muestra información de los procesos activos. `-e`muestra todos los procesos, `a` los procesos asociados a un TTY (Terminal), `-f` muestra el formato largo.

`systemctl --all`

  * Muestra información sobre todos las unidades que tiene systemd en memoria.

---

#### Reiniciar interfaz de red:

* `systemctl restart nerworking.service`

#### Actualizar a Debian 11

* `sudo apt update -y && sudo apt upgrade -y`

> La opción `-y` acepta todos los prompts.

* `sudo apt dist-upgrade`

##### Reemplazar `/etc/apt/sources.list` con:

```bash
deb http://deb.debian.org/debian/ bullseye main
deb-src http://deb.debian.org/debian/ bullseye main
deb https://deb.debian.org/debian-security bullseye-security main contrib
deb-src https://deb.debian.org/debian-security bullseye-security main contrib
deb http://deb.debian.org/debian/ bullseye-updates main contrib
deb-src http://deb.debian.org/debian/ bullseye-updates main contrib
``` 

##### Continuar con los siguientes comandos en orden:

* `sudo apt update`

* `sudo apt upgrade --without-new-pkgs`

> Confirmar los prompts y siempre mantener la versión de configuración actual de los servicios

* `sudo apt full-upgrade`

* `sudo reboot` 

##### Esperar un minuto y comprobar que se actualizó correctamente:

`lsb_release -a`

## SESIÓN 1

`systemctl list-dependencies default.target`: muestra las unidades que dependen del default.target

`systemctl get-default`: muestra el target por defecto

`systemctl list-unit-files --type=service`:  muestra los servicios que hay activados o desactivados actualmente

> `static` -> activo con dependencias. `enable` -> activo sin dependencias

**!! importante saber qué son services y targets**

#### Directorios importantes:

* `/lib/systemd/system/`

* `/etc/systemd/system/`

##### hacer script para boot -> permisos de ejecución!

##### crear miservicio.service en /lib/systemd/system -> importante ejectuarlo lo antes posible por dependencias

`systemctl daemon-reload`: Actualiza los archivos de configuración del systemd (rehace el árbol de dependencias).

`systemd-analyze blame`: Muestra una lista de todas las unidades activas, ordenándolas por el tiempo que les llevó iniciarse.

`systemd-analyze critical-chain`: Muestra un árbol con el tiempo de boot de las principales unidades.

`systemctl --state=failed`: Muestra los servicios que han fallado.

`journalctl -u X.service`: Muestra el log de un servicio.

##### ifconfig + ip (en ns34 mejor), configurar interfaz de red logico (hay que darle todos los datos)

`netstat -nr`: Muestra la tabla de routing

> route add a la ip -> 10.11.52.0 que no existe (añadir una entrada en la tabla de routing)

`netstat -i`: Abre el menú general de netstat

`ss -a`: Muestra todos los sockets TCP/UDP/RAW/UNIX.

`lsof -ni`: Muestra los detalles de las conexiones de red abiertas

##### Decidir qué servicios innecesarios desactivar.

1. `systemctl stop X` (si está activo).

2. `systemctl disable X`.

3. `systemctl mask X` crea un symlink a `/dev/null` para que ningún otro servicio lo pueda rehabilitar.

4. `systemctl unmask X` elimina el symlink a `/dev/null`.

bluetooth, dbus-org.bluez, ModemManager, switcheroo-control, wpa-supplicant
// ? -> cups socket, cups service, cups browsed

`iptraf`: herramienta para monitorizar redes.

## SESIÓN 2

/etc/hosts.deny -> denegar 
all:all  (servicio:máquinas)

mirar -> servicio:máquinas: twist... (o spawn)

/etc/hosts.allow
sshd: 127.0.0.1, 10.11.48.COMP, 10.11.50.COMP 
sshd: rangos eduroam y vpn : spawn echo %a %a >> /home/lsi/logssh

dir ipv6 entre corchetes -> sshd:[::1],[2002:_:_::1]

rango ipv6 udc -> 2001:720:121c:

rango vpn: 10.30.8.X - 10.30.15.X

rando eduroam: conectarse a eduroam, miramos la red, vemos netmask (desde gateway)

rsyslog, syslog 

/etc/rsyslog.conf -> subsistemas 
niveles de gravedad, de - a +: debug, info, notice, warning, err, crit, alert, emerg

para actualizar cambios: systemctl restart rsyslog.service

`#logger -p mail.err "Hola"`
`#tail -f /var/log/mail.log`

apartado O: ipv6
quitar ipv6? (ubuntu y máquina)


montar tunel 6 to 4:
10.11.48.50
2002:a0b:3032:

/etc/network/interfaces
iface 6to4 inet6 v4tunnel
address 2002:a0b:3032:    ::1
netmask 16
endpoint any
local 10.11.48.50

- para quitar ipv6:
/etc/sysctl.conf -> para configurar variables 
sysctl -p (para activar los cambios)
sysctl -a (visualizar)

EN LA PRÁCTICA IPV6 LEVANTADO, 
CONFIGURADO EL TUNEL ETC
SABER QUITAR IPV6

instalar thc-ipv6 (echarle un ojo)

- limpiar disco -> se comprueba espacio con df -H
apt clean
apt autoclean
apt autoremove

borrar man
apt remove --purge man-db

borrar imagenes kernel antiguas
uname -a (para ver kernel actual)
apt --purge autoremove
dpkg --list | grep linux-image <- miras todos los kernels
apt-get --purge remove linux-image-4....... <- elimina un kernel en específico

- sincronizar tiempo con compañero
servidor ntp <-> cliente ntp
docplayer.es/7992340-instalacion-y-configuracion-de-ntp.html

`sudo apt install ntpdate ntp`

servicio -> ntp.service
fich conf -> /etc/ntp.conf:

restrict -4 default ______
restrict -6 _______ ______

comentar pool

server 127.127.1.1
fudge 127.127.1.1 stratum 10
restrict 10.11.49.106 mask 255.255.255.255
restrict 127.127.1.1 mask __________ noquery

restrict ignore es el más restrictivo
noquery no permite consultas


ntpdate a la ip del compañero para comprobar q el servidor está OK

/etc/ntp.conf:

comentar pool

server ipcompañero
...

comprobar que está todo bien:
ntpq -pn
ntpdc -n

el reach se incrementa cada pool segundos (o pull, sabe dios)

- rsyslog

## SESIÓN 3



