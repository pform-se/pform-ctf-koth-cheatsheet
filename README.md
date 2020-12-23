<h1>PFORM - CTF / KOTH - Cheatsheet</h1>

<h2>RECON</h2> 

<b>pform@attacker~: </b><code>rustscan -A IP</code>

<b>pform@attacker~: </b><code>sudo nmap -Pn -sV -sC -oN nmap.log IP</code>

<b>pform@attacker~: </b><code>gobuster -dir -e php,html,htm,txt,log,conf,flag -u http://IP -w /usr/share/wordlists/directory-list-2.3-medium.txt</code>

<b>pform@attacker~: </b><code>nikto -Display 1234EP -o report.html -Format htm -Tuning 123bde -host IP</code>

<h2>SMB</h2>

<b>pform@attacker~: </b><code>smbmap -H IP</code>

<b>pform@attacker~: </b><code>smbclient -L \\\\IP</code>

<b>pform@attacker~: </b><code>smbclient -L \\\\IP\\C$ -U guest -W WORKGROUP</code>



<h2>FTP</h2>

Is the host vuln to anonymous login?

<b>pform@attacker~: </b><code>ftp IP</code>

<b>pform@attacker~: </b>Login: <code>anonymous</code> Password: <code>anonymous</code>

Dont forget to check for hidden directory´s  somethimes they hidde them in plain sight like this:

<b>pform@attacker~: </b><code>ls -la</code>

drwxr-xr-x pform pform 4096 Dec 32 00:00 .

drwxr-xr-x pform pform 4096 Dec 32 20:00 ..

drwxr-xr-x pform pform 4096 Dec 32 00:00 ... <b>  < - - - - - - - - - - - - ( 3 dots )</b>

-rw-r--r-  pform pform    0 Dec 32 00:00 random-stuff.txt


<h2>RDP</h2>

I think remmina is slow, compared to xfreerdp. 

<b>+clipboard</b> makes it possible to copy and paste from <b>attacker machine</b> to <b>victim machine</b>

<b>pform@attacker~: </b><code>xfreerdp +clipboard +window-drag /u:username /p:password /v:IP</code>


<h2>To be contiuned...</h2>
