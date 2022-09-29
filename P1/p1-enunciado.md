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


- **avahi-daemon**: servicio de *autodiscovery* que no necesitamos en nuestra máquina:

```console
root@debian:/home/lsi# systemctl disable avahi-daemon.service
Synchronizing state of avahi-daemon.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install disable avahi-daemon
Removed /etc/systemd/system/sockets.target.wants/avahi-daemon.socket.
Removed /etc/systemd/system/dbus-org.freedesktop.Avahi.service.
Removed /etc/systemd/system/multi-user.target.wants/avahi-daemon.service.

root@debian:/home/lsi# systemctl mask avahi-daemon.service
Created symlink /etc/systemd/system/avahi-daemon.service → /dev/null.
```

- **accounts-daemon**: API para GNOME con las accounts, innecesario ya que solo utilizamos *ssh*:

```console
root@debian:/home/lsi# systemctl disable accounts-daemon.service
Removed /etc/systemd/system/graphical.target.wants/accounts-daemon.service.

root@debian:/home/lsi# systemctl mask accounts-daemon.service
Created symlink /etc/systemd/system/accounts-daemon.service → /dev/null.
```

- **NetworkManager**: No necesario porque usamos `networking.service`, este se encarga de gestionar automáticamente si no lo tenemos en `networking`.

```console
root@debian:/home/lsi# systemctl disable NetworkManager.service
Removed /etc/systemd/system/dbus-org.freedesktop.nm-dispatcher.service.
Removed /etc/systemd/system/network-online.target.wants/NetworkManager-wait-online.service.
Removed /etc/systemd/system/multi-user.target.wants/NetworkManager.service.

root@debian:/home/lsi# systemctl mask NetworkManager.service
Created symlink /etc/systemd/system/NetworkManager.service → /dev/null.
```

Más servicios innecesarios:

- `systemctl disable cups && systemctl mask cups`: los servicios para la impresión no serán necesarios.

- `systemctl disable cups-browsed.service && systemctl mask cups-browsed.service`: impresión.

- `systemctl disable ModemManager.service && systemctl mask ModemManager.service`: servicio para gestión de módem.

- `systemctl disable bluetooth && systemctl mask bluetooth`: la máquina no usa bluetooth.

- `systemctl disable switcheroo-control.service && systemctl mask switcheroo-control.service`: servicio para comprobar disponibilidad de dual-GPU (tarjeta gráfica dual).


### Apartado I

Diseñe y configure un pequeño “script” y defina la correspondiente unidad de tipo service para
que se ejecute en el proceso de botado de su máquina.

- He creado el script `/usr/local/bin/notify` que se conecta con un bot de Telegram:

```bash
#!/bin/bash
API_KEY=XXXXXXXXXXXX
CHAT_ID=XXXXXXXXXXXX

if [ "$1" == "--boot" ]; then MESSAGE="`/bin/date +'%d/%m/%Y %R:%S '`-> Iniciada máquina de Álvaro."; else MESSAGE=$1; fi

MESSAGE=${MESSAGE:-"Vaya, no hay mensaje."}

wget -qO- "https://api.telegram.org/bot$API_KEY/sendMessage?chat_id=$CHAT_ID&text=$MESSAGE" &> /dev/null
```

- Y un servicio `/etc/systemd/system/notify-boot.service` que me avisa de que la máquina ha sido encendida:

```bash
[Unit]
Description=Custom service that notifies through Telegram on startup.
After=network.target

[Service]
Type=simple
Restart=on-failure
RestartSec=5s
User=lsi
ExecStart=notify --boot

[Install]
WantedBy=multi-user.target
```

- Para activarlo:

```console
root@debian:/home/lsi# systemctl enable notify-boot.service
Created symlink /etc/systemd/system/multi-user.target.wants/notify-boot.service → /etc/systemd/system/notify-boot.service.
```

### Apartado J

Identifique las conexiones de red abiertas a y desde su equipo.

