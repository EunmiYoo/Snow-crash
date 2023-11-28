# LEVEL01

1. Login to the level01

```
ssh -i ~/.ssh/id_rsa.pub level01@165.22.17.96 -p 4242
```

2. The password of level01 is the token from level00

```
    x24ti5gi3x0ol2eh4esiuxias
```

3. So, try to apply the same logic as level00

`2>/dev/null : Redirect error messages to null, effectively suppressing them`

```
find / -type f -user "flag01" 2>/dev/null
find / -type f -user "user01" 2>/dev/null
```

=> Nothing came out

4. try another one.

```
find / -type f -user "level01" 2>/dev/null
```

=> with this command, I got the results as below,

=> but nothing is related to the "password" or "flag01" or "user01"

```
level01@SnowCrash:~$ find / -type f -user "level01" 2>/dev/null
/proc/5980/task/5980/fdinfo/0
/proc/5980/task/5980/fdinfo/1
/proc/5980/task/5980/fdinfo/2
/proc/5980/task/5980/fdinfo/255
/proc/5980/task/5980/ns/net
/proc/5980/task/5980/ns/uts
/proc/5980/task/5980/ns/ipc
/proc/5980/task/5980/environ
/proc/5980/task/5980/auxv
/proc/5980/task/5980/status
/proc/5980/task/5980/personality
/proc/6413/io
....
```

4. So, try to find the other command with the password

```
cat /etc/passwd
```

and then I can find the results as below

```
level01@SnowCrash:~$ cat /etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/bin/sh
bin:x:2:2:bin:/bin:/bin/sh
sys:x:3:3:sys:/dev:/bin/sh
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/bin/sh
man:x:6:12:man:/var/cache/man:/bin/sh
lp:x:7:7:lp:/var/spool/lpd:/bin/sh
mail:x:8:8:mail:/var/mail:/bin/sh
news:x:9:9:news:/var/spool/news:/bin/sh
uucp:x:10:10:uucp:/var/spool/uucp:/bin/sh
proxy:x:13:13:proxy:/bin:/bin/sh
www-data:x:33:33:www-data:/var/www:/bin/sh
backup:x:34:34:backup:/var/backups:/bin/sh
list:x:38:38:Mailing List Manager:/var/list:/bin/sh
irc:x:39:39:ircd:/var/run/ircd:/bin/sh
gnats:x:41:41:Gnats Bug-Reporting System (admin):/var/lib/gnats:/bin/sh
nobody:x:65534:65534:nobody:/nonexistent:/bin/sh
libuuid:x:100:101::/var/lib/libuuid:/bin/sh
syslog:x:101:103::/home/syslog:/bin/false
messagebus:x:102:106::/var/run/dbus:/bin/false
whoopsie:x:103:107::/nonexistent:/bin/false
landscape:x:104:110::/var/lib/landscape:/bin/false
sshd:x:105:65534::/var/run/sshd:/usr/sbin/nologin
level00:x:2000:2000::/home/user/level00:/bin/bash
level01:x:2001:2001::/home/user/level01:/bin/bash
level02:x:2002:2002::/home/user/level02:/bin/bash
level03:x:2003:2003::/home/user/level03:/bin/bash
level04:x:2004:2004::/home/user/level04:/bin/bash
level05:x:2005:2005::/home/user/level05:/bin/bash
level06:x:2006:2006::/home/user/level06:/bin/bash
level07:x:2007:2007::/home/user/level07:/bin/bash
level08:x:2008:2008::/home/user/level08:/bin/bash
level09:x:2009:2009::/home/user/level09:/bin/bash
level10:x:2010:2010::/home/user/level10:/bin/bash
level11:x:2011:2011::/home/user/level11:/bin/bash
level12:x:2012:2012::/home/user/level12:/bin/bash
level13:x:2013:2013::/home/user/level13:/bin/bash
level14:x:2014:2014::/home/user/level14:/bin/bash
flag00:x:3000:3000::/home/flag/flag00:/bin/bash
flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash
flag02:x:3002:3002::/home/flag/flag02:/bin/bash
flag03:x:3003:3003::/home/flag/flag03:/bin/bash
flag04:x:3004:3004::/home/flag/flag04:/bin/bash
flag05:x:3005:3005::/home/flag/flag05:/bin/bash
flag06:x:3006:3006::/home/flag/flag06:/bin/bash
flag07:x:3007:3007::/home/flag/flag07:/bin/bash
flag08:x:3008:3008::/home/flag/flag08:/bin/bash
flag09:x:3009:3009::/home/flag/flag09:/bin/bash
flag10:x:3010:3010::/home/flag/flag10:/bin/bash
flag11:x:3011:3011::/home/flag/flag11:/bin/bash
flag12:x:3012:3012::/home/flag/flag12:/bin/bash
flag13:x:3013:3013::/home/flag/flag13:/bin/bash
flag14:x:3014:3014::/home/flag/flag14:/bin/bash
```

`flag01:42hDRfypTqqnw:3001:3001::/home/flag/flag01:/bin/bash`
=> Found the clue of flag01,
maybe decode the "42hDRfypTqqnw"?

5. Try to put this password ? This was wrong password

The password is hashed, so need to find the algorithm to find the way to crack the hashed password
=> Need to use "John the Ripper" to crack the password, but John is not installed on the file

https://www.openwall.com/john/

John the Ripper : is the open source password cracking tool available for many operating systems.

Need to know the notion of "hash"

One way function =======>
Input -> Hash Function -> Output
<======= Impossible to reverse the hash

- Before executing the "John the Ripper", first, we need to copy our password safely.

scp :Secure Copy
-P 4242 :connection for ssh, we use port 4242
level01@127.0.0.1: : user's name and host address
/etc/passwd : path of file that we will copy
. : the place where we save our copy, "Current directory"

`scp -P 4242 level01@127.0.0.1:/etc/passwd .`

=> Install the "John the Ripper" and the crack the password.

`flag01@SnowCrash:~$ su flag01
Password:
Don't forget to launch getflag !
flag01@SnowCrash:~$ getflag
Check flag.Here is your token : f2av5il02puano7naaf6adaaf
flag01@SnowCrash:~$`
