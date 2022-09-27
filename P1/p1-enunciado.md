# PRÁCTICA 1 - CONFIGURACIÓN BÁSICA (SOLUCIÓN)

En esta segunda parte se muestran las soluciones a la primera práctica. En [`p1-sesiones`](./p1-sesiones.md) se mostrarán los apuntes tomados durante las sesiones.

## PARTE 1

### Apartado A

Configure su máquina virtual de laboratorio con los datos proporcionados por el profesor.
Analice los ficheros básicos de configuración (interfaces, hosts, resolv.conf,
nsswitch.conf, sources.list, etc.)

1. Modificar el fichero `/etc/network/interfaces`:

> Mis IPs son `10.11.48.50` y `10.11.50.50`. La configuración de este fichero dependerá de tus IPs.

```
# This file describes the network interfaces available on your system
# and how to activate them. For more information, see interfaces(5).
#source /etc/network/interfaces.d/*
# The loopback network interface
auto lo ens33 ens34
iface lo inet loopback

iface ens33 inet static
	address 10.11.48.50
	netmask 255.255.254.0
	broadcast 10.11.49.255
	network 10.11.48.0
	gateway 10.11.48.1

iface ens34 inet static
	address 10.11.50.50
	netmask 255.255.254.0
	broadcast 10.11.51.255
	network 10.11.50.0
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

4. Targets del sistema y su estado:

```console
root@debian:/home/lsi# systemctl list-unit-files --type=target
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

5. Servicios:

```console
root@debian:/home/lsi# systemctl list-unit-files --type=service
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

6. Otro tipo de unidades: `systemctl list-units -t help`

```console
root@debian:/home/lsi# systemctl list-units -t help
Available unit types:
service
mount
swap
socket
target
device
automount
timer
path
slice
scope
```

### Apartado D

Determine los tiempos aproximados de botado de su kernel y del userspace. Obtenga la relación
de los tiempos de ejecución de los services de su sistema.

1. Tiempos aproximados de botado de kernel y userspace:

```console
root@debian:/home/lsi# systemd-analyze
Startup finished in 3.544s (kernel) + 34.433s (userspace) = 37.978s
graphical.target reached after 34.368s in userspace
```

2. Relación de tiempos de ejecución y services:

```console
root@debian:/home/lsi# systemd-analyze blame
19.260s plymouth-quit-wait.service
11.564s apparmor.service
 2.505s systemd-udev-settle.service
 2.494s ifupdown-pre.service
 2.448s dev-sda1.device
 2.171s udisks2.service
 1.847s accounts-daemon.service
 1.769s NetworkManager.service
 1.739s polkit.service
 1.718s apt-daily-upgrade.service
 1.435s apt-daily.service
 1.249s avahi-daemon.service
 1.216s switcheroo-control.service
 1.193s systemd-logind.service
 1.179s wpa_supplicant.service
 1.172s user@117.service
 1.143s systemd-random-seed.service
 1.138s user@1000.service
 1.093s networking.service
  613ms systemd-timesyncd.service
  572ms packagekit.service
  563ms systemd-udevd.service
  561ms systemd-journald.service
  529ms fwupd-refresh.service
  514ms systemd-udev-trigger.service
  453ms keyboard-setup.service
  381ms gdm.service
  374ms e2scrub_reap.service
  322ms man-db.service
  296ms ModemManager.service
  229ms systemd-modules-load.service
  222ms rsyslog.service
  197ms logrotate.service
  152ms upower.service
  147ms systemd-tmpfiles-setup.service
  133ms colord.service
  129ms systemd-journal-flush.service
  126ms plymouth-start.service
  114ms dev-mqueue.mount
  110ms dev-hugepages.mount
  109ms systemd-tmpfiles-setup-dev.service
  109ms sys-kernel-debug.mount
  108ms sys-kernel-tracing.mount
   97ms systemd-sysusers.service
   90ms systemd-sysctl.service
   76ms systemd-remount-fs.service
   62ms modprobe@fuse.service
   59ms user-runtime-dir@1000.service
   54ms run-vmblock\x2dfuse.mount
   49ms dev-sda5.swap
   47ms plymouth-read-write.service
   45ms systemd-user-sessions.service
   45ms systemd-update-utmp-runlevel.service
   41ms modprobe@drm.service
```

### Apartado E

Investigue si alguno de los servicios del sistema falla. Pruebe algunas de las opciones del sistema
de registro journald. Obtenga toda la información journald referente al proceso de botado de la
máquina. ¿Qué hace el systemd-timesyncd?

1. Comprobar si algún servicio ha fallado:

```console
root@debian:/home/lsi# systemctl list-unit-files --type=service --failed
UNIT FILE STATE VENDOR PRESET

0 unit files listed.
```

2. `journalctl -u SERVICE` muestra el log de un servicio.

```console
root@debian:/home/lsi# journalctl -u networking.service
-- Journal begins at Wed 2022-09-07 12:23:14 CEST, ends at Tue 2022-09-27 20:32:37 CEST. --
sep 07 12:23:17 debian systemd[1]: Starting Raise network interfaces...
sep 07 12:23:19 debian systemd[1]: Started Raise network interfaces.
sep 11 19:53:22 debian systemd[1]: Stopping Raise network interfaces...
sep 11 19:53:23 debian systemd[1]: networking.service: Succeeded.
sep 11 19:53:23 debian systemd[1]: Stopped Raise network interfaces.
-- Boot bf9029a9f3c84a03ae6c3f67eb80f1b0 --
sep 11 19:53:47 debian systemd[1]: Starting Raise network interfaces...
sep 11 19:53:49 debian systemd[1]: Finished Raise network interfaces.
sep 20 17:23:22 debian systemd[1]: Stopping Raise network interfaces...
sep 20 17:23:23 debian systemd[1]: networking.service: Succeeded.
...
...
sep 27 16:05:30 debian systemd[1]: Stopped Raise network interfaces.
-- Boot ea5ceb8c885f469aa1d8987cf1fcd24c --
sep 27 16:05:48 debian systemd[1]: Starting Raise network interfaces...
sep 27 16:05:49 debian systemd[1]: Finished Raise network interfaces.
sep 27 17:10:55 debian systemd[1]: Stopping Raise network interfaces...
sep 27 17:10:55 debian systemd[1]: networking.service: Succeeded.
sep 27 17:10:55 debian systemd[1]: Stopped Raise network interfaces.
-- Boot dfaedc0ebc374a848a16d5e14e110108 --
sep 27 17:11:14 debian systemd[1]: Starting Raise network interfaces...
sep 27 17:11:15 debian systemd[1]: Finished Raise network interfaces.
```

3. `journalctl -b` muestra el log del boot actual.

4. `systemd-timesyncd` es un servicio del sistema que se usa para sincronizar el reloj local del sistema con un servidor NTP remoto.

### Apartado F

Identifique y cambie los principales parámetros de su segundo interface de red (ens34).
Configure un segundo interface lógico. Al terminar, déjelo como estaba.

1. Estado inicial de `ens34`:

```console
root@debian:/home/lsi# ifconfig ens34
ens34: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 10.11.50.50  netmask 255.255.254.0  broadcast 10.11.51.255
        inet6 fe80::250:56ff:fe97:cee4  prefixlen 64  scopeid 0x20<link>
        ether 00:50:56:97:ce:e4  txqueuelen 1000  (Ethernet)
        RX packets 6843  bytes 1563475 (1.4 MiB)
        RX errors 0  dropped 2420  overruns 0  frame 0
        TX packets 19  bytes 1426 (1.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 16  base 0x2080  

```

2. Configuración del segundo interface de red (`ens34`):

```console
root@debian:/home/lsi# ifconfig ens34 down
root@debian:/home/lsi# ifconfig ens34 mtu 1200
root@debian:/home/lsi# ifconfig ens34 hw ether 00:1e:2e:b5:18:07
root@debian:/home/lsi# ifconfig ens34 10.11.50.51 netmask 255.255.254.0
root@debian:/home/lsi# ifconfig ens34 up
root@debian:/home/lsi# ifconfig ens34
ens34: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1200
        inet 10.11.50.51  netmask 255.255.254.0  broadcast 10.11.51.255
        ether 00:1e:2e:b5:18:07  txqueuelen 1000  (Ethernet)
        RX packets 6867  bytes 1568069 (1.4 MiB)
        RX errors 0  dropped 2431  overruns 0  frame 0
        TX packets 19  bytes 1426 (1.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 16  base 0x2080
```

3. Configuración de una interfaz lógica:

```console
root@debian:/home/lsi# ifconfig ens34:1 192.168.1.1 netmask 255.255.255.0
root@debian:/home/lsi# ifconfig ens34:1 up
root@debian:/home/lsi# ifconfig
ens34: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1200
        inet 10.11.50.51  netmask 255.255.254.0  broadcast 10.11.51.255
        ether 00:1e:2e:b5:18:07  txqueuelen 1000  (Ethernet)
        RX packets 6898  bytes 1574835 (1.5 MiB)
        RX errors 0  dropped 2444  overruns 0  frame 0
        TX packets 19  bytes 1426 (1.3 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 16  base 0x2080  

ens34:1: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1200
        inet 192.168.1.1  netmask 255.255.255.0  broadcast 192.168.1.255
        ether 00:1e:2e:b5:18:07  txqueuelen 1000  (Ethernet)
        device interrupt 16  base 0x2080
```

- Como no hemos hecho nada persistente, si queremos dejar todo como estaba simplemente hacemos `reboot`. Para persistir los cambios debemos hacerlos en `/etc/network/interfaces`.

### Apartado G

¿Qué rutas (routing) están definidas en su sistema?. Incluya una nueva ruta estática a una
determinada red.

1. Rutas definidas en el sistema:

```console
root@debian:/home/lsi# ip route show
default via 10.11.48.1 dev ens33 onlink 
10.11.48.0/23 dev ens33 proto kernel scope link src 10.11.48.50 
10.11.50.0/23 dev ens34 proto kernel scope link src 10.11.50.50 
169.254.0.0/16 dev ens33 scope link metric 1000
```

```console
root@debian:/home/lsi# route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         _gateway        0.0.0.0         UG    0      0        0 ens33
10.11.48.0      0.0.0.0         255.255.254.0   U     0      0        0 ens33
10.11.50.0      0.0.0.0         255.255.254.0   U     0      0        0 ens34
link-local      0.0.0.0         255.255.0.0     U     1000   0        0 ens33
```

2. Nueva ruta: `ip route add 10.11.52.0/24 via 10.11.48.1`

```console
root@debian:/home/lsi# ip route show
default via 10.11.48.1 dev ens33 onlink 
10.11.48.0/23 dev ens33 proto kernel scope link src 10.11.48.50 
10.11.50.0/23 dev ens34 proto kernel scope link src 10.11.50.50 
10.11.52.0/24 via 10.11.48.1 dev ens33 
169.254.0.0/16 dev ens33 scope link metric 1000
```

```console
root@debian:/home/lsi# route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         _gateway        0.0.0.0         UG    0      0        0 ens33
10.11.48.0      0.0.0.0         255.255.254.0   U     0      0        0 ens33
10.11.50.0      0.0.0.0         255.255.254.0   U     0      0        0 ens34
10.11.52.0      _gateway        255.255.255.0   UG    0      0        0 ens33
link-local      0.0.0.0         255.255.0.0     U     1000   0        0 ens33
```

### Apartado H

En el apartado d) se ha familiarizado con los services que corren en su sistema. ¿Son necesarios
todos ellos?. Si identifica servicios no necesarios, proceda adecuadamente. Una limpieza no le
vendrá mal a su equipo, tanto desde el punto de vista de la seguridad, como del rendimiento.

Se han identificado varios services no necesarios, tanto en el inicio, como en la máquina en sí,
por sus características de uso por ssh.
- `systemctl mask avahi-daemon.socket`
- `systemctl mask avahi-daemon.service`
- `systemctl disable NetworkManager.service`
- `systemctl mask cups`: los servicios para la impresión no serán necesarios
- `systemctl mask cups-browsed.service`
- `systemctl mask bluetooth`: la máquina virtual no usará bluetooth
- `systemctl disable accounts-daemon.service`: si es necesario, GNOME lo iniciará
- `systemctl disable cron.service`: se podrá iniciar después. Por otra parte, `anacron.service` no se salta los eventos programados, aunque la máquina se apague 


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
