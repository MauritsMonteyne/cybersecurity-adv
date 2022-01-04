# Lab 2: DNS/TCP Dump

## DNS
DNS is a complex distributed system, where billions of clients get replies from countless servers. We approach DNS from a client's point of view: what kind of information can we get? Can this information be used to explore the network of an organisation - a.k.a. a form of **reconnaissance**.

### Basic DNS queries
#### Nslookup
A DNS client is used by almost every application: an underlying program asks a DNS server which IP address corresponds to the URL specified in the program. This process is called resolving. Although occurring in the background, this resolving occurs with almost every request that the computer starts to the outside world - you just don't notice it... Manually, a DNS request can also be triggered with a program like `nslookup`:
```
comnet1@home:~$ nslookup www.ugent.be
Server:  ugdns1.ugent.be
Address:  157.193.40.37

Non-authoritative answer:
Name:   www.ugent.be
Address: 157.193.43.50
```

When resolving, the default DNS server is used here, which is a setting on the host (e.g. done by DHCP). The name and IP address of the DNS server providing us with this information is displayed first, followed by the name and IP address of the URL that was requested.

If you explicitly want to use another server to request information, you can do this by also specifying the server address (or name) to the command:
```console
comnet1@home:~$ nslookup <URL-to-question> [DNS-server IP-address or name]
comnet1@home:~$ nslookup www.ugent.be 157.193.40.42
Server:  ugdns2.ugent.be
Address:  157.193.40.42

Non-authoritative answer:
Name:    www.ugent.be
Address:  157.193.43.50
```

Besides the `nslookup` command, which works on both Linux and Windows, Linux users can also use the `host` command (shorter output), or the `dig` command (extensive output).

#### Dig
Using `dig` you can also request additional information, such as e.g. the servers responsible for the details of a particular domain (a.k.a. the authoritative server):
```console
comnet1@home:~$ dig +short NS ugent.be
ugdns3.ugent.be.
ugdns1.ugent.be.
ugdns2.ugent.be.
ns.belnet.be.
```

A reverse lookup - converting an IP address to its DNS name - can be done with the -x option:
```console
comnet1@home:~$ dig +short -x 157.193.43.50
portlvs.ugent.be.
```

#### DNS lookup questions
1. Which IP address does [www.hogent.be](www.hogent.be) have? Which servers are responsible for this domain?
    ```console
    maurits@osboxes:~$ dig +short www.hogent.be
    hogent.be.
    193.190.173.132
    maurits@osboxes:~$ dig +short NS www.hogent.be
    hogent.be.
    ns3.belnet.be.
    ens2.hogent.be.
    ns2.belnet.be.
    ns1.belnet.be.
    ens1.hogent.be.
    ```

2. [www.belnet.be](www.belnet.be) returns an IP address, but if you try to convert this IP address into a URL (reverse lookup), you notice that the server has a different name. Which? Explain how you found this.
    ```console
    aurits@osboxes:~$ dig +short www.belnet.be
    217.19.230.167
    maurits@osboxes:~$ dig +short -x 217.19.230.167
    217.19.230.167.static.hosted.by.combell.com.
    ```

3. Resolve the URL [www.tinder.com](www.tinder.com) several times in succession, using different name servers. Repeat the same on the home server (log in with SSH), which uses other DNS servers. Describe what you see - why does a large company operate this way?
    ```
    De IP adressen veranderen afhankelijk van DNS server. Redundancy, load balancing,faster resolving.
    ```

### DNS queries beyond A records
The above examples give you 'normal' DNS information. However, consulting other types of DNS records can give you other information about the domain you are exploring. Consult https://simpledns.plus/help/dns-record-types to have an idea of the different possibilities in record types. You can explore the options you now have to start your **reconnaissance** of some domains.

1. Can you find the authoritative servers for the domain `hogent.be`?
    ```console
    maurits@osboxes:~$ dig +short NS hogent.be 
    ns3.belnet.be.
    ens2.hogent.be.
    ns2.belnet.be.
    ns1.belnet.be.
    ens1.hogent.be.
    ```

2. Repeat for `belgium.be`. Can you tell which cloud platform is being used for the servers of this domain?
    ```console
    maurits@osboxes:~$ dig +short NS belgium.be
    dns2s.belgium.be.
    dns1w.fgov.be.
    dns3a.westeurope.cloudapp.azure.com.

    => Azure
    ```


3. Repeat for stad.gent. Can you tell which provider is receiving the tax payers money in the city of Ghent?
    ```console
    maurits@osboxes:~$ dig +short NS stad.gent
    ns4.combell.net.
    ns3.combell.net.

    => Combell
    ```

4. Can you discover which company is providing the e-mail software for hogent.be. based on the DNS info?
    ```console
    maurits@osboxes:~$ dig +short MX hogent.be
    0 hogent-be.mail.protection.outlook.com.

    => Microsoft
    ```

