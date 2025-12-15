## Alias (псевдонимы)

### Как помогают системным администраторам?

Ваши алиасы — отличный пример! Давайте разберем, почему они так полезны.

**Экономия времени:**

```bash
# Вместо 17 символов
sudo dnf -y install package

# Всего 8 символов
install package
```

За день таких команд могут быть десятки — экономия огромная.

**Стандартизация опций:**

```bash
# Всегда использовать подтверждение
alias install="sudo dnf -y install"

# Всегда обновлять с подтверждением
alias update="sudo dnf update"
```

---
### Самые популярные алиасы у админов:

**Безопасность:**

```bash
# Защита от случайного удаления
alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'

# Подтверждение при перезаписи
alias ln='ln -i'
```

**Удобная навигация:**

```bash
# Быстрая навигация
alias ..='cd ..'
alias ...='cd ../..'
alias ....='cd ../../..'

# Переход в часто используемые директории
alias logs='cd /var/log'
alias www='cd /var/www/html'
alias conf='cd /etc/nginx/conf.d'
```

**Человеко-читаемый вывод:**

```bash
# Размеры файлов в человеко-читаемом формате
alias df='df -h'
alias du='du -h'
alias free='free -h'

# Цветной вывод
alias ls='ls --color=auto'
alias grep='grep --color=auto'
alias egrep='egrep --color=auto'
alias fgrep='fgrep --color=auto'
```

**Мониторинг и отладка:**

```bash
# Короткие команды для мониторинга
alias psg='ps aux | grep'
alias ports='netstat -tulanp'
alias meminfo='free -m -l -t'
alias cpuinfo='lscpu'

# Просмотр логов
alias tailauth='tail -f /var/log/auth.log'
alias tailsys='tail -f /var/log/syslog'
```

**Hyprland**:

```bash
# из любой директории откроет конфиг
alias hypr='nano ~/.config/hypr/hyprland.conf'
```

**Сетевые команды:**

```bash
# Быстрый ping
alias ping='ping -c 5'
alias fastping='ping -c 100 -s.2'

# Безопасный wget
alias wget='wget -c'
```

---
### Где хранятся алиасы?

- **Локальные:** `~/.bashrc` или `~/.bash_aliases`

- **Глобальные:** `/etc/bashrc` или `/etc/profile.d/`

**Чтобы алиас работал постоянно, добавьте его в ~/.bashrc:**

```bash
echo 'alias install="sudo dnf -y install"' >> ~/.bashrc
source ~/.bashrc
```

**Alias** — ваша персонализация рабочего окружения. Создавайте алиасы для всего, что вводите часто. Это инвестиция в вашу эффективность.