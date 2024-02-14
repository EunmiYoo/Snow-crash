## Level10

Login as usual wih a password

```
ssh -i ~/.ssh/id_rsa.pub level10@127.0.0.1 -p 4242
s5cAJpM8ev6XHw998pRWG728z
```

We found the token and level0 files as below

```
level10@SnowCrash:~$ ls -la
total 28
dr-xr-x---+ 1 level10 level10 140 Mar 6 2016 .
d--x--x--x 1 root users 340 Aug 30 2015 ..
-r-x------ 1 level10 level10 220 Apr 3 2012 .bash_logout
-r-x------ 1 level10 level10 3518 Aug 30 2015 .bashrc
-rwsr-sr-x+ 1 flag10 level10 10817 Mar 5 2016 level10
-r-x------ 1 level10 level10 675 Apr 3 2012 .profile
-rw------- 1 flag10 flag10 26 Mar 5 2016 token

Seems like level10 file is related to the host.
```

level10 file is ELF binary file.

```
level10@SnowCrash:~$ file ./level10
./level10: setuid setgid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0xf7e21fb68568fa57d6317d0535b97d9fca66f841, not stripped

level10@SnowCrash:~$ ltrace ./level10
__libc_start_main(0x80486d4, 1, 0xbffff7b4, 0x8048970, 0x80489e0 <unfinished ...>
printf("%s file host\n\tsends file to ho"..., "./level10"./level10 file host
	sends file to host if you have access to it
)             = 65
exit(1 <unfinished ...>
+++ exited (status 1) +++
```

Seems like we need to send file to host

```
level10@SnowCrash:~$ ./level10 token
./level10 file host
	sends file to host if you have access to it

level10@SnowCrash:~$ ./level10 file
./level10 file host
sends file to host if you have access to it
```

Did some tests and it tries to connect to 6969.
Maybe, we need to open the other terminal and try to connect with the `nc -l 0.0.0.0 6969`?

```
level10@SnowCrash:~$ ./level10 ./level10 hello
Connecting to hello:6969 .. Unable to connect to host hello
```