5. Repeat for vlaanderen.be
    ```console
    maurits@osboxes:~$ dig +short MX vlaanderen.be
    2 vlaanderen-be.mail.protection.outlook.com.

    => Microsoft
    ```

#### DNS (mis)information
For the next exercises, you will have to log in onto the server of the previous lab, `157.193.215.170`. You can then continue working from that server.

First, let us explore how DNS information might be untrustworthy:

1. Make a normal A record request for www.facebook.com. Try to surf to the IP address that you got as a result (on your own host).
    ```console
    <hogent-login>@home:~$ nslookup www.facebook.com
    Server:		157.193.215.2
    Address:	157.193.215.2#53

    Non-authoritative answer:
    www.facebook.com	canonical name = star-mini.c10r.facebook.com.
    Name:	star-mini.c10r.facebook.com
    Address: 179.60.195.36
    ```

2. Second, repeat this request, but now ask this to DNS server 157.193.215.171 (only accessible from the 157.193.215.170 server). Again, try to surf to the resulting IP address. Reading https://en.wikipedia.org/wiki/DNS_hijacking#Rogue_DNS_server, can you understand why this would be configured in a DNS server? How could this be misused? How could it be used for the good? More information on DNS hijacking, and the difference with hosts-poisoning can be found in the endnotes.
    ```console
    <hogent-login>@home:~$ nslookup www.facebook.com 157.193.215.170
    Server:		157.193.215.171
    Address:	157.193.215.171#53

    Name:	www.facebook.com
    Address: 217.19.234.204

    ```
    ```
    Bad: Hijacking internet connection, man-in-the-middle attack, domain blokkeren
    Good: ?
    ```

When a DNS server is redundant (master/slave), the entire zone can be transferred from the primary server to the secondary server. Nothing wrong there: it is a feature of DNS - see https://en.wikipedia.org/wiki/DNS_zone_transfer However, when badly configured, the content of the entire zone might be accessible to everybody on the Internet. Read https://digi.ninja/projects/zonetransferme.php for further explanation.

1. Again, from the server, can you transfer the entire zone facebook.com from the 157.193.215.171 DNS server? Which IP address corresponds to the CTF (capture-the-flag) hidden in there?
    ```console
    <hogent-login>@home:~$ dig axfr facebook.com @157.193.215.171

    ; <<>> DiG 9.10.3-P4-Debian <<>> axfr facebook.com @157.193.215.171
    ;; global options: +cmd
    facebook.com.		86400	IN	SOA	localhost. root.localhost. 1 604800 86400 2419200 86400
    facebook.com.		86400	IN	NS	localhost.
    facebook.com.		86400	IN	A	217.19.234.204
    ctf.facebook.com.	86400	IN	A	8.8.4.4
    www.facebook.com.	86400	IN	A	217.19.234.204
    facebook.com.		86400	IN	SOA	localhost. root.localhost. 1 604800 86400 2419200 86400
    ;; Query time: 15 msec
    ;; SERVER: 157.193.215.171#53(157.193.215.171)
    ;; WHEN: Tue Jan 04 17:10:04 CET 2022
    ;; XFR size: 6 records (messages 1, bytes 197)
    ```
    ```
    8.8.4.4
    ```


## Capturing on the CLI: tcpdump
Wireshark is a great tool, but only works in a GUI environment. When a CLI is the only thing you have (e.g. on a remote server), tcpdump might be your friend in trying to understand the network traffic.

### Capturing HTTP/HTTPS traffic
In Wireshark, you can capture traffic and then start filtering in all the content. In tpcdump, you typically try to limit the number of packets you get on your CLI by building capture filters . Let's build up the options.

1. Go through the man page of tcpdump. How can you find all available interfaces on your device? Which one will you use? When in doubt, use any.
    ```console 
    maurits@osboxes:~$ tcpdump -D
    1.enp0s3 [Up, Running]
    2.lo [Up, Running, Loopback]
    3.any (Pseudo-device that captures on all interfaces) [Up, Running]
    4.bluetooth-monitor (Bluetooth Linux Monitor) [none]
    5.nflog (Linux netfilter log (NFLOG) interface) [none]
    6.nfqueue (Linux netfilter queue (NFQUEUE) interface) [none]
    ```
    ```
    enp0s3
    ```

2. Now let's try to capture a plain HTTP GET request. Use the command curl http://www.test.atlantis.ugent.be to issue an HTTP GET request. How can you make sure you'll only see HTTP traffic (both requests and responses)? Hint: https://ds-lab-05.cyberwarrior.com/filteringtraffic/
    ```console
    maurits@osboxes:~$ curl http://www.test.atlantis.ugent.be
    ...
    ```

    ```
    Filter wireshark op "http"
    ```

3. How do you create a trace file of this? How can you view this file with tcpdump afterwards?
    ```
    Save capture file als `.pcap` bestand.
    ```
    ```console
    maurits@osboxes:~$ tcpdump -r trace_file.pcap
    ...
    ```

4. How can you view this file with wireshark?
    ```
    File > Open
    ```

The above is basic filtering. Using curl, you could even fake a login. E.g. `curl -X POST -d 'uname=TEST&pass=PASSWORD' http://example.com` can simulate a login on a site. As this site is HTTP and not HTTPS, so you should be able to see the credentials you used in plaintext. How could you show the contents of the packets in tcpdump? There are multiple ways! Consult the man page!

ou can test this with an actual HTTP login at http://testphp.vulnweb.com/login.php .

1. You'll notice there will be a lot of output on the terminal line. Can you add a filter to tcpdump so we only see the HTTP POST request, but not any responses? Think about direction of requests and answers!

    ```console 
    maurits@osboxes:~$ sudo tcpdump -i enp0s3 -s 0 -A 'tcp[((tcp[12:1] & 0xf0) >> 2):4] = 0x504F5354'
    tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
    listening on enp0s3, link-type EN10MB (Ethernet), capture size 262144 bytes
    17:36:22.407946 IP osboxes.53310 > ec2-44-228-249-3.us-west-2.compute.amazonaws.com.http: Flags [P.], seq 3791969654:3791969840, ack 2894356956, win 502, options [nop,nop,TS val 717206503 ecr 3096114983], length 186: HTTP: POST /login.php HTTP/1.1
    E.....@.@.M     ....,....>.P...v..a......3.....
    *......'POST /login.php HTTP/1.1
    Host: testphp.vulnweb.com
    User-Agent: curl/7.68.0
    Accept: */*
    Content-Length: 24
    Content-Type: application/x-www-form-urlencoded

    uname=TEST&pass=PASSWORD
    ```

2. Let's use the terminal to filter only the relevant information. You can combine use -l option and pipe it to grep for realtime filtering. Try it out. See if you can find the credentials or data you used in plaintext everytime you submit a login or form.
```
TODO
```

To try this with HTTPS, you need to filter on the HTTPS port. Which port is this? If you start `tcpdump` on this port, you'll probably see a lot of random traffic. This is because HTTP largely unused these days, so until now it wasn't really necessary to filter the output. HTTPS is used by various software on your device, so it is best to filter the traffic and limit it to certain clients/servers. However, `tcpdump` can only filter on IP addresses. You will need to do a manual lookup if you are using URLs (as you have just learned).

1. Build a tcpdump filter which filter out all the HTTPS traffic if you e.g. surf to https://www.hogent.be using curl.
    ```console
    maurits@osboxes:~$ sudo tcpdump -i enp0s3 -s 0 'tcp port https'
    ```

2. Can you try to retrieve your credentials now? Why (not)?
    ```
    Nee, de packets zijn geëncrypteerd door HTTPS
    ```

### Capturing DNS traffic
HTTP(S) is the majority of Internet traffic; however no URL will be working without the DNS system. Now let's combine out knowledge of lookups, and build a DNS filter in tcpdump.

1. Which port (and protocol) will you use to build up a filter which only shows you the DNS traffic?
    ```
    DNS, port 53
    ```

2. Lookup the URL '[www.hogent.be](www.hogent.be)', asking it to DNS server 8.8.4.4. Can you tcpdump this request (only)? How many DNS packets do you see?
    ```console
    maurits@osboxes:~$ sudo tcpdump -i enp0s3 -s 0 'udp port 53 and host 8.8.4.4'
    17:54:33.897359 IP osboxes.52658 > dns.google.domain: 22692+ A? www.hogent.be. (31)
    17:54:33.922459 IP dns.google.domain > osboxes.52658: 22692 2/0/0 CNAME hogent.be., A 193.190.173.132 (61)
    17:54:33.926962 IP osboxes.50732 > dns.google.domain: 15083+ AAAA? hogent.be. (27)
    17:54:33.957450 IP dns.google.domain > osboxes.50732: 15083 0/1/0 (79)
    ```

3. Repeat this for the load-balanced URL of [www.tinder.com](www.tinder.com). How many packets do you now see? Where can you find the information of the multiple IP addresses that are given as an answer to your lookup client?
    ```console
    maurits@osboxes:~$ sudo tcpdump -i enp0s3 -s 0 'udp port 53 and host 8.8.4.4'
    tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
    listening on enp0s3, link-type EN10MB (Ethernet), capture size 262144 bytes
    ...
    17:57:28.710197 IP dns.google.domain > osboxes.51288: 22076 4/0/0 A 52.222.174.2, A 52.222.174.72, A 52.222.174.20, A 52.222.174.41 (96)
    ...
    ```
