

### Apartado 1.A

```
root@debian:/home/lsi# touch /etc/ssh/ssh_known_hosts
root@debian:/home/lsi# ssh-keyscan 10.11.49.106 >> /etc/ssh/ssh_known_hosts 
# 10.11.49.106:22 SSH-2.0-OpenSSH_8.4p1 Debian-5+deb11u1
# 10.11.49.106:22 SSH-2.0-OpenSSH_8.4p1 Debian-5+deb11u1
# 10.11.49.106:22 SSH-2.0-OpenSSH_8.4p1 Debian-5+deb11u1
# 10.11.49.106:22 SSH-2.0-OpenSSH_8.4p1 Debian-5+deb11u1
# 10.11.49.106:22 SSH-2.0-OpenSSH_8.4p1 Debian-5+deb11u1
```

```
root@debian:/home/lsi# echo "" > /home/lsi/.ssh/known_hosts 
root@debian:/home/lsi# nano /home/lsi/.ssh/known_hosts 
root@debian:/home/lsi# ssh lsi@10.11.49.106
lsi@10.11.49.106's password: 
```


### Apartado 1.C

como usuario lsi !!!!

```
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
```
root@debian:/home/lsi/.ssh# scp lsi@10.11.48.50:/home/lsi/.ssh/*.pub .
root@debian:/home/lsi/.ssh# touch authorized_keys
root@debian:/home/lsi/.ssh# cat ../keys/id_rsa.pub >> authorized_keys 
root@debian:/home/lsi/.ssh# cat ../keys/id_ed25519.pub >> authorized_keys 
root@debian:/home/lsi/.ssh# cat ../keys/id_ecdsa.pub >> authorized_keys
```

comprobamos que no nos pide password:
```
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

