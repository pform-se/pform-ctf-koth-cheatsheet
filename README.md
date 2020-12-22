# PFORM - CTF / KOTH - Cheatsheet

- Recon 

sudo nmap -Pn -sV -sC -oN nmap.log IP
- RDP

I think remmina is slow, compared to xfreerdp.
makes it possible to copy and paste from attackermachine to victim machine


pform@attackerbox~ : xfreerdp +clipboard /u:username /p:password /v:ip
