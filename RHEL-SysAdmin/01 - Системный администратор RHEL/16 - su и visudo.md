Линукс, система многопользовательская. Если с поомщью команды `wc` подсчитать кол-во строк в файле **etc/passwd** где перечислены все пользователи.
cat /
```bash
wc -l /etc/passwd

40 /etc/passwd
```

Мы увидим, что в системе 40 пользователей

```bash
cat /etc/passwd

root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
news:x:9:9:news:/var/spool/news:/usr/sbin/nologin
uucp:x:10:10:uucp:/var/spool/uucp:/usr/sbin/nologin
proxy:x:13:13:proxy:/bin:/usr/sbin/nologin
www-data:x:33:33:www-data:/var/www:/usr/sbin/nologin
backup:x:34:34:backup:/var/backups:/usr/sbin/nologin
list:x:38:38:Mailing List Manager:/var/list:/usr/sbin/nologin
irc:x:39:39:ircd:/run/ircd:/usr/sbin/nologin
_apt:x:42:65534::/nonexistent:/usr/sbin/nologin
nobody:x:65534:65534:nobody:/nonexistent:/usr/sbin/nologin
systemd-network:x:998:998:systemd Network Management:/:/usr/sbin/nologin
dhcpcd:x:100:65534:DHCP Client Daemon:/usr/lib/dhcpcd:/bin/false
tss:x:101:103:TPM software stack:/var/lib/tpm:/bin/false
systemd-timesync:x:991:991:systemd Time Synchronization:/:/usr/sbin/nologin
messagebus:x:990:990:System Message Bus:/nonexistent:/usr/sbin/nologin
dnsmasq:x:999:65534:dnsmasq:/var/lib/misc:/usr/sbin/nologin
avahi:x:102:107:Avahi mDNS daemon:/run/avahi-daemon:/usr/sbin/nologin
speech-dispatcher:x:103:29:Speech Dispatcher:/run/speech-dispatcher:/bin/false
usbmux:x:104:46:usbmux daemon:/var/lib/usbmux:/usr/sbin/nologin
cups-pk-helper:x:105:108:user for cups-pk-helper service:/nonexistent:/usr/sbin/nologin
fwupd-refresh:x:989:989:Firmware update daemon:/var/lib/fwupd:/usr/sbin/nologin
geoclue:x:106:110::/var/lib/geoclue:/usr/sbin/nologin
gnome-remote-desktop:x:988:988:GNOME Remote Desktop:/var/lib/gnome-remote-desktop:/usr/sbin/nologin
saned:x:107:111::/var/lib/saned:/usr/sbin/nologin
polkitd:x:987:987:User for polkitd:/:/usr/sbin/nologin
rtkit:x:108:112:RealtimeKit:/proc:/usr/sbin/nologin
colord:x:109:113:colord colour management daemon:/var/lib/colord:/usr/sbin/nologin
Debian-gdm:x:110:114:Gnome Display Manager:/var/lib/gdm3:/bin/false
xbpsx:x:1000:1000:xbpsx,,,:/home/xbpsx:/bin/bash
nvidia-persistenced:x:111:115:NVIDIA Persistence Daemon:/var/run/nvidia-persistenced/:/usr/sbin/nologin
_flatpak:x:112:116:Flatpak system-wide installation helper:/nonexistent:/usr/sbin/nologin
libvirt-qemu:x:64055:993:Libvirt Qemu:/var/lib/libvirt:/usr/sbin/nologin
```

Среди них есть наш пользователь **user(xbpsx:)** , супервользователь **root**, а остальные создавали не мы, а система, считаются [[сервисными пользователями]]. 

Для практики, **нам понадобится, еще один пользователь**, поэтому мы создадим его

```bash
sudo useradd user2
```

Зададим его пароль

```bash
sudo passwd user2
```

---
## su

Давайте почитаем манул по команде `su` с помощью `masn su` . Она позволяет менять текущего пользователя или запускать команды от имени другого пользователя. Это бывает нужно, когда у нашего пользователя нет нужных прав, либо когда нам нужно запустить какойто процесс, от имени другого пользователя, в целях безопасности. 

Допустим, наш пользвоатель не может войти в диркторию `/home/user2`

```bash
cd /home/user2
bash: cd : /home/user2: Permission denied
```

У нас нет прав. Мы можем написать `su user2` и ввести пароль

```bash
su user2
Password:
```

И мы станем пользователем `user2` и мы можем зайти в нужную директорию. Что бы вернуться к моему пользователю, надо ввести либо `exit` либо нажать `Ctl+D` . 

Если просто написать `su` или `su root` и ввести пароль суперпользователя, можно работать от root пользователя. 

Мы разбирали файлы, `.bash_profile` и `.bashrc`. 

- .**bash_profile** - это файл настроек для [[loginshell]]
- **.bashrc** - для [[nonloginshell]] 

Для теста перейдем к пользователю `su user2` и в его `.bash_profile` создадим тестовую переменную

```bash
#.bash_profile
test1=test1
```

а в файле `.bashrc` 

```bash
.bashrc
test2=test2
```

А теперь перезайдем, обратно

```bash
exit
su user2

echo $test1 #Пусто
echo $test2 
test2
```

Как мы видим, сработала настройка только из `.bashrc` , то есть `nonlogionshell`. Это озночает, что переменная окружение у нас остались от предыдущего пользователя, допустим, если посмотреть переменную **$PATH**

```bash
echo $PATH
```

![[Screenshot From 2025-12-15 16-48-56.png]]

Можно увидеть **пути к директориям** 

Зачастую нужно, что бы при логине за другого пользователя **менялось окружение**. То есть, что бы применились настройки из `.bash_profile` нужного  пользователя. 

Для этого после `su -`

```bash
su - user2
```

Теперь у нас есть **обе переменные** 

```bash
echo $test1
test1
echo $test2
test2
```

То есть считался файл у `.bashrc` и `.bash_profile`, а значит это был `loginshell`

Так же стоит заметить при `su -` поменялась директория

```bash
pwd
/home/user2
```

 Раньше мы находились в директории пользователя `user1` , а после перехода текующая директория **home/user2**

Для поверки `loginshell` и `nonloginshell` , достаточно проверить значенеи переменной `$0` 

```bash
echo $0

#bash - nonloginshell
#-bash - loginshell
```

При проверке `$PATH` снова в `loginshell`

![[Screenshot From 2025-12-15 17-01-42.png]]

Теперь здесь нет путей **/usr/home/bin**, то есть **переменные окружения не передались от другого пользователя.**

- **login shell** - это оболочка с авторизацией, то есть сначала они считывает настройки, те же переменные, aлиасы, функции и прочее, сначала с файла `/etc/profile`, где написано, стоит смотреть так же в директории `/etc/profile.d/*` и файл `./bash_profile`, а через него уже `./.bashrc` , тот же указывает на `/etc/bashrc`

```text
# login shell
/etc/profile > /etc/profile.d/*
			> ~/.bash_profile > ~/.bashrc > /etc/bashrc
```

- **non login shell** - сначала читается файл `/.bashrc`, в домашней директории пользователя, далее `/etc/bashrc` который ссылается на `/etc/profile.d/*`(может отличается в другом дистрибутиве)

```text
# non login shell
~/.bashrc > /etc/bashrc > /etc/profile.d/*
```

Мы говорили, что `su` может запускать команды от имени другого пользователя, для этого используем ключ `-c`

```bash
su user2 -c "touch file"
# : Permission denied
```

```bash
su - user2 -c "touch file"
Password:
```

Часто, `su` используют, что бы работать от имени `root` пользователя. 

> Если `su` запускать от имени `root` то **никаких паролей не потребуется**

У `su` есть **один нюас**, нужо знать _пароль других пользователей_. Когда мы один админ в системе, это не проблема. Давайте представим, что есть несколько администраторов, давать **друг другу пароли - плохая идея**, потому-что безопасность строится на **полном недоверие друг к другу**. 

> Другой администратор может в теории умышленно нас подставить.


---
## sudo

Когда работают несколько пользователей, например мы хотим одному из пользователей дать возможность, только создавать пользователей. Вроде ему и нужны права `root` , но даватьему эту возможность не хочется(показывать пароль). Для решени таких проблем, есть утилита `sudo`

С помощью `sudo` мы устанавливали программы и устанавливали новый пользователей.

```bash
# Команда sudo useradd 
# Запускается от суперпользователя
sudo useradd user2
```

**Не каждый пользователей** может написать `sudo` и выполнить команду. У нашего **user1** это работает, так как при установки системы, мы поставили галочку напротив сджелать его администратором. **Но как это работает?**

У `sudo` есть **файл настроек** 

```bash
nano /etc/sudoers
```

Вначале нас встречает строчка 

```bash
#This file MUST be edited with the 'visudo' command as root.
```

Все дело в том, что при запуске `sudo` программа каждый раз считывает этот файл и если тут будет "синтаксическая ошибка" то `sudo` **перестанет работать**. 

```bash
# Запускает дефолтный редактор vim
sudo visudo
```

Если мы хотим, чтоб все работало с `nano` нам нужно, кое что поменять. Как правило, программы по умолчанию задаются при помощи **переменных окружений**.  

Зайдем в `/.bashrc` и напишем

```bash
# .bashrc
export EDITOR=nano
```

Далее мы входим в и находим стьрочку с **env keep**

```bash
sudo nano /etc/sudoers
```

![[Screenshot From 2025-12-16 13-11-03.png]]

Выше мы видим **env_reset** которая сбрасывает все переменные, чтобы сделать `sudo` минимальным.

Так вот, во вторую строчку добавляем **EDITOR**

```bash
Default   env_keep =  "EDITOR COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS"
```

Что бы `sudoers` не игнорировал нашу переменную. Теперь при запуске 

```bash
# откроется nano
sudo visudo
```

Еще один момент, сверху указано не **etc/sudoers**, а **etc/sudoers.tmp**

![[Screenshot From 2025-12-16 13-22-54.png]]

Это озночает, когда мы запускаем `visudo` у нас **sudoers** копируется во **временный файл** , если в этом файле сделать ошибку. То `visudo` сначада проверит, все ли нормально с файлом, заметит ошибку и спросит, что делать. 

> `visudo` защищает нас от синтактических ошибок