```console
root@debian:/home/lsi# netstat -netua
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       User       Inode     
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      0          12650     
tcp        0    356 10.11.48.50:22          10.30.10.89:50322       ESTABLISHED 0          22994     
tcp6       0      0 :::22                   :::*                    LISTEN      0          12661     
udp        0      0 10.11.50.50:123         0.0.0.0:*                           0          12589     
udp        0      0 10.11.48.50:123         0.0.0.0:*                           0          12587     
udp        0      0 127.0.0.1:123           0.0.0.0:*                           0          12585     
udp        0      0 0.0.0.0:123             0.0.0.0:*                           0          12581     
udp6       0      0 fe80::250:56ff:fe97:123 :::*                                118        12703     
udp6       0      0 fe80::250:56ff:fe97:123 :::*                                118        12701     
udp6       0      0 ::10.11.48.50:123       :::*                                0          12599     
udp6       0      0 2002:a0b:3032::1:123    :::*                                0          12597     
udp6       0      0 ::1:123                 :::*                                0          12591     
udp6       0      0 :::123                  :::*                                0          12578
```

> Tengo dos conexiones *ssh* abiertas con la máquina, una como *root* y otra como *lsi*. El servidor *ssh* está escuchando en IPv4 e IPv6.

### Apartado K

Nuestro sistema es el encargado de gestionar la CPU, memoria, red, etc., como soporte a los datos
y procesos. Monitorice en “tiempo real” la información relevante de los procesos del sistema y
los recursos consumidos. Monitorice en “tiempo real” las conexiones de su sistema.

1. Procesos del sistema en *tiempo real*:

```console
root@debian:/home/lsi# top

top - 23:25:47 up  1:53,  1 user,  load average: 0,00, 0,00, 0,00
Tasks: 188 total,   1 running, 187 sleeping,   0 stopped,   0 zombie
%Cpu(s):  0,0 us,  0,0 sy,  0,0 ni,100,0 id,  0,0 wa,  0,0 hi,  0,0 si,  0,0 st
MiB Mem :   1479,2 total,   1072,1 free,    147,3 used,    259,8 buff/cache
MiB Swap:   1534,0 total,   1534,0 free,      0,0 used.   1192,5 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND                       
   1017 root      20   0   10288   3892   3128 R   0,3   0,3   0:00.30 top                           
      1 root      20   0   99500  10224   7656 S   0,0   0,7   0:01.75 systemd                       
      2 root      20   0       0      0      0 S   0,0   0,0   0:00.00 kthreadd                      
      3 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 rcu_gp                        
      4 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 rcu_par_gp                    
      6 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 kworker/0:0H-events_highpri   
      8 root       0 -20       0      0      0 I   0,0   0,0   0:00.11 kworker/0:1H-events_highpri   
      9 root       0 -20       0      0      0 I   0,0   0,0   0:00.00 mm_percpu_wq
      ...
      ...
      ...
```

2. Conexiones del sistema en *tiempo real*:

```console
root@debian:/home/lsi# netstat -netuac
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       User       Inode     
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      0          12650     
tcp        0    356 10.11.48.50:22          10.30.10.89:50322       ESTABLISHED 0          22994     
tcp6       0      0 :::22                   :::*                    LISTEN      0          12661     
udp        0      0 10.11.50.50:123         0.0.0.0:*                           0          12589     
udp        0      0 10.11.48.50:123         0.0.0.0:*                           0          12587     
udp        0      0 127.0.0.1:123           0.0.0.0:*                           0          12585     
udp        0      0 0.0.0.0:123             0.0.0.0:*                           0          12581     
udp6       0      0 fe80::250:56ff:fe97:123 :::*                                118        12703     
udp6       0      0 fe80::250:56ff:fe97:123 :::*                                118        12701     
udp6       0      0 ::10.11.48.50:123       :::*                                0          12599     
udp6       0      0 2002:a0b:3032::1:123    :::*                                0          12597     
udp6       0      0 ::1:123                 :::*                                0          12591     
udp6       0      0 :::123                  :::*                                0          12578     
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       User       Inode     
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      0          12650     
tcp        0    304 10.11.48.50:22          10.30.10.89:50322       ESTABLISHED 0          22994     
tcp6       0      0 :::22                   :::*                    LISTEN      0          12661     
udp        0      0 10.11.50.50:123         0.0.0.0:*                           0          12589     
udp        0      0 10.11.48.50:123         0.0.0.0:*                           0          12587     
udp        0      0 127.0.0.1:123           0.0.0.0:*                           0          12585     
udp        0      0 0.0.0.0:123             0.0.0.0:*                           0          12581     
udp6       0      0 fe80::250:56ff:fe97:123 :::*                                118        12703     
udp6       0      0 fe80::250:56ff:fe97:123 :::*                                118        12701     
udp6       0      0 ::10.11.48.50:123       :::*                                0          12599     
udp6       0      0 2002:a0b:3032::1:123    :::*                                0          12597     
udp6       0      0 ::1:123                 :::*                                0          12591     
udp6       0      0 :::123                  :::*                                0          12578
```

