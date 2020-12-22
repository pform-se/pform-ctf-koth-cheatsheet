# PFORM - CTF / KOTH - Cheatsheet

- Recon 

sudo nmap -Pn -sV -sC -oN nmap.log IP

sudo gobuster -dir -e php,html,htm,txt,log,conf -u http://IP -w /opt/wordlists/directory-list-2.3-medium.txt

# Connect to hosts

-RDP

I think remmina is slow, compared to xfreerdp.
makes it possible to copy and paste from attackermachine to victim machine

pform@attackerbox~ : xfreerdp +clipboard +window-drag /u:username /p:password /v:ip
