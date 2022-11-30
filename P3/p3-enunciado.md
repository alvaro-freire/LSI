

### Apartado 1.A

```bash
root@debian:/home/lsi# touch /etc/ssh/ssh_known_hosts
root@debian:/home/lsi# ssh-keyscan 10.11.49.106 >> /etc/ssh/ssh_known_hosts 
# 10.11.49.106:22 SSH-2.0-OpenSSH_8.4p1 Debian-5+deb11u1
# 10.11.49.106:22 SSH-2.0-OpenSSH_8.4p1 Debian-5+deb11u1
# 10.11.49.106:22 SSH-2.0-OpenSSH_8.4p1 Debian-5+deb11u1
# 10.11.49.106:22 SSH-2.0-OpenSSH_8.4p1 Debian-5+deb11u1
# 10.11.49.106:22 SSH-2.0-OpenSSH_8.4p1 Debian-5+deb11u1
```

```bash
root@debian:/home/lsi# echo "" > /home/lsi/.ssh/known_hosts 
root@debian:/home/lsi# nano /home/lsi/.ssh/known_hosts 
root@debian:/home/lsi# ssh lsi@10.11.49.106
lsi@10.11.49.106's password: 
```


### Apartado 1.C

como usuario lsi !!!!

```bash
lsi@debian:~$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/lsi/.ssh/id_rsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/lsi/.ssh/id_rsa
Your public key has been saved in /home/lsi/.ssh/id_rsa.pub
The key fingerprint is:
SHA256:HUjKq1t1DeiVu7jlDyqGgSI0zUv3snuNC1XelxkER9k lsi@debian
The key's randomart image is:
+---[RSA 3072]----+
|        .  .o+o  |
|     . o o .o. E |
|  o   o o.=  .   |
| o + . ooo.=  +  |
|. o.o o.S.+..+   |
|o ...o.o o ..    |
|..  .++ + +      |
|    .++o * .     |
|    .ooo+ ...    |
+----[SHA256]-----+
```

```
lsi@debian:~$ ssh-keygen -t ed25519
Generating public/private ed25519 key pair.
Enter file in which to save the key (/home/lsi/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/lsi/.ssh/id_ed25519
Your public key has been saved in /home/lsi/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:CHp89jJosHstoIEusaFjX099185ugWcErw9oHIMpyIM lsi@debian
The key's randomart image is:
+--[ED25519 256]--+
|                 |
|             .   |
|    + .   o   o  |
|   E = o o o   o |
|. o o = S . + +  |
|= .+ + ..  + +.+ |
|o*..ooo..... .=..|
|*o o+ +o  . . oo |
|o.oo . .      o+ |
+----[SHA256]-----+
```

```
lsi@debian:~$ ssh-keygen -t ecdsa
Generating public/private ecdsa key pair.
Enter file in which to save the key (/home/lsi/.ssh/id_ecdsa): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in /home/lsi/.ssh/id_ecdsa
Your public key has been saved in /home/lsi/.ssh/id_ecdsa.pub
The key fingerprint is:
SHA256:hDwu6WHOyrUFdH5zxHr7CJVsYoXhXmc6rKj+0A50tsM lsi@debian
The key's randomart image is:
+---[ECDSA 256]---+
|        .        |
|     . o +       |
|    . = + = o    |
|   . = + B =     |
|    B = S @      |
|   * O = O o     |
|    B E o .      |
| . o B . . o     |
|  o.+.o   . .    |
+----[SHA256]-----+
```

compa desde ~/.ssh:

```bash
root@debian:/home/lsi/.ssh# scp lsi@10.11.48.50:/home/lsi/.ssh/*.pub ../keys
root@debian:/home/lsi/.ssh# touch authorized_keys
root@debian:/home/lsi/.ssh# cat ../keys/id_rsa.pub >> authorized_keys 
root@debian:/home/lsi/.ssh# cat ../keys/id_ed25519.pub >> authorized_keys 
root@debian:/home/lsi/.ssh# cat ../keys/id_ecdsa.pub >> authorized_keys
```

