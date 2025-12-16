## **Практические примеры для администратора**

### **Пример 1: Почему не сработала ваша переменная**

```bash
# Вы сделали:
su user2                    # Non-login shell
# Выполнился ТОЛЬКО ~/.bashrc
# ~/.bash_profile НЕ выполнился!

# Чтобы переменная из .bash_profile применилась:
su - user2                  # Login shell (с дефисом!)
# Теперь выполнились: /etc/profile → ~/.bash_profile
```

### **Пример 2: Проверка типа шелла**

```bash
# Узнать, какой у нас сейчас шелл:
echo $0
# Если начинается с "-" → login shell
# Пример: -bash (login), bash (non-login)

# Или проверить переменную:
shopt -q login_shell && echo "Login shell" || echo "Non-login shell"
```

### **Пример 3: Разное поведение переменных**

```bash
# В .bash_profile:
export GLOBAL_VAR="видно везде"

# В .bashrc:
LOCAL_VAR="видно только здесь"

# При входе:
su - user              # Обе переменные доступны
echo $GLOBAL_VAR       # работает
echo $LOCAL_VAR        # работает (если .bashrc вызван из .bash_profile)

su user                # Только если .bashrc выполнился
echo $GLOBAL_VAR       # НЕ РАБОТАЕТ (не экспортировалась)
echo $LOCAL_VAR        # работает
```

---
### **Проблема 1: Переменные PATH не работают в скриптах**

```bash
# В .bashrc добавили:
PATH="$PATH:/opt/myapp/bin"

# Но скрипты, запущенные из cron, не видят путь
# Решение: Добавить в .profile (login shell файл) или в /etc/environment
```

### **Проблема 2: Разный prompt в ssh и локально**

```bash
# SSH запускает login shell, локальный терминал — non-login
# Решение: Настройку PS1 поместить в .bashrc, а в .bash_profile вызвать .bashrc
```

### **Проблема 3: Запуск служб из systemd**

```bash
# Systemd НЕ запускает шелл! Поэтому ни .bashrc, ни .bash_profile не выполняются
# Для переменных окружения служб используйте:
# 1. Environment= в unit-файле
# 2. Файлы в /etc/environment.d/
# 3. EnvironmentFile= в unit-файле
```

---

## **Правильная настройка (лучшие практики)**

### **Рекомендуемая структура файлов:**

**~/.bash_profile:**

```bash
# Выполняется только при логине
# 1. Экспорт переменных
export EDITOR=vim
export HISTSIZE=10000

# 2. Вызов .bashrc (чтобы настройки были и в login shell)
if [ -f ~/.bashrc ]; then
    . ~/.bashrc
fi

# 3. Логирование входа (опционально)
logger "$USER logged in from $SSH_CONNECTION"
```

**~/.bashrc:**

```bash
# Выполняется при каждом новом non-login shell
# 1. Алиасы
alias ll='ls -la --color=auto'
alias grep='grep --color=auto'

# 2. Prompt
PS1='\[\033[01;32m\]\u@\h\[\033[00m\]:\[\033[01;34m\]\w\[\033[00m\]\$ '

# 3. Функции
function mkcd() { mkdir -p "$1" && cd "$1"; }

# 4. Безопасность: не сохранять дубликаты в истории
HISTCONTROL=ignoreboth

# 5. Опции шелла
shopt -s autocd   # Автоматический cd при вводе пути
```

---
## **Важные команды для админа**

### **Принудительный login shell:**

```bash
# Для выполнения всех инициализационных скриптов
sudo -i                    # Login shell для root
su - user                  # Правильный переход к пользователю
ssh user@server            # По умолчанию login shell
bash --login               # Явный запрос login shell
```

### **Только команда без шелла:**

```bash
# Иногда нужно выполнить команду от другого пользователя Без входа в шелл
sudo -u user command       # Лучше чем su
runuser -u user command    # Альтернатива
```

### **Отладка загрузки:**

```bash
# Просмотреть, какие файлы выполняются
bash -x -l                 # Login shell с отладкой
bash -x                    # Non-login shell с отладкой

# Посмотреть текущие переменные
env                        # Все экспортированные переменные
set                        # Все переменные (локальные + экспортированные)
```



