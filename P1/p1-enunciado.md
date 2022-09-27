# PRÁCTICA 1 - CONFIGURACIÓN BÁSICA (SOLUCIÓN)

En esta segunda parte se muestran las soluciones a la primera práctica. En [`p1-sesiones`](./p1-sesiones.md) se mostrarán los apuntes tomados durante las sesiones.

## PARTE 1

### Apartado A

Configure su máquina virtual de laboratorio con los datos proporcionados por el profesor.
Analice los ficheros básicos de configuración (interfaces, hosts, resolv.conf,
nsswitch.conf, sources.list, etc.)

### Apartado B

¿Qué distro y versión tiene la máquina inicialmente entregada?. Actualice su máquina a la última
versión estable disponible.

### Apartado C

Identifique la secuencia completa de arranque de una máquina basada en la distribución de
referencia (desde la pulsación del botón de arranque hasta la pantalla de login). ¿Qué target por defecto tiene su máquina? ¿Cómo podría cambiar el target de arranque? ¿Qué targets tiene su sistema y en qué estado se encuentran? ¿Y los services? Obtenga la relación de servicios de su sistema y su estado. ¿Qué otro tipo de unidades existen?

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
