# PFORM - CTF / KOTH - Cheatsheet

- RECON 

<code><b>pform@attacker~: </b>sudo nmap -Pn -sV -sC -oN nmap.log IP</code>

<code><b>pform@attacker~: </b>sudo gobuster -dir -e php,html,htm,txt,log,conf,flag -u http://IP -w /opt/wordlists/directory-list-2.3-medium.txt</code>

# Connect to hosts

- RDP

I think remmina is slow, compared to xfreerdp.
makes it possible to copy and paste from <b>attacker machine</b> to <b>victim machine</b>

<code><b>pform@attacker~: </b>xfreerdp +clipboard +window-drag /u:username /p:password /v:ip</code>
