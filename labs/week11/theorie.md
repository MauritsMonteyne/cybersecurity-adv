# Week 11
## Week 44 - Passwords - Lecture slides 1 to 8
Link: https://learning.edx.org/course/course-v1:NYUx+CYB.PEN.3+1T2021/block-v1:NYUx+CYB.PEN.3+1T2021+type@sequential+block@e9b09e0e7546437e9405c87d834059d3

### Password attacks
- Voor veel zaken worden wachtwoorden gebruikt (en hergebruikt)
- Patroon vinden in wachtwoord kan andere wachtwoorden opleveren
- Als we wachtwoord kunnen te weten komen, hoeven we geen exploit te gebruiken
- 2 grote categoriën:
    1. **Password guessing**
    2. **Password cracking**
- Andere methodes:
    - **Keylogger**
    - **Authentication sniffing** (telnet, http, ftp)
    - **Pass the hash**

**Password Entropy:** maat voor hoe sterk/onvoorspelbaar een wachtwoord is. Formule: `2^n` met n aantal mogelijk symbolen

#### Password guessing
- Brute force methode
- Kan veel aandacht trekken (logs, network traffic)
- Kan accounts blokkeren / errors genereren (=> aandacht trekken)
- Traag door interactie met authenticatie systeem

#### Password cracking
- Reverse engineren cryptography van gestolen password hashes
- Case, getallen en speciale tekens kunnen verloren gaan bij hashen (rekening mee houden)
- Gebruikt eigen hardware (sky is the limit)
- Soms ook brute force


### Wordlists
- Lijst van woorden uit verschillende bronnen van leaked wachtwoorden uit verschillende bronnen
- Kan handig zijn lijst aan te passen afhankelijk van wachtwoord regels van organisatie
- Te vinden op het internet (vaak gratis) of zelf op te stellen
- Voorkeur naar korte specifiek lijst dan lange lijst


### Account lockout
- Password guessing kan resulteren in DoS tegen jezelf
- Typisch settings voor lockout:
    - Threshold: hoeveel keer proberen?
    - Duration: Tijdelijk? Permanent?
    - Observation windows: wanneer reset aantal foute pogingen?
- Default geen permamente lockout voor admin (anders full system lockout mogelijk)


### Password Guessing THC-Hydra
- Password guessing tool (CLI en GUI)
- Ondersteunt veel verschillende protocollen
- Kan gebruikt worden tegen 1 of meerdere users, apparaten of wachtwoorden 


### Password Formats and Hashes
