# Lab 4: HackTheBox labs
Let's apply what we have learned so far on https://hackthebox.eu . Create an account and go to [starting point](https://app.hackthebox.eu/starting-point) .

You'll notice there are 2 tiers: tier 0 and 1. Each has 3 free machines available to which you can connect. In order to connect to these machines, you'll need to open an OpenVPN connection to the network of Hack the Box by downloading and importing an .ovpn file. We advise you to work from within your Linux VM (as it already has OpenVPN installed). Alternative is to work on your host system, installing OpenVPN and using the console from there. More information can be found [here](https://help.hackthebox.eu/en/articles/5185687-gs-introduction-to-lab-access) . If you still have difficulties, you can find video tutorials on [youtube](https://www.youtube.com/watch?v=t61ibWfpmUI) .

Can you solve the challenges of the following machines?

Tier 0 "the key is a strong foundation"
- Meow: nmap + telnet
- Fawn: nmap + FTP
- Dancing: nmap + SMB

Tier 1 "you need to walk before you can run"
- Appointment: SQL injection + gobuster
- Sequel: MariaDB
- Crocodile: nmap + FTP + gobuster

Tip: the starting point machines have walkthroughs available in pdf which you can consult if you're stuck. Look for the "*Download Walkthrough*" button.