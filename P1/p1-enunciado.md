# PRÁCTICA 1 - CONFIGURACIÓN BÁSICA (SOLUCIÓN)

En esta segunda parte se muestran las soluciones a la primera práctica. En [`p1-sesiones`](./p1-sesiones.md) se mostrarán los apuntes tomados durante las sesiones.

## PARTE 1

### Apartado A

Configure su máquina virtual de laboratorio con los datos proporcionados por el profesor.
Analice los ficheros básicos de configuración (interfaces, hosts, resolv.conf,
nsswitch.conf, sources.list, etc.)

`/etc/network/interfaces`:

```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).
#source /etc/network/interfaces.d/*
# The loopback network interface
auto lo ens33
iface lo inet loopback

iface ens33 inet static
	address 10.11.48.50
	netmask 255.255.254.0
	broadcast 10.11.49.255
	network 10.11.48.0
	gateway 10.11.48.1
```

`/etc/hosts`:

```
127.0.0.1	localhost
10.11.48.50	debian

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

`/etc/resolv.conf`:

```
domain udc.pri
search udc.pri
nameserver 10.8.12.49
nameserver 10.8.12.50
nameserver 10.8.12.47
```

`/etc/nsswitch.conf`:

```
# /etc/nsswitch.conf
#
# Example configuration of GNU Name Service Switch functionality.
# If you have the `glibc-doc-reference' and `info' packages installed, try:
# `info libc "Name Service Switch"' for information about this file.

passwd:         files systemd
group:          files systemd
shadow:         files
gshadow:        files

hosts:          files mdns4_minimal [NOTFOUND=return] dns myhostname
networks:       files

protocols:      db files
services:       db files
ethers:         db files
rpc:            db files

netgroup:       nis
```

`/etc/apt/sources.list`:

```
deb http://deb.debian.org/debian/ buster main
deb-src http://deb.debian.org/debian/ buster main

deb https://deb.debian.org/debian-security buster-security main contrib
deb-src https://deb.debian.org/debian-security buster-security main contrib

deb http://security.debian.org/debian-security buster/updates main
deb-src http://security.debian.org/debian-security buster/updates main
```

### Apartado B

¿Qué distro y versión tiene la máquina inicialmente entregada?. Actualice su máquina a la última
versión estable disponible.

Mostramos la distro actual y su versión:

```console
root@debian:/home/lsi# lsb_release -a
No LSB modules are available.
Distributor ID:	Debian
Description:	Debian GNU/Linux 10 (buster)
Release:	10
Codename:	buster
```

Para actualizar (desde el usuario `root`):

1. `apt update -y && sudo apt upgrade -y`

> La opción `-y` acepta todos los prompts.

2. `apt dist-upgrade`

3. Reemplazar la **lista de fuentes**  `/etc/apt/sources.list` con:

```
deb http://deb.debian.org/debian/ bullseye main
deb-src http://deb.debian.org/debian/ bullseye main

deb https://deb.debian.org/debian-security bullseye-security main contrib
deb-src https://deb.debian.org/debian-security bullseye-security main contrib

deb http://deb.debian.org/debian/ bullseye-updates main contrib
deb-src http://deb.debian.org/debian/ bullseye-updates main contrib
```

4. `apt update`

5. `apt upgrade --without-new-pkgs`

6. `apt full-upgrade`

7. `reboot` 

8. Comprobamos que la máquina se actualizó correctamente: 

```console
lsi@debian:~$ lsb_release -a
No LSB modules are available.
Distributor ID: Debian
Description: Debian GNU/Linux 11 (bullseye)
Release: 11
Codename: bullseye
```

### Apartado C

Identifique la secuencia completa de arranque de una máquina basada en la distribución de
referencia (desde la pulsación del botón de arranque hasta la pantalla de login). ¿Qué target por defecto tiene su máquina? ¿Cómo podría cambiar el target de arranque? ¿Qué targets tiene su sistema y en qué estado se encuentran? ¿Y los services? Obtenga la relación de servicios de su sistema y su estado. ¿Qué otro tipo de unidades existen?


1. `systemctl list-dependencies default.target`

2. Target por defecto: `graphical.target`

3. Para cambiarlo: `systemctl set-default TARGET`

4. Targets del sistema y su estado: `systemctl list-units --type target --all`

```
UNIT FILE                     STATE    VENDOR PRESET
basic.target                  static   -
blockdev@.target              static   -
bluetooth.target              static   -
...
...
...
umount.target                 static   -
usb-gadget.target             static   -

67 unit files listed.
```

5. Servicios: `systemctl list-unit-files --type=service`

```
UNIT FILE                              STATE           VENDOR PRESET
accounts-daemon.service                masked          enabled
alsa-restore.service                   static          -
alsa-state.service                     static          -
alsa-utils.service                     masked          enabled
anacron.service                        enabled         enabled
apparmor.service                       enabled         enabled
apt-daily-upgrade.service              static          -
apt-daily.service                      static          -
autovt@.service                        alias           -
...
...
...
wacom-inputattach@.service             static          -
wpa_supplicant-nl80211@.service        disabled        enabled
wpa_supplicant-wired@.service          disabled        enabled
wpa_supplicant.service                 disabled        enabled
wpa_supplicant@.service                disabled        enabled
x11-common.service                     masked          enabled

173 unit files listed.
```

### Apartado D

Determine los tiempos aproximados de botado de su kernel y del userspace. Obtenga la relación
de los tiempos de ejecución de los services de su sistema.

### Apartado E

Investigue si alguno de los servicios del sistema falla. Pruebe algunas de las opciones del sistema
de registro journald. Obtenga toda la información journald referente al proceso de botado de la
máquina. ¿Qué hace el systemd-timesyncd?

### Apartado F

Identifique y cambie los principales parámetros de su segundo interface de red (ens34).
Configure un segundo interface lógico. Al terminar, déjelo como estaba.

### Apartado G

¿Qué rutas (routing) están definidas en su sistema?. Incluya una nueva ruta estática a una
determinada red.

### Apartado H

En el apartado d) se ha familiarizado con los services que corren en su sistema. ¿Son necesarios
todos ellos?. Si identifica servicios no necesarios, proceda adecuadamente. Una limpieza no le
vendrá mal a su equipo, tanto desde el punto de vista de la seguridad, como del rendimiento.

### Apartado I

Diseñe y configure un pequeño “script” y defina la correspondiente unidad de tipo service para
que se ejecute en el proceso de botado de su máquina.

### Apartado J

Identifique las conexiones de red abiertas a y desde su equipo.

### Apartado K

Nuestro sistema es el encargado de gestionar la CPU, memoria, red, etc., como soporte a los datos
y procesos. Monitorice en “tiempo real” la información relevante de los procesos del sistema y
los recursos consumidos. Monitorice en “tiempo real” las conexiones de su sistema.

### Apartado L

Un primer nivel de filtrado de servicios los constituyen los tcp-wrappers. Configure el tcp-
wrapper de su sistema (basado en los ficheros hosts.allow y hosts.deny) para permitir
conexiones SSH a un determinado conjunto de IPs y denegar al resto. ¿Qué política general de
filtrado ha aplicado?. ¿Es lo mismo el tcp-wrapper que un firewall?. Procure en este proceso no
perder conectividad con su máquina. No se olvide que trabaja contra ella en remoto por ssh.

### Apartado M

Existen múltiples paquetes para la gestión de logs (syslog, syslog-ng, rsyslog). Utilizando el
rsyslog pruebe su sistema de log local.

### Apartado N

Configure IPv6 6to4 y pruebe ping6 y ssh sobre dicho protocolo. ¿Qué hace su tcp-wrapper en
las conexiones ssh en IPv6? Modifique su tcp-wapper siguiendo el criterio del apartado h).
¿Necesita IPv6?. ¿Cómo se deshabilita IPv6 en su equipo?

--- 

## PARTE 2

### Apartado A

En colaboración con otro alumno de prácticas, configure un servidor y un cliente NTP.

### Apartado B

Cruzando los dos equipos anteriores, configure con rsyslog un servidor y un cliente de logs.

### Apartado C

Haga todo tipo de propuestas sobre los siguientes aspectos: ¿Qué problemas de seguridad identifica en los dos apartados anteriores? ¿Cómo podría solucionar los problemas identificados?

### Apartado D

En la plataforma de virtualización corren, entre otrs equipos, más de 200 máquinas virtuales para LSI. Como los recursos son limitados, y el disco duro también, identifique todas aquellas acciones que pueda hacer para reducir el espacio de disco ocupado.
