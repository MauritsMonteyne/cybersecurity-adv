# Lab 1: Wireshark intro

The following lab presumes that you are working in a Linux VM, which has both Wireshark and Telnet installed as packages.
For your convenience, you can download such a Linux Mint VM and import it in VirtualBox.
Login: `osboxes` ; passwd: `osboxes.org` (indicating the origin of the VM as well)


## Exploring a first trace file

**CSA - lab1 Wireshark intro 1 ICMP.pcap**

Understanding packet trace files isn't done by the software: it's done by you. Think of how DNS is working, on what ICMP is all about (remember Computer Networks 1 ...).

Open this trace file. Try to analyse which command has lead to this stream of packets. Be as precise as you can be.


## Capturing your own DHCP traffic

Within the Linux VM we have provided, you should perform your first "live capture". Working within this VM gives you the advantage that all IP traffic from your "host laptop" isn't included in your capture - only the VM traffic will be there.


### Capture Instructions

- Flush the IP address on your network card: `sudo ip addr flush enp0s3`
- Start Wireshark. Capture traffic on your interface **enp0s3**.
- Let the DHCP client process negotiate a new IP address: `sudo dhclient`


### Questions related to your capture
1. Are DHCP messages sent over UDP or TCP?
    ```
    UDP, want TCP ondersteunt geen broadcasting wat nodig is bij DHCP
    ```

2. What is the IP address of your DHCP server?
    ```
    10.0.2.2
    ```

3. Draw a timing datagram illustrating the sequence of the first four-packet Discover/Offer/Request/ACK DHCP exchange between the client and server. For each packet, indicated the source and destination port numbers.
    ```
    1) Discover:    Src – port 68, Dst – port 67

    2) Offer:       Dst – port 67, Src – port 68

    3) Request:     Src – port 68, Dst – port 67

    4) ACK:         Dst – port 67, Src – port 68
    ```

4. A host uses DHCP to obtain an IP address, among other things. But a host’s IP address is not confirmed until the end of the four-message exchange! If the IP address is not set until the end of the four-message exchange, then what values are used in the IP datagrams in the four-message exchange? For each of the four DHCP messages (Discover/Offer/Request/ACK DHCP), indicate the source and destination IP addresses that are carried in the encapsulating IP datagram.
    ```
    Discover:    Src – 0.0.0.0, Dst – 255.255.255.255 (=Broadcast address)

    Offer:       Dst – 192.168.0.1, Src – 255.255.255.255

    Request:     Src – 0.0.0.0, Dst – 255.255.255.255

    ACK:         Dst – 192.168.0.1, Src – 255.255.255.255
    ```

5. Explain the purpose of the lease time. How long is the lease time in your experiment?
    ```
    Bepaalt hoe lang een apparaat het gegeven IP adres krijgt. 1 uur.
    ```


## Analysing a HTTP capture
**CSA - lab1 Wireshark intro 2 HTTP basic.pcap**

Let’s begin our exploration of HTTP by opening the provided packet trace file. Look at the information in the HTTP GET and response messages. Hint: you can build a filter so that you only see the HTTP messages (and not the other tcp info).

By looking at the information in the HTTP GET and response messages, answer the following questions. When answering the following questions, you should display the GET and response messages and indicate where in the message you’ve found the information that answers the following questions.

1. Is the browser running HTTP version 1.0 or 1.1? What version of HTTP is the server running?
    ```
    Request Version: HTTP/1.1
    ```

2. What languages (if any) does your browser indicate that it can accept to the server?
    ```
    Accepted-Language: en,nl
    ```

3. What is the IP address of your computer? Of the gaia.cs.umass.edu server?
    ```
    Computer: 157.193.215.200
    Server: 128.119.245.12
    ```

4. What is the status code returned from the server to your browser?
    ```
    Status Code: 200
    ```

**CSA - lab1 Wireshark intro 3 HTTP with objects.pcap**

1. How many HTTP GET request messages did your browser send? To which Internet addresses (URL) were these GET requests sent?
    ```
    4
    - [Full request URI: http://gaia.cs.umass.edu/wireshark-labs/HTTP-wireshark-file4.html]
    - [Full request URI: http://gaia.cs.umass.edu/pearson.png]
    - [Full request URI: http://manic.cs.umass.edu/~kurose/cover_5th_ed.jpg]
    - [Full request URI: http://caite.cs.umass.edu/~kurose/cover_5th_ed.jpg]
    ```


2. Have a look at packet 20. Which HTTP return code did you receive? What does this indicate?
    ```
    Status Code: 302, resource found, maar wel een redirect
    ```


3. Can you tell whether your browser downloaded the two images serially, or whether they were downloaded from the two web sites in parallel? Explain.
    ```
    Serieel, het 2e GET request start pas na de reply op het 1e GET request.
    ```


## Capture your own telnet / SSH traffic

We will now log in to a remote server, and capture the traffic related to this (TCP) connection. We will connect to a Telnet server on the one hand, and a SSH server on the other hand.

The server we will test on the Internet has IP address `157.193.215.170`.

Your credentials are your HoGent login name (e.g. 945435bc); password idem.

### Telnet Capture Instructions
- Open a CLI window. Before logging in, test your connection to the server telnet `<server IP address> 23`
Do not log in, but interrupt your connection - it is merely a test!
- Next, start WireShark. Capture traffic on your interface **enp0s3**.
- Start your telnet connection again to the server. Now, log in using your credentials. Have a look at the files in your home folder. Then log out.
- Stop capturing.

### Questions related to your Telnet capture
Using a display filter might be useful: https://wiki.wireshark.org/DisplayFilters can give you a hint on how to filter on a subset of the captured packets.

1. Can you filter on the IP address of the server?
    ```
    Ja, ip.addr == 157.193.215.17
    ```

2. What was the (TCP) destination port that you have connected to? What was the client port used by your client (for this session)?
    ```
    Destination port: 23
    Client port: 34986
    ```

3. Right click, and try to re-assemble the entire TCP stream of your connection. Describe what you can deduct from the displayed information.
Input and output of the telnet session.
    ```
    ..... ..#..'..... ..#..'.. .....#.....'........... .38400,38400....#.osboxes:0....'..DISPLAY.osboxes:0......xterm-256color................!.............v.7.......!......Debian GNU/Linux 9
    home login: <hogent-login><hogent-login>
    .
    Password: <hogent-login>
    .
    Linux home 4.9.0-15-amd64 #1 SMP Debian 4.9.258-1 (2021-03-08) x86_64

    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.
    .]0;<hogent-login>@home: ~..[01;32m<hogent-login>@home.[00m:.[01;34m~.[00m$ llss
    .
    README
    .]0;<hogent-login>@home: ~..[01;32m<hogent-login>@home.[00m:.[01;34m~.[00m$ llss  --aall
    .
    total 40
    drwxr-xr-x   2 <hogent-login> homeCN  4096 Sep 30 12:28 .[0m.[01;34m..[0m
    drwxr-xr-x 217 root     root   20480 Sep 30 12:58 .[01;34m...[0m
    -rw-r--r--   1 <hogent-login> homeCN   220 Dec 30  2012 .bash_logout
    -rw-r--r--   1 <hogent-login> homeCN  3526 May 15  2017 .bashrc
    -rw-r--r--   1 <hogent-login> homeCN   675 Dec 30  2012 .profile
    -rw-r--r--   1 <hogent-login> homeCN    80 Sep 22 16:52 README
    .]0;<hogent-login>@home: ~..[01;32m<hogent-login>@home.[00m:.[01;34m~.[00m$ ccaatt  RREE	ADME 
    .
    Welcome to this (vintage) server, merely installed for your learning potential.
    .]0;<hogent-login>@home: ~..[01;32m<hogent-login>@home.[00m:.[01;34m~.[00m$ eexxiitt
    .
    logout
    ```

4. Is setting up a Telnet connection method to e.g. a Cisco router or switch a good practice?
    ```
        Nee, is in plain text zonder encryption.
    ```

### SSH Capture Instructions
- Open a CLI window. Before logging in, test your connection to the server telnet `<server IP address> 22` # the port number of SSH
Do not log in, but interrupt your connection - it is merely a test!
- Next, start WireShark. Capture traffic on your interface enp0s3.
- Start your SSH connection to the server. ssh `<login name>@<server IP address>`
- Again, log in using your credentials. Have a look at the same files in your home folder. Then log out.
- Stop capturing.

### Questions related to your SSH capture
1. What was the (TCP) destination port that you have connected to? What was the client port used by your client (for this session)?
    ```
    Destination port: 22
    Client port: 58982
    ```

2. Right click, and try to re-assemble the entire TCP stream of your connection. Describe what you can deduct from the displayed information.
    ```
    Niets, communicatie is geëncrypteerd.
    ```
