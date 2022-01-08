# Week 9
## Week 36 - Exploit Foundations / Lectures 5-5 to 5-8
### Metasploit
Link: https://learning.edx.org/course/course-v1:NYUx+CYB.PEN.2+1T2021/block-v1:NYUx+CYB.PEN.2+1T2021+type@sequential+block@f635a9a4a9164514a6c8e4b6d56d6e4d/block-v1:NYUx+CYB.PEN.2+1T2021+type@vertical+block@3e9b607941ed48d08e2585105ec171f9


**Metasploit**
- Exploitation tool voor het geautomatiseerd uitvoeren van gekende vulnerabilities
- Heeft tal van modules (exploits, payloads)
- Managed en organiseerd alles mooi in 1 framework
- Easy to use
- Zeer krachtige tool, maar:
    - Zijn gekende exploits
    - Vaak zijn exploits out-of-date of oud
    - Niet makkelijk te troubleshooten door automatisering
    - Eerder "Script kiddy"-achtig
- Beter andere tool gebruiken bij het zoeken naar nieuwe exploits


### Metasploit Basics
- Modules:
    - Exploit
    - Payload
        - Singles (inline)
        - Stager
        - Stage
    - Auxiliary (vooral scanners)
    - Post exploitation


**Active exploitation**
- Vooral gericht op services
- Service luistert naar open connecties (passief)
- Attacker probeert dit de exploiteren, heeft controle over uitvoering (actief)

**Passive exploitation**
- Vooral gericht op client software
- Attacker moet exploit op target apparaat krijgen (url, download etc)
- Target moet de exploit activeren (actief)
- Attacker luistert naar open connecties (passief), heeft geen controle over uitvoering 


**Payload Stager types**:
- **Reverse**: target doen verbinden met attacker listener (voorkeur)
- **Bind**: opzetten listener bij target

**Payload Stager protocols**:
- TCP
- HTTP
- HTTPS


**Meterpreter**
- Metasploit interpreter
- Soort van shell
- Injecteerd DLL's rechtstreeks in geheugen
- Ontwijkt anti-virus
- Schrijft niet naar drive
- Alle communicatie is geëncrypteerd