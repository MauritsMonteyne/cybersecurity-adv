# Week 4
## How networks are attacked - Part I
Link: [Week 2 - How Networks are Attacked - Part I - slides 31-40](https://learning.edx.org/course/course-v1:NYUx+CYB.NET.1+3T2020/block-v1:NYUx+CYB.NET.1+3T2020+type@sequential+block@9d35edaacc39412cb38d759cfb1732a7/block-v1:NYUx+CYB.NET.1+3T2020+type@vertical+block@4bfa2f541a0a4affb24518615a3d4e6a)

### Nmap
Nmap is een populaire port scanner voor UNIX en Windows

### TCP SYN Scan
Een **SYN scan** is het meest voorkomende type scan. Kijkt welke ports open/closed zijn.

Normaal verloop TCP:
1. Host stuurt **SYN**
2. Server antwoordt met **SYN/ACK**
3. Host bevestigd verbinding met **ACK**

=> Handshake is compleet, **port is open**

Er zijn 3 mogelije scenario's bij het sturen van SYN naar een server.

#### Port Open
- Server antwoordt met **SYN/ACK**
- 2 opties om scan te verbergen voor bepaalde logs:
    1. **Niet antwoorden met ACK** (= **half-open scan**)
    2. **Antwoorden met RST** (Reset, breekt conenctie af)

#### Port Closed
- Server antwoordt met **RST** (Reset)

#### Port Filtered
- Server **antwoord niet**
- Wellicht **firewall** of **router** die packets weggooit 

### Wireshark
Wireshark is een populaire network protocol analyser. Kan traffic visualizeren en capturen.

### Scan types in Nmap
| Scan type | Uitleg |
| :--- | :--- |
| TCP Connect | Doet threeway-way handshake. Meest betrouwbaar, makkelijkst te detecteren |
| TCP SYN | Doet half-open scan. Threeway-way handshake komt niet tot stand |
| TCP FIN | Stuurt FIN packets. Closed ports sturen RST terug |
| TCP NULL | Stuurt packets met geen enkel flag. Ports sturen RST |
| TCP ACK | Gebruikt om ACL rules te achterhalen of aanwezigheid van stateless inspection |
| TCP XMAS | Stuurt packets met FIN, URG, and PSH flags. Ports sturen RST |
| FTP Proxy "Bounce Scan" | Maakt misbruik van slecht geconfigureerde FTP server om bestanden te sturen naar andere toestellen op het netwerk |
| Version Scanning | Probeert version van programma op port te achterhalen |
| Fragmented Scans | Kan sommige router ACL omzeilen |
| TCP Sequence Prediction | Gebruikt bij spoofing attacks |
| Idle Scan (Hide Scan Source) | Verbergt source van de scan. (IP spoofing) |

### Nmap Commands with OS Fingerprinting
```
# nmap -sV -O -sC --top-ports 100 -T4 -oA [file] [address]
$ nmap -sV -O -sC --top-ports 100 -T4 -oA out.txt 10.1.1.0/24
```

| Option | Uitleg |
| :--- | :--- |
| -Pn | treat hosts as online, don't ping |
| -p | port |
| -sV | Checkt open ports op service en version info |
| -O | OS detection |
| -sC | Script scanning |
| --top-ports | Enkel populaire ports |
| -T4 | Scan speed [0 - 5] |
| -oA | Output naar file |

### Firewalk
- Firewalk is een network scanning tool om targets achter een firewall te zoeken.
- Maakt gebruik van TTL van packets (UDP/TCP)
- Bij vervallen van TTL stuurt toestel/router IMCP message naar source
- Firewalk stuurt packet met TTL langer dan de te testen firewall
- Werkt enkel als firewall traffice doorlaat

### HTTPRecon
HTTPRecon is een tool om webservers te bekijken en hun response headers te capturen. Deze headers bevatten info zoals OS en webserver software.

### Vega
- Vega is een tool om webservers te scannen op type server, OS, kwetsbaarheden.
- Vega scant website recursief en bouwt een interne representatie van de website als een tree-like datastructuur.

### Staying Anonymous
- Attackers gebruiken vaak proxy server om anoniem te blijven.
- Attacks komen dan van de proxy server i.p.v rechtstreeks van de attacker


### Onion Routing
- Tor is een network van virtuele tunnels die aan elkaar gelinkt zijn en functioneren als 1 grote proxy.
- Bij elke site worden willekeurige servers gebruikt
- Biedt enkel anonimiteit, geen vertrouwelijkheid. (Exit node kan alles bekijken)



## Week 34 - section 4-5: scanning TCP/UDP ports
- Bijna alle scannen via TCP/UDP
- TCP
    - Connection-oriented
    - Snel
    - Stateful
- UDP
    - Connenctionless
    - Traag
    - Stateless

### TCP scanning
Normaal verloop TCP:
1. Host stuurt **SYN**
2. Server antwoordt met **SYN/ACK**
3. Host bevestigd verbinding met **ACK**

| Attacker | Target | Uitleg |
| :--- | :--- | :--- |
| SYN | SYN/ACK | port open |
| SYN | RST/ACK | port closed/blocked |
| SYN | ICMP port unreachable | firewall/blocked |
| SYN | no respone | port blocked |

*No respone kan lang duren bij Nmap (stateful) door wachten op timeouts en retransmissions. Zmap is stateless, maar minder accuraat*

### UDP scanning

| Attacker | Target | Uitleg |
| :--- | :--- | :--- |
| UDP | UDP | port open |
| UDP | ICMP dest unreachable | port blocked |
| UDP | ICMP port unreachable | port closed/blocked |
| UDP | no respone | port closed/blocked, port open but firewall, wrong payload |

*No repose is het meest voorkomend bij UDP*

### UDP ICMP respones
| Type | Uitleg |
| :--- | :--- |
| 1 | host unreachable |
| 2 | protocol unreachable |
| 9 | dest network administratively prohibited |
| 10 | dest host administratively prohibited |
| 13 | communication administratively prohibited |

*Linux beperkt ICMP "host unreachable" naar 1 message per seconde*



## Week 34 - section 4-6: netcat
### Netcat
- Simpele tool om verbinding te maken met port of te luisteren naar port.
- Op bijna alle plaformen beschikbaar
- Niet geëncrypteerd, plain text (interpreteert data ook niet vs telnet)
- Kan script uitvoeren
- Kali variant ncat is wel geëncrypteerd
- Kan ook files versturen met netcat

| Option | Uitleg |
| :--- | :--- |
| -l | listen |
| -n | don't resolve name |
| -p | port |



## Week 34 - section 4-7 to 4-10: nmap basics and beyond 
### Nmap
- Meest populaire port scanner voor UNIX en Windows
- Network mapper

| Option | Uitleg |
| :--- | :--- |
| -Pn | treat hosts as online, don't ping |
| -p | port |
| -sV | Checkt open ports op service en version info |
| -O | OS detection |
| -sC | Script scanning |
| --top-ports | Enkel populaire ports |
| -T4 | Scan speed [0 - 5] |
| -oA | Output naar file |

*Default speed is T3, maar zal uiteindelijk afhangen van network congestion*

### Scan detection
- Slechte timing/speed kan aandacht trekken
- Timing hangt ook af van soort scan (bv. veel version scans zijn verdacht)