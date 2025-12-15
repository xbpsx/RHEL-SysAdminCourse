
### bash-completion

Есть два типа дополнения, **простое и продвинутое**. В случае простого дополнения, у нас просто дописывается сама команда или файл. Есть еще **продвинутое дополнение** которое может дополнять **опции и значения**. 

Например, команда для **архивации** `tar` , если нажать tab то сперва появится дефис, а после двух нажатий tab появятся все опции, у `tar` ключи обязательны.

```bash
tar -
-A  -c  -d  -r  -t  -u  -x  
```

Когда стоит минимальная система, продвинутого дополнения нету, нужен пакет `bash-completion`

[[bash-completion]]

---
### alias

Еще в **bash** можно настроить `alias`, они дают возможность настроить свои команды на основе других, если написать просто `alias` в терминале мы увидим список заданых алисов для текующей **bash сессии**.

```bash
alias  
alias egrep='grep -E --color=auto'  
alias fgrep='grep -F --color=auto'  
alias grep='grep --color=auto'  
alias l.='ls -d .* --color=auto'  
alias ll='ls -l --color=auto'  
alias ls='ls --color=auto'  
alias which='(alias; declare -f) | /usr/bin/which --tty-only --read-alias --read-functions --sh  
ow-tilde --show-dot'  
alias xzegrep='xzegrep --color=auto'  
alias xzfgrep='xzfgrep --color=auto'  
alias xzgrep='xzgrep --color=auto'  
alias zegrep='zegrep --color=auto'  
alias zfgrep='zfgrep --color=auto'  
alias zgrep='zgrep --color=auto'  
```

Когда мы запускаем эмулятор терминала, у нас запускается **bash сессия**. Для каждой сессии баш запускается с нуля. То есть, он считывает файл настроек. Это значит, что если мы изменим файл настроек, то в запущеном баше ничего не измениться. Если написать bash или запустить новое окно, то нвоая сессия эти изминения увидит.

В выводе alias мы видим знакомые команды, например `alias l.='ls -d .* --color=auto'` 

```bash
# Покажет все скрытые файлы или директории alias 
l.  
.bash_history  .bashrc  .fonts      .local    .nvidia-settings-rc  .var  
.bash_logout   .cache   .gitconfig  .mozilla  .pki                 .vscode  
.bash_profile  .config  .icons      .nv       .ssh 
```

Давай зададим новые `alias`, например

```bash
alias vi=nano
```

У нас появился новый alias и при открывания файла с помощью vi, будет запускаться nano

```bash
alias
alias egrep='grep -E --color=auto'
alias fgrep='grep -F --color=auto'
alias grep='grep --color=auto'
alias l.='ls -d .* --color=auto'
alias ll='ls -l --color=auto'
alias ls='ls --color=auto'
alias vi='nano'
```

```bash
# запустит nano
vi .bashrc
```

> `alias` сохроняются в текущей сессии, и если перезагрузить терминал, то они пропадут

Если мы хотим, чтоб наши alias сохранялись навсегда, то мы должны написать их в `.bashrc`

![[Screenshot From 2025-11-25 00-12-03.png]]

`alias` нужны пользователям для **упрощения работы**, бывают сложные команды, иногда бывают длинные команды которые мы часто должны вбивать, что бы облегчить себе жизнь, мы создадим себе алиасы. **Бывают случае когда нам нужен не alias, а сама команда**. Для этого мы доабвляем `\ls,` чтобы запустить обычный `ls`, вместо  `ls='ls --color=auto'` , либо взять команду `"ls"`, либо написать `command ls` 

В файле `.bashrc` **нету alias которые мы видели** в выводе в эмуляторе, потому-что .`bashrc` распространяется на моего пользвоателя, а есть глобальный файл `/etc/bashrc`. Если мы хотим создать alias который бы работал у всех пользователей в системе, создаем его глбально. Но тут тоже нет, тех же альянсов, что и в выводе.

[[alias]]

---
### type

**Bash** может запускать как команды так и алисы. Сами команды можно разделить на два типа, **внешние и внутрение**. Команды которые встроенные в bash, они называются внутрение, например тот же `cd`. Но большинство команд лежат на файловой системе в **специальных директориях и называются внешними**. Что бы отличить внутреннюю или внешнюю команду, есть `type`

```bash
# ls это alias
type ls  
ls is aliased to `ls --color=auto`

# Показывает путь к программе, внешняя программа
type mkdir  
mkdir is /usr/bin/mkdir

# Встроенная программа
type cd  
cd is a shell builtin
```

`ls` это `alias` сам на себя, как понять, что стоит за этим алиасом

```bash
# Для этого опция -a
type -a ls
ls is aliased to `ls --color=auto`
ls is /usr/bin/ls
```
_Теперь мы видим, что ls это внешняя программа_

[[type]]
