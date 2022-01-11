# Week 9
## Week 39 - Web Application Testing / lecture 8-1 to 8-7
Link: https://learning.edx.org/course/course-v1:NYUx+CYB.PEN.2+1T2021/block-v1:NYUx+CYB.PEN.2+1T2021+type@sequential+block@dbaf3b6058484ada852bf2aae5fa708d/block-v1:NYUx+CYB.PEN.2+1T2021+type@vertical+block@872b70278e5f4487a98abf852660c646

### Web app testing
### Where does Web App Testing fit in
- Pen Testing verwijst vaak naar testen en exploiteren van networks en hosts
- Web App Testing wordt gezien als apart discipline omdat het andere kennis vereist
    - Development kennis i.p.v. networking
    - Scope soms beperkt (bv. enkel frontend, niet servers/network)
- Vaak bug bounty programma's


### Web History and Technologies
#### History of the Web
- Geen beveiliging en encryptie
- Geschienden van web is een iteratieve cycle

#### HTTP History
- HTTP/0.9
    - Enkel GET requests
    - Enkel HTML
- HTTP/1.0
    - Headers, metadata, 
    - POST requets
    - Extra file types
- HTTP/1.1
    - Persistente verbinding
- HTTP/2
    - Alles binaire data (geen plaintext)
    - TLS

#### Requests
| Request | Uitleg |
| :--- | :--- |
| GET | request files |
| POST | stuur data |
| HEAD | idem als GET, maar enkel de header|
| OPTION | request server options |
| PUT | voeg nieuwe resource toe (idempotent) |
| DELETE | delete bestaande resource |
| TRACE | request data echoed back |
| PATCH | Wijzig bestaande resource |
| CONNECT | Maak web socket van connectie |

#### GET issues
- Values en variables staan in URL (makkelijk aan te passen)
- Makkelijk om "evil" URL te maken
- Request kunnnen opgeslagen worden (inclusief gevoelige data)

PUT, DELETE en TRACE kunnen ook misbruikt worden


#### User Agent String
- Identificeert browser en geeft systeem details aan server
- Wordt gebruikt om specifieke pagina's te laden (mobile) of doorverwijzen naar app


#### HTTP Errors
| Error | Status |
| :--- | :--- |
| 1xx | Info respone |
| 2xx | Succes |
| 3xx | Redirection |
| 4xx | Client error |
| 5xx | Server error |


#### Uniform Resource Identifier (URI)
- Manier of resources te bereiken
- URL is meest gebruikte URI
- Specifieert toegang mechanisme en network location

Formaat: `protocol://user:pass@host.sub.domain:port/folder/resource?variable=value`


### OWASP
#### Open Web Application Sercurit Project (OWASP)
- Zeer interessante bron met info en tools rond web en app security
- Onderhouden jaarlijkse top 10 lijst van Application Security Risks
- Ook andere interessante top 10's (Mobile etc)
- Onderhouden ZED Attack Proxy tool (vindt automatisch kwetsbaarheden in web app)
- OWASP Cheat Sheet (exploit instructies, voorbeelden en mitigation)


### Web Testing
#### Basic Methodes
- CLI tools & headless browisng (geen GUI)
    - Automatiseren/scripting
- Browser plugins, developer mode
    - Analyseren en aanpassen requests
- Proxies
    - Volledige controle (analyseren,aanpassen en meer)
    - Zed Attack Proxy
    - Burp Suite

#### Burp Suite
- Eén van de meeeste gebruikte web testing tools en proxies


#### Zed Attack Proxy (ZAP)
- Gelijkaardig functionaliteiten als Burp voor proxy
- Bevat extra functionaliteiten:
    - Automation
    - Port scanning
    - Fuzzing, Brute forcing
    - Webcrawler


### Network Zones
#### Network Zones
- "Layered" security
- Implementeerd least privilege (enkel strict noodzakkelijke)
- Doel: schade beperken bij attack
- Zones:
    - Red zone (untrusted)
        - Externe gebruikers
    - Green zone (DMZ)
        - Web, VPN, mail server
    - Blue zone (Trusted)
        - Applicaties, werknemers, non-sensitive data
    - Black zone (Restricted)
        - Management, admin, sensitive data


#### HTTP & Traditional Network Controls
- Firewalls, ACL's, Anti-virus, Intrusion Dectection Systems
- Traditionele beveiling applicaties en toestellen voorkomen geen kwetsbaarheden in applicaties
- Kijken vooral naar Network Layer (OSI model)
- Attacker camofleert zich vaak als "normaal" network traffic


#### Traditional Developer
- Focust vaak op functionaliteit en opleveren
- Focust vaak op "normale" input (erro-handling!)
- Kent framework/library niet altijd super grondig door vele veranderingen