comprobamos que no nos pide password:

```bash
lsi@debian:~$ ssh lsi@10.11.49.106
Linux debian 5.10.0-18-amd64 #1 SMP Debian 5.10.140-1 (2022-09-02) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
You have no mail.
Last login: Wed Nov 23 11:20:51 2022 from 10.20.37.153
```

### Apartado 2.A

```bash
root@debian:/usr/lib/ssl/misc# ./CA.pl -newca
CA certificate filename (or enter to create)

Making CA certificate ...
====
openssl req  -new -keyout ./demoCA/private/cakey.pem -out ./demoCA/careq.pem 
Generating a RSA private key
...................+++++
................................+++++
writing new private key to './demoCA/private/cakey.pem'
Enter PEM pass phrase:
Verifying - Enter PEM pass phrase:
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:lsi
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:web
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
==> 0
====
====
openssl ca  -create_serial -out ./demoCA/cacert.pem -days 1095 -batch -keyfile ./demoCA/private/cakey.pem -selfsign -extensions v3_ca  -infiles ./demoCA/careq.pem
Using configuration from /usr/lib/ssl/openssl.cnf
Enter pass phrase for ./demoCA/private/cakey.pem:
Check that the request matches the signature
Signature ok
Certificate Details:
        Serial Number:
            71:0d:f6:0d:21:2e:ec:50:91:33:f3:5a:63:ea:ef:dc:1f:26:8a:a3
        Validity
            Not Before: Nov 30 10:06:23 2022 GMT
            Not After : Nov 29 10:06:23 2025 GMT
        Subject:
            countryName               = AU
            stateOrProvinceName       = Some-State
            organizationName          = lsi
            commonName                = web
        X509v3 extensions:
            X509v3 Subject Key Identifier: 
                D0:68:75:D8:0D:00:E9:3F:06:77:E6:0F:11:AE:32:F1:5A:97:69:9D
            X509v3 Authority Key Identifier: 
                keyid:D0:68:75:D8:0D:00:E9:3F:06:77:E6:0F:11:AE:32:F1:5A:97:69:9D

            X509v3 Basic Constraints: critical
                CA:TRUE
Certificate is to be certified until Nov 29 10:06:23 2025 GMT (1095 days)

Write out database with 1 new entries
Data Base Updated
==> 0
====
CA certificate is in ./demoCA/cacert.pem
```

### Apartado 2.B

Generamos certificado para el servidor web de mi compañero:

```bash
root@debian:/usr/lib/ssl/misc# ./CA.pl -newreq-nodes
====
openssl req  -new -nodes -keyout newkey.pem -out newreq.pem -days 365 
Ignoring -days; not generating a certificate
Generating a RSA private key
...........+++++
..............................+++++
writing new private key to 'newkey.pem'
-----
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:
State or Province Name (full name) [Some-State]:
Locality Name (eg, city) []:
Organization Name (eg, company) [Internet Widgits Pty Ltd]:lsi
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:pelayo
Email Address []:

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
==> 0
====
Request is in newreq.pem, private key is in newkey.pem
```

Firmamos el certificado:

