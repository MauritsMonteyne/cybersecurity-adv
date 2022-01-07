# Week 8
## OSINT
Link: https://learning.edx.org/course/course-v1:NYUx+CYB.PEN.1+1T2021/block-v1:NYUx+CYB.PEN.1+1T2021+type@sequential+block@969153ae1a86412ab1d60309ac9a05f8/block-v1:NYUx+CYB.PEN.1+1T2021+type@vertical+block@a934f49523094693b0fcb6ede5543d77

**Open source intelligence (OSINT)**
- Verzamelen en analyseren van gegevens uit publieke bronnen met als doel er bruikbare informatie uit te halen.

### Recon
Veel mogelijke bronnen: Google, Wikipedia, LinkedIn, social media, website organisatie

- Wat doet het bedrijf, wat maakt het, inkomst bron etc
- Job openings / type vacatures
    - Welke software/hardware?
    - Management job? => interne problemen?
- Huidige projecten
- Servers, API's die open staan naar het internet


**Google Search Engine**
- Zeer flexibel
- Zoeken op tal van criteria (title, file type, datum, open "index of")



**Robots.txt**
- Stopbord voor "brave" web-crawlers
- Bevat info over padden op de webserver


**Google Dorking (Google Hack Database)**
- Verzamelingen van Google search parameters die interessante info/leaks opleveren


## Network Recon
Link: https://learning.edx.org/course/course-v1:NYUx+CYB.PEN.1+1T2021/block-v1:NYUx+CYB.PEN.1+1T2021+type@sequential+block@969153ae1a86412ab1d60309ac9a05f8/block-v1:NYUx+CYB.PEN.1+1T2021+type@vertical+block@34bda17b1097474fab1d0318b8a17136

*Enkel passieve methodes*

### Domain Name Recon
**Whois**
- Registratie info van domain
- Contact gegevens, locatie
- DNS servers
- Sub-domains

**Regional Internet Registries**
- Gelijkaardig aan Whois
- Eigenaar IP adres (range)


**Nslookup**
- Info over DNS servers
- DNS record-types (mail/name servers etc)
- Zone transfers
- Cache Snooping attacks (spelen met DNS recursion, wat is wel/niet cached)
    - Web history van organisatie achterhalen
    - Hardware/software achterhalen via update servers

### Private DNS
- Privé DNS server
- 2 types:
    1. DNS over TLS (DoT)
        - Inhoud encrypted
        - Traffic volume zichtbaar
        - Gebruikt specifieke port (853) => easy to detect
    2. DNS over HTTPS (DoH)
        - Niet te onderscheiden van "gewoon" HTTPS traffic
        - Moeilijk te monitoren



## Shodan
Link: https://learning.edx.org/course/course-v1:NYUx+CYB.PEN.1+1T2021/block-v1:NYUx+CYB.PEN.1+1T2021+type@sequential+block@969153ae1a86412ab1d60309ac9a05f8/block-v1:NYUx+CYB.PEN.1+1T2021+type@vertical+block@ac3ef9397a274f3d847cca37735083bb

**Shodan**
- Internet crawler die het internet afzoekt naar network-enabled toestellen en services
- Stelt database op door banner grabbing van meest gebruikt ports
- Users kunnen zoeken in Shodan database 
- Op basis van: 
    - locatie
    - domain
    - protocol
    - network
    - hash
    - favicon
    - etc
- Toont zeer veel interessante info:
    - Open ports
    - Protocol
    - Service
    - OS
    - Software
- Web interface en CLI