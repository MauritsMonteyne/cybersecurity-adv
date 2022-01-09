# Week 4
## Week 3 - How Networks are Attacked - Part II All slides except 20,21,22 and 32
Link: [Week 3 - How Networks are Attacked - Part II
All slides except 20,21,22 and 32](https://learning.edx.org/course/course-v1:NYUx+CYB.NET.1+3T2020/block-v1:NYUx+CYB.NET.1+3T2020+type@sequential+block@2882471384284911aa2d9f522c50fffe)

### Ingress Filtering
- Manier om IP spoofing gedeeltelijk tegen te gaan (Best practise)
- Drop packet als source IP niet behoort tot netwerk
- Effectiviteit hangt af van hoe wijdverspreid implementatie is

**BGP filtering**: ingress filtering op ISP niveau

**Session Hijacking**: TCP connectie overnemenen via spoofing en sniffing technieken

**IETF** (Internet Engineering Task Force): internationale community van industrie professionals die de standaard bepaalt voor internetprotocollen. (Ze stellen RFC's (Request for Comment) op)

### DoS (Denial of Serice)
- Zorgt dat gebruiker bepaalde service niet meer kan bereiken
- 2 methodes:
    - Packet met speciale payload sturen die service doet crashen
    - Overwelmen van bandbreedte en/of resources van service

**DoS (Denial of Service) Attack**
- Sturen van meerdere gespeciale packets 
- Doel: applicatie doen crashen
- Source: 1 spoofed IP adres
- Overwelm target met absurde requests

**DDoS (Distributed Denial of Service) Attack**
- Sturen van veel packets
- Doel: applicatie doen crashen
- Source: meerdere niet spoofed IP adressen (botnet)
- Moeilijk packets te blokkeren door vele sources

Types of Vulnerability Attacks
| Type | Uitleg |
| :--- | :--- |
| Land Attack | Spoofed packet waarvan source en destination adres port zelfde zijn |
| Ping of Death | Stuurt over-sized ping packets |
| Jolt2 | Stuurt stream van fragements. Fragments rebuilden vraagt cpu resources |
| Teardrop, etc | Stuurt overlaping fragmented packets. Fragments rebuilden vraagt cpu resources |

Meeste kwetsbaarheden zijn al gepatcht, maar idee is steeds hetzelfde: niet standaard packet sturen en hopen op slechte error-handling => crash

### Connection Flooding
**SYN flood** 
- Attack op de three-way handshake
- Attacker stuurt veel SYN packets maar geen voltooid handshake niet (= half open connectie)
- Vult geheugen van de server op waardoor nieuwe verbinding niet mogelijk zijn

**SYN Cookies**
- Voorkomen SYN flood attack
- Server maakt hash van source en destination IP adres + random number
- Server trackt connectie niet in memory (zit in cookie)
- Bij ACK berekent server hash opnieuw en vergelijkt met cookie. Legit? => open connectie


### Reflection Attack
- DDoS attack
- Response op een (attacker) request naar target sturen i.p.v. de attacker zelf
- **Source IP = target IP**
- Request gebruikt **UDP protocol**

### Amplification Attack
- Soort reflection attack
- Kort request sturen, maar met **grote response**

**DNS amplification attack**: misbruik maken van DNS server

**NTP amplification attack**: misbruikt maken van **NTP** (Network Time Protocol) server. `monlist` commando geeft response met alle toestellen die recent tijd hebben gevraagd

**memcached DDoS attack**: misbruik maken van memcached server (cache voor internet traffic)


### Defending Against DDoS
1. Laat systemen geen bots worden
2. Filter bad packets
3. Beschik over meer resources dan de attacker
4. Maakt gebruikt van distributed infrastructuur


### DNS Attacks
**DNS Poisoning**
- Meeste DNS servers cachen hun entries
- Attacker overschrijft deze entries met andere locaties
- Niet eenvoudig upstream DNS request heeft ID, attacker moet:
    - Sequence number raden van request
    - Response authoritative server tegenhouden of sneller zijn
    - Zelf response sturen met spoofed IP adres


### Client Side Attacks

**Foefelen met URL's**
- Gelijkaardige namen
- Verstoppen in html
- Automatisch laden bij openen website

**Phishing**
- Lijkt echt
- Fake brieven, emails
- Malicious PDF

### Reverse Shell
- **Firewall blokkeerd outside connections**, maar niet inside connections
- Firewall blocking omzeilen door **vanop target pc een shell te openen naar de attack pc**
- Shell kan geopend worden via phising mail, malicious link etc
- Vb. Reverse Shell iTunes Exploit


### Tools & Implementation (Relevant?)
- SATAN (Security Admin Tool for Analyzing Networks)
    - Scanning tool
- Nessus Professional
    - Identificiëren vulnerabilities, policy-violating configurations en bruikbare malware
- Nikto2 | CIRT.net
    - Testen van webserver
- CA Veracode Web Application Scanning
    - Ontdekken en schatten van risico's
- Canvas
    - Geautomatiseerd exploitatiesysteem + pentest development framework

### Metasploit
- Meest populaire framework
- Pen Testing software
- Makkelijk deployen en hergebruiken van code/exploits
- Open source


### Offensive Security Issues
**Vulnerability**: kwetsbaarheid in een systeem die beveiliging vermindert
**Exploit**: code die gebruik maakt van vulnerability
**Payload**: code die bezorgd wordt door exploit (draait op target systeem)
**Encoders**: methode om payload te verdoezelen zodat het niet opgemerkt wordt


### Payloads
