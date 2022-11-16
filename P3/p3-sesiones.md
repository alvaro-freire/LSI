# SSH
configurar /etc/ssh/ssh_known_hosts
$ ssh -v lsi@ip (saber lo que hace por bloques)

cliente -> /etc/ssh/ssh_config
server  -> /etc/ssh/sshd_config

# config preferencias d algoritmos
SSH2_MSG_KEXINIT sent/received algoritmo

kex_algorithms , , curve 2559 # key exchange, clave publica-privada de sesión
server_host_key_alg ecdsa # autenticar el servidor
encryption -> # del servidor al cliente y al reves, orden de preferencia de los algoritmos para filtrar la paquetería
encryption <- chacha20 # clave privada

#clave pub-priv para  sesión
#clave privada para cifrar y descifrar paquetería (mucho más rápidos que pub-priv)

/etc/ssh:

ssh.host_dns
____.pub
....rs
____.pub
____.ecds
___.pub
ed255c
____.pub

$HOME/.ssh/identity
$HOME/.ssh/known_hosts # servidores a los que se ha conectado un usuario

# cojo la clave publica de un server y a mano la meto en /etc/ssh/ssh_known_hosts
# todos mis usuarios, cuando se conecten, no necesitarán que les inyecten las claves
# así ya no hay flujos de claves y todos los usuarios usan la misma clave

# intercambiamos los .pub y los metemos en /etc/ssh/ssh_known_hosts
# primera pregunta de P3
# Usuario: borrame el contenido de $HOME/.ssh/known_hosts y prueba conexion ssh
# -> aparece lo del fingerprint: MAL
# -> no aparece: BIEN (porque coge la clave /etc/ssh/ssh_known_hosts)


ssh_keyscan ipcompa >> /etc /etc/ssh/ssh_known_hosts # meter las claves del compañero

# ssh -X ip

# scp

# conexion d usuario con clave publica:
# no te líes y hazlo con usuario "lsi"

# configurar sistema d autenticación de clave publica y privada en vez de user password

# cojo mi publica y se la mando a mi compa # en authorized_keys
# cojo la publica del usuario lsi y la cifro con la privada

ssh_keygen -t rsa/ecdsa/ed25519

# genera el id_rsa y id_rsa.pub (o el alg que sea)
# proteger clave privada con permisos
# passphrase en blanco

# pasar esos pub al compa
# meter en $HOME/.ssh/autohorized_keys

# cagada de todos los años (con la gran empanada): confundir sistemas de claves pub-priv (ssh hosts ASOCIADAS AL HOST) 
# con el sistema de claves pub-priv asociadas a los usuarios de la máquina

# tambien se pueden copiar los .pub con ssh_copy_id -i $HOME/.ssh/id_rsa lsi@ipcompa
# eso lo mete en el authorized_keys

# tener claro como funciona lo de las claves privadas en la conexión

# ssh -P -L 0.0.0.0:10080:ipcompa:80 lsi@ipcompa
# estoy redirigiendo mi puerto local 10080 al puerto 80 de mi compañero
# accedo a un servicio no seguro a través de una conexión ssh segura

# medidas de securización de ssh (nino nos lo perdona)


### apartado 2 HTTPS

el servidor tiene un certificado con una clave pub y su corresp privada
el servidor envia clave pub al cliente para lo mismo que ssh
-> autenticamos al servidor
-> se establece una clave de sesión para filtrar el tráfico http (con x ejemplo chacha20)

como funciona verificacion certificado

# configurarnos como autoridad certificadora (AC)
dos ficheros:
K PUB de la AC (organization name: LSI)
K PRIV de la AC

# el compa tiene que insertar la clave publica en los repositorios

# generar certificado para el servidor web de mi compa
organization name tiene que ser el mismo (LSI en nuestro caso)
common name: web (lo que se está certificando)

# puedes poner en el /etc/hosts URL apuntando a IP de servidor

# con el certificado, genero un hash y lo cifro con la clave priv -> certificado firmado


# openssl

# cd /usr/lib/ssl/misc
# ./CA.pl -newca
# frase paso: meterla
# completar datos
# generará un .pub y otro fichero (YA SOMOS AC con ese comandito de mierda)

# generar certificado para el server web de mi compa con su clave priv
# ./CA.pl newreq-node
# FRASE DE PASO EN BLANCO
# common
# genera clave publica y privada (controlar permisos de la privada)

# ./CA.pl -sign

# cacert.pem (clave publica de la CA)
# private/cakey.pem (clave priv de la CA)
# newcert.pem (cert firmado)
# newkey.pem (Clave privada)

# activar ssl en apache para activar el puerto 443 y tener https

# usuario-passwd en https: nos perdona este apartado también