> Cada segundo se imprime la tabla con los datos de las conexiones del sistema.


### Apartado L

Un primer nivel de filtrado de servicios los constituyen los tcp-wrappers. Configure el tcp-
wrapper de su sistema (basado en los ficheros hosts.allow y hosts.deny) para permitir
conexiones SSH a un determinado conjunto de IPs y denegar al resto. ¿Qué política general de
filtrado ha aplicado?. ¿Es lo mismo el tcp-wrapper que un firewall?. Procure en este proceso no
perder conectividad con su máquina. No se olvide que trabaja contra ella en remoto por ssh.

- `/etc/hosts.allow`: El sistema comprueba primero este archivo para las conexiones tcp.

```bash
# /etc/hosts.allow: list of hosts that are allowed to access the system.
#                   See the manual pages hosts_access(5) and hosts_options(5).
#
# Example:    ALL: LOCAL @some_netgroup
#             ALL: .foobar.edu EXCEPT terminalserver.foobar.edu
#
# If you're going to protect the portmapper use the name "rpcbind" for the
# daemon name. See rpcbind(8) and rpc.mountd(8) for further information.
#

# localhost + pelayo
sshd: 127.0.0.1, 10.11.49.106, 10.11.51.106: spawn echo `/bin/date`\: intento de conexión de %a a %A [PERMITIDO] >> /home/lsi/logssh

# vpn udc:
sshd: 10.30.8.0/255.255.248.0: spawn echo `/bin/date`\: intento de conexión de %a a %A [PERMITIDO] >> /home/lsi/logssh

# eduroam:
sshd: 10.20.32.0/255.255.248.0: spawn echo `/bin/date`\: intento de conexión de %a a %A [PERMITIDO] >> /home/lsi/logssh
```

- `/etc/hosts.deny`: Si la IP desde la que se están intentando conectar a nuestra máquina no coincide con ninguna en `/etc/hosts.allow`, se comprueba si está denegada aquí.

```bash
# /etc/hosts.deny: list of hosts that are _not_ allowed to access the system.
#                  See the manual pages hosts_access(5) and hosts_options(5).
#
# Example:    ALL: some.host.name, .some.domain
#             ALL EXCEPT in.fingerd: other.host.name, .other.domain
#
# If you're going to protect the portmapper use the name "rpcbind" for the
# daemon name. See rpcbind(8) and rpc.mountd(8) for further information.
#
# The PARANOID wildcard matches any host whose name does not match its
# address.
#
# You may wish to enable this to ensure any programs that don't
# validate looked up hostnames still leave understandable logs. In past
# versions of Debian this has been the default.
# ALL: PARANOID

ALL: ALL: spawn echo `bin/date`\: intento de conexión %a a %A [DENEGADA] >> /home/lsi/logssh
```

> Si no coincide con ninguna IP en el `hosts.deny` entonces de permite el acceso por defecto, por lo que denegamos todas las conexiones para que solo se acepten las que están en `hosts.allow`. 

- TCPWrapper no es lo mismo que un *firewall*, pero trabaja de una forma similar en la capa 7.

### Apartado M

Existen múltiples paquetes para la gestión de logs (syslog, syslog-ng, rsyslog). Utilizando el
rsyslog pruebe su sistema de log local.

```console
root@debian:/home/lsi# logger hello
root@debian:/home/lsi# logger "Esto es una prueba"
root@debian:/home/lsi# tail -2 /var/log/syslog
Sep 27 23:45:49 debian lsi: hello
Sep 27 23:45:59 debian lsi: Esto es una prueba
```

> `tail -2` devuelve las dos últimas líneas de un fichero.


### Apartado N

Configure IPv6 6to4 y pruebe ping6 y ssh sobre dicho protocolo. ¿Qué hace su tcp-wrapper en
las conexiones ssh en IPv6? Modifique su tcp-wapper siguiendo el criterio del apartado h).
¿Necesita IPv6?. ¿Cómo se deshabilita IPv6 en su equipo?

1. Añadir a `/etc/network/interfaces` la configuración de la nueva interfaz:

```
auto 6to4
iface 6to4 inet6 v4tunnel
	pre-up modprobe ipv6
	address 2002:a0b:3032::1
	netmask 16
	gateway ::10.11.48.1
	endpoint any
	local 10.11.48.50
```

2. Activar la interfaz con `ifup 6to4`.

3. Probamos a hacer un `ping6`:

```console
root@debian:/home/lsi# ping6 2002:a0b:3032::1
PING 2002:a0b:3032::1(2002:a0b:3032::1) 56 data bytes
64 bytes from 2002:a0b:3032::1: icmp_seq=1 ttl=64 time=0.061 ms
64 bytes from 2002:a0b:3032::1: icmp_seq=2 ttl=64 time=0.058 ms
64 bytes from 2002:a0b:3032::1: icmp_seq=3 ttl=64 time=0.058 ms
^C
--- 2002:a0b:3032::1 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2050ms
rtt min/avg/max/mdev = 0.058/0.059/0.061/0.001 ms
```

- Añadimos a `/etc/hosts.allow`:

```bash
shd: [2002:a0b:3032::1]/48, [2002:a0b:316a::1]/48
```

> IP propia (ens33) + compañero (ens33 también).

- Probamos *ssh*:

```console
root@debian:/home/lsi# ssh lsi@2002:a0b:3032::1
lsi@2002:a0b:3032::1's password: 
Linux debian 5.10.0-18-amd64 #1 SMP Debian 5.10.140-1 (2022-09-02) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Wed Sep 28 00:04:25 2022 from 2002:a0b:3032::1
lsi@debian:~$
```

- Pruebo con mi compañero también:

```console
root@debian:/home/lsi# ping6 2002:a0b:316a::1
PING 2002:a0b:316a::1(2002:a0b:316a::1) 56 data bytes
64 bytes from 2002:a0b:316a::1: icmp_seq=1 ttl=64 time=0.449 ms
64 bytes from 2002:a0b:316a::1: icmp_seq=2 ttl=64 time=0.480 ms
64 bytes from 2002:a0b:316a::1: icmp_seq=3 ttl=64 time=0.386 ms
64 bytes from 2002:a0b:316a::1: icmp_seq=4 ttl=64 time=0.476 ms
^C
--- 2002:a0b:316a::1 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3079ms
rtt min/avg/max/mdev = 0.386/0.447/0.480/0.037 ms
```

```console
root@debian:/home/lsi# ssh lsi@2002:a0b:316a::1
lsi@2002:a0b:316a::1's password: 
Linux debian 5.10.0-18-amd64 #1 SMP Debian 5.10.140-1 (2022-09-02) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Wed Sep 28 11:03:52 2022 from 2002:a0b:3032::1
```

- No es necesario en nuestra red interna el uso de IPv6 gracias a nuestra interfaz 6to4.

- Se dehabilita añadiendo estas líneas en el fichero `/etc/sysctl.conf`:

```
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
```

> Para que el sistema aplique los cambios: `sysctl -p`

--- 

## PARTE 2

### Apartado A

En colaboración con otro alumno de prácticas, configure un servidor y un cliente NTP.

> Demostración con `10.11.49.106` como servidor y `10.11.48.50` como cliente.

1. Servidor: El fichero `/etc/ntp.conf` debería quedar así:

```bash
# /etc/ntp.conf, configuration for ntpd; see ntp.conf(5) for help

driftfile /var/lib/ntp/ntp.drift

# Leap seconds definition provided by tzdata
leapfile /usr/share/zoneinfo/leap-seconds.list

# Enable this if you want statistics to be logged.
#statsdir /var/log/ntpstats/

statistics loopstats peerstats clockstats
filegen loopstats file loopstats type day enable
filegen peerstats file peerstats type day enable
filegen clockstats file clockstats type day enable

# Configuración servidor
server 127.127.1.0 minpoll 4
fudge 127.127.1.0 stratum 0

# You do need to talk to an NTP server or two (or three).

# pool.ntp.org maps to about 1000 low-stratum NTP servers.  Your server will
# pick a different set every time it starts up.  Please consider joining the
# pool: <http://www.pool.ntp.org/join.html>
#pool 0.debian.pool.ntp.org iburst
#pool 1.debian.pool.ntp.org iburst
#pool 2.debian.pool.ntp.org iburst
#pool 3.debian.pool.ntp.org iburst
```

```bash
# By default, exchange time with everybody, but don't allow configuration.
#restrict -4 default kod notrap nomodify nopeer noquery limited
#restrict -6 default kod notrap nomodify nopeer noquery limited
restrict default ignore
restrict 10.11.48.50 nomodify nopeer notrap

# Local users may interrogate the ntp server more closely.
restrict 127.0.0.1
restrict ::1
```

- Además, el servidor debe añadir al cliente en su `hosts.allow` para el servicio `ntpd`:

```bash
ntpd: 10.11.48.50
```

2. Cliente: Dejamos el fichero `/etc/ntp.conf` así:

```bash
# /etc/ntp.conf, configuration for ntpd; see ntp.conf(5) for help

driftfile /var/lib/ntp/ntp.drift

# Leap seconds definition provided by tzdata
leapfile /usr/share/zoneinfo/leap-seconds.list

# Enable this if you want statistics to be logged.
#statsdir /var/log/ntpstats/

statistics loopstats peerstats clockstats
filegen loopstats file loopstats type day enable
filegen peerstats file peerstats type day enable
filegen clockstats file clockstats type day enable

### Configuración cliente
server 10.11.49.106 minpoll 4
fudge 127.127.1.0 stratum 1
```

```bash
# By default, exchange time with everybody, but don't allow configuration.
#restrict -4 default kod notrap nomodify nopeer noquery limited
#restrict -6 default kod notrap nomodify nopeer noquery limited
#restrict default ignore
#restrict 10.11.49.106 nomodify notrap nopeer

# Local users may interrogate the ntp server more closely.
restrict 127.0.0.1
restrict ::1

# Needed for adding pool entries
restrict source notrap nomodify noquery
```

3. Actualizar cambios en `ntp.conf`: `systemctl restart ntp`.

4. Comprobamos:

```console
root@debian:~# date +%T -s 1
01:00:00
root@debian:~# ntpdate 10.11.49.106
28 Sep 23:43:46 ntpdate[1017]: step time server 10.11.49.106 offset +81815.937779 sec
```

> Lo que hice aquí fue poner el reloj local del cliente a las `01:00:00` y hacer `ntpdate` a la IP del servidor NTP para sincronizar la hora.

### Apartado B

Cruzando los dos equipos anteriores, configure con rsyslog un servidor y un cliente de logs.

> Demostración con `10.11.48.50` como servidor y `10.11.49.106` como cliente.

1. Servidor: El fichero `/etc/rsyslog.conf` debería quedar así:

```bash
#################
#### MODULES ####
#################

module(load="imuxsock") # provides support for local system logging
module(load="imklog")   # provides kernel logging support
#module(load="immark")  # provides --MARK-- message capability

# provides UDP syslog reception
#module(load="imudp")
#input(type="imudp" port="514")

# provides TCP syslog reception
module(load="imtcp")
input(type="imtcp" port="514")

# template para guardar los registros de log
$template remote, "/var/log/rsyslog-server/%fromhost-ip%/%programname%.log"
*.* ?remote
& stop

###########################
#### GLOBAL DIRECTIVES ####
###########################

#
# Servidor sólo aceptará mensajes del compañero
#
$AllowedSender TCP 127.0.0.1, 10.11.49.106
```

- Además, el servidor debe añadir al cliente en su `hosts.allow` para el servicio `rsyslogd`:

```bash
rsyslogd: 10.11.49.106
```

2. Cliente: Añadir lo siguiente al final del fichero `/etc/rsyslog.conf`:

```bash
# Client:

# Old Syntax (deprecated)

#$ActionQueueType LinkedList
#$ActionQueueFileName /var/log/rsyslog-queue
#$ActionQueueSaveOnShutdown on
#$ActionResumeRetryCount -1
#*.* @@10.11.49.48.50:514

# New Syntax

*.* action(
       type="omfwd" 
       target="10.11.48.50" 
       port="514" 
       protocol="tcp" 
       action.resumeRetryCount="-1"
       queue.type="linkedlist"
       queue.filename="/var/log/rsyslog-queue"
       queue.saveOnShutdown="on"
)
```

3. Actualizar cambios en `rsyslog.conf`: `systemctl restart rsyslog.service`.

4. Probamos:

- Cliente:

```console
root@debian:/home/lsi# logger "ESTO ES UNA PRUEBA"
```

- Servidor:

```console
root@debian:~# cat /var/log/rsyslog-server/10.11.49.106/lsi.log 
2022-09-28T20:49:58+02:00 debian lsi: ESTO ES UNA PRUEBA
```
5. Probando la cola:
	1. En el servidor, abrir el archivo `/etc/rsyslog.conf` y comentar la siguiente línea, para que deje de escucharse en el puerto 514/tcp:
	```bash
	# provides TCP syslog reception
	module(load="imtcp")
	# input(type="imtcp" port="514")
	```
	2. Tras esto, ejecutar `systemctl restart rsyslog`.
	3. En el cliente, mandar un log al servidor. Por ejemplo, `logger "Hola, estás ahí?"`. Con esto, el cliente intentará enviarlo, pero al tener cortada la conexión deberá encolarlo.
	4. Comprobar que no se ha guardado el nuevo mensaje de log generado por el cliente desconectado `tail /var/log/rsyslog/10.11.49.106/lsi.log`
	5. Descomentar la línea comentada en el paso `1.` y volver a ejecutar el paso `2.`.
	6. Tras unos segundos, volver a hacer `tail /var/log/rsyslog/10.11.49.106/lsi.log` para comprobar que los mensajes se han procesado y almacenado.

### Apartado C

Haga todo tipo de propuestas sobre los siguientes aspectos: ¿Qué problemas de seguridad identifica en los dos apartados anteriores? ¿Cómo podría solucionar los problemas identificados?

- En `rsyslog` cualquiera podría enviar logs al servidor y llenar el disco. Además, los logs van sin cifrar por la red; cualquiera podría ver o modificar sus contenidos con un ataque MitM (Man in the Middle). También podrían hacer al servidor un \[D\]DOS.

- NTP trabaja sobre UDP; también es posible un \[D\]DOS haciendo IP Spoofing.

- Solución: 

	- Certificados TLS. Con estos certificados entre cliente y servidor podríamos asegurar la autenticidad del cliente y la confidencialidad e integridad de los datos. 

	- Podríamos asegurar la conectividad al puerto mediante *firewall* y mecanismos en capas inferiores.


### Apartado D

En la plataforma de virtualización corren, entre otrs equipos, más de 200 máquinas virtuales para LSI. Como los recursos son limitados, y el disco duro también, identifique todas aquellas acciones que pueda hacer para reducir el espacio de disco ocupado.

- `df -H`: Muestra información sobre el almacenamiento de los sistemas de ficheros montados en la máquina.

- `apt autoclean`: Elimina de la caché los paquetes de versiones antiguas e innecesarias.

- `apt clean`: Elimina **todos** los paquetes de la caché.

- `apt autoremove`: Elimina aquellos paquetes perdidos, generalmente instalados como dependencias de otras instalaciones, que ya no son necesarios.

- `apt --purge autoremove`: La opción `--purge` permite otras llamadas de *apt* para borrar también archivos de configuración y demás.

- Borrar man: `apt remove --purge man-db`

- Borrar imágenes kernel antiguas:

	- `uname -r`: Muestra kernel actual.

	- `dpkg --list | grep linux-image`: Muestra los kernels que tenemos en el sistema.

	- `apt-get --purge remove linux-image-4.......` Elimina un kernel en específico.
	
	- Dejar solo estas dos imágenes:

	```console
	root@debian:/home/lsi# dpkg --list | grep linux-image
	ii  linux-image-5.10.0-18-amd64           5.10.140-1                       amd64        Linux 5.10 for 64-bit PCs (signed)
	ii  linux-image-amd64                     5.10.140-1                       amd64        Linux for 64-bit PCs (meta-package)
	```