```bash
root@debian:/usr/lib/ssl/misc# ./CA.pl -sign
====
openssl ca  -policy policy_anything -out newcert.pem  -infiles newreq.pem
Using configuration from /usr/lib/ssl/openssl.cnf
Enter pass phrase for ./demoCA/private/cakey.pem:
Check that the request matches the signature
Signature ok
Certificate Details:
        Serial Number:
            71:0d:f6:0d:21:2e:ec:50:91:33:f3:5a:63:ea:ef:dc:1f:26:8a:a5
        Validity
            Not Before: Nov 30 10:17:41 2022 GMT
            Not After : Nov 30 10:17:41 2023 GMT
        Subject:
            countryName               = AU
            stateOrProvinceName       = Some-State
            organizationName          = lsi
            commonName                = pelayo
        X509v3 extensions:
            X509v3 Basic Constraints: 
                CA:FALSE
            Netscape Comment: 
                OpenSSL Generated Certificate
            X509v3 Subject Key Identifier: 
                29:CB:34:17:E0:EC:FC:B5:59:E3:CC:C6:C7:B1:C7:FF:D7:44:25:F1
            X509v3 Authority Key Identifier: 
                keyid:D0:68:75:D8:0D:00:E9:3F:06:77:E6:0F:11:AE:32:F1:5A:97:69:9D

Certificate is to be certified until Nov 30 10:17:41 2023 GMT (365 days)
Sign the certificate? [y/n]:y


1 out of 1 certificate requests certified, commit? [y/n]y
Write out database with 1 new entries
Data Base Updated
==> 0
====
Signed certificate is in newcert.pem
```

### Apartado 2.C

Copiamos de nuestro compa el certificado firmado, la CPriv del certificado y la CPriv de la CA:

```bash
root@debian:/home/lsi# scp lsi@10.11.49.106:/home/lsi/certs_to_copy/* compa_cert 
lsi@10.11.49.106's password: 
cacert.pem                                    100% 4295     3.5MB/s   00:00    
newcert.pem                                   100% 4424     4.9MB/s   00:00    
newkey.pem                                    100% 1704     2.3MB/s   00:00
```

Activamos ssl en apache:

```bash
root@debian:/home/lsi# cd /etc/apache2
root@debian:/etc/apache2# a2enmod ssl
Considering dependency setenvif for ssl:
Module setenvif already enabled
Considering dependency mime for ssl:
Module mime already enabled
Considering dependency socache_shmcb for ssl:
Module socache_shmcb already enabled
Enabling module ssl.
See /usr/share/doc/apache2/README.Debian.gz on how to configure SSL and create self-signed certificates.
To activate the new configuration, you need to run:
  systemctl restart apache2
```

Copiamos los ficheros a nuestra carpeta en `/etc/apache2/ssl`:

```bash
root@debian:/etc/apache2# mkdir ssl
root@debian:/etc/apache2# cd ssl
root@debian:/etc/apache2/ssl# mkdir alvaro
root@debian:/etc/apache2/ssl# cd alvaro/
root@debian:/etc/apache2/ssl/alvaro# cp /home/lsi/compa_cert/* .
```

Modificamos los permisos para que queden así:

```bash
root@debian:/etc/apache2/ssl/alvaro# ls -l
total 20
-rw-r----- 1 root root 4295 nov 30 11:43 cacert.pem
-rw-r----- 1 root root 4424 nov 30 11:43 newcert.pem
-rw------- 1 root root 1704 nov 30 11:43 newkey.pem
```

Modificamos el `/etc/apache2/sites-available/default-ssl.conf`:

```
#   Enable/Disable SSL for this virtual host.
SSLEngine on

#   A self-signed (snakeoil) certificate can be created by inst>
#   the ssl-cert package. See
#   /usr/share/doc/apache2/README.Debian.gz for more info.
#   If both key and certificate are stored in the same file, on>
#   SSLCertificateFile directive is needed.
SSLCertificateFile      /etc/apache2/ssl/alvaro/newcert.pem
SSLCertificateKeyFile /etc/apache2/ssl/alvaro/newkey.pem
```

```
#   Certificate Authority (CA):
#   Set the CA certificate verification path where to find CA
#   certificates for client authentication or alternatively one
#   huge file containing all of them (file must be PEM encoded)
#   Note: Inside SSLCACertificatePath you need hash symlinks
#                to point to the certificate files. Use the pro>
#                Makefile to update the hash symlinks after cha>
#SSLCACertificatePath /etc/ssl/certs/
SSLCACertificateFile /etc/apache2/ssl/alvaro/cacert.pem
```

Actualizamos apache:

```bash
root@debian:/etc/apache2/sites-available# systemctl restart apache2
```