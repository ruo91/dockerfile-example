Dockerfile Example
==========

#### Ex1) SSH

- sdsdsdsd
```
root@ruo91:~# git clone https://github.com/ruo91/dockerfile-example.git
root@ruo91:~# docker build --rm -t ssh dockerfile-example/ssh/
root@ruo91:~# docker run -d -p 2222:22 -h "ssh" --name "ssh" `docker images | grep ssh | awk '{print $3 }'`
```
- 
root@ruo91:~# ssh -p 2222 root@localhost
The authenticity of host '[localhost]:2222 ([127.0.0.1]:2222)' can't be established.
ECDSA key fingerprint is e7:75:99:8d:9b:69:aa:ee:bb:32:c3:b3:27:a4:c4:81.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '[localhost]:2222' (ECDSA) to the list of known hosts.
root@localhost's password:

root@ssh:~# cat /etc/issue
Ubuntu 14.04 LTS \n \l

root@ssh:~# netstat -ant
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN
tcp        0      0 192.168.0.2:22          192.168.0.1:45540       ESTABLISHED
tcp6       0      0 :::22                   :::*                    LISTEN
root@ssh:~# ps -aux
USER       PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root         1  0.2  0.6  52120 13808 ?        Ss   03:50   0:00 /usr/bin/python /usr/bin/supervisord
root         9  0.0  0.1  61364  3004 ?        S    03:50   0:00 /usr/sbin/sshd -D
root        10  0.0  0.1  65996  3484 ?        Ss   03:50   0:00 sshd: root@pts/0
root        12  0.0  0.0  18184  2020 pts/0    Ss   03:50   0:00 -bash
root        25  0.0  0.0  15568  1104 pts/0    R+   03:51   0:00 ps -aux
