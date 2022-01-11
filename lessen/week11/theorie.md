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
    - **Pass the hash** (inloggen via hash i.p.v. plaintext wachtwoord)

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
#### Windows
- Hashes worden opgeslagen in Security Account Manager (SAM)
- 2 vormen:
    - **LANMAN**
        - Oud
        - Kwetsbaar
    - **NT Hash**
        - Nieuw
        - Verschillende versies
        - Kan nog steeds gekraakt worden

#### LANMAN Hash
- Splitsen van ww voor hashen maakt het makkelijker te kraken
- Stappen:
1. Maak ww 14 tekens lang (inkorten of aanvullen null bytes)
2. Maak ww uppercase
3. Splits ww in 2 7-byte delen
4. Voeg 0 bit na elke 7e bit (=> totaal 8 bytes)
5. Encrypteer elk deel met DES
6. Hang beide 8 bytes delen aan elkaar (=> 16 bytes LANMAN hash)


#### NT LM Hash
- Maakt MD4 hash van volledig ww naar 16 bytes hash
- Geen splitsen
- Geen salt (rainbow table attacks zijn easy)


#### LANMAN & NTLMv1 Authentication
- Geen wederzijdse authenicatie
- Geen beveiliging tegen replay/relay attacks MitM atttack
- NTLMv2 is veiler
    - Server moet zich eerste autheniceren
    - MD5 i.p.v. DES
- Kerberos is nog beter systeem
- Stappen
1. Voeg 5 bytes toe aan hash (=> 21 bytes totaal)
2. Splits 21 bytes in 3 groepen van 7 bytes
3. Server stuur 8 byte challenge om te autheniceren
4. DES encrypteer challenge 3x met 7 bytes groepen als key (=> 3x 8 bytes geëncrypteerde challenge = response)
5. Stuur response naar server voor authenicatie


#### Linux
- Gebruikt crypt(3)
- Gebruikt verschillende algoritmes (meerdere keren)
- Laatste ronde produceert hash
- Voegt salt toe aan ww
- Formaat: `USERNAME:$[hashtype]$[64b_salt]$[hash_output]`
- Attacks
    - Rainbow table opstellen/zoeken met specifieke salt
    - Brute force collision attack



### Getting Password Hashes
#### Linux
- User info en ww worden opgeslagen in 2 files:
    - /etc/passwd   (read: iedereen)
    - /etc/shadow   (hashes, read: root)
- John the Ripper haalt info uit beide files


#### Windows
- Verschillende mogelijkheden:
    - Pwdump
        - Admin rechten gebruiken om hashes te extracten via protocollen en API's
    - Metasploit Meterpreter hashdump
        - Sessie met admin rechten nodig
        - Volledig in geheugen
    - Mimikatz (dump from memory)
        - Sessie met admin rechten nodig
        - Haalt authenticatie info uit geheugen (hash en plaintext ww)
    - Volume Shadow Copy Service (VSS) op domain controller
        - Shell met admin rechten nodig
        - Kopieert ntds.dit files
    - Sniff network traffic naar authenicatie challenge/response
        - Via MitM attack (ARP/DNS poisoning, Port mirroring)


### John the Ripper
- Password cracker tool
- Password formaten:
    - Linux: DES, MD5, Blowfish
    - Windows: LANMAN, (NT met jumbo patch)
- 3 methodes:
    - Single (gebruikt default settings en files)
    - Wordlist (eigen wordlist of variatie op bestaande lists)
    - Incremental (brute force volgens bepaalde regels)
- Files:
    - john.conf (config)
    - john.pot (gekraakte ww)
    - john.rec (slaat sesssie op indien crash/interupt)


### Hashcat
- Password cracker tool
- Gebruikt GPU => betere performance
- Attack modes:
    - Straight attack (wordlist attack)
    - Combinator attack (combineert woorden uit wordlists)
    - Mask attack (smart brute forcing)
    - Hybrid attack (combo wordlist en bepaalde brute force regels)
    - Rule-based attack (REGEX regels)
    - Toggle-Case attack (upper/lowercase variaties van wordlists)
- Voor mangeling met wordlists is John the Ripper beter
