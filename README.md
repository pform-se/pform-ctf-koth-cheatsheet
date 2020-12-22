# PFORM - CTF / KOTH - Cheatsheet

<b>RECON<b/> 

<code><b>pform@attacker~: </b>rustscan -A IP</code>

<code><b>pform@attacker~: </b>sudo nmap -Pn -sV -sC -oN nmap.log IP</code>

<code><b>pform@attacker~: </b>sudo gobuster -dir -e php,html,htm,txt,log,conf,flag -u http://IP -w /opt/wordlists/directory-list-2.3-medium.txt</code>

Is SMB active?
<code><b>pform@attacker~: </b>smbmap -H IP</code>


# Connect to BOX

<b>FTP</b>
Is the host vuln to anonymous login?

<b>pform@attacker~: </b><code>ftp IP</code>

<b>pform@attacker~: </b><code>Login: anonymous</code>

<b>pform@attacker~: </b><code>Password: anonymous</code>

<b>RDP</b>

I think remmina is slow, compared to xfreerdp.
makes it possible to copy and paste from <b>attacker machine</b> to <b>victim machine</b>

<code><b>pform@attacker~: </b>xfreerdp +clipboard +window-drag /u:username /p:password /v:IP</code>
