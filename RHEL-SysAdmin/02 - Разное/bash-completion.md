**Bash-completion** — это система автоматического дополнения команд в bash. Когда вы начинаете вводить команду, путь к файлу или опцию, и нажимаете **Tab**, bash-completion предлагает возможные варианты завершения.

### Что дает на практике?

**Без bash-completion:**

```bash
systemctl status apache2.service
```

Вы должны ввести всё полностью, возможны опечатки.

**С bash-completion:**

```bash
sys<Tab> → systemctl
systemctl sta<Tab> → systemctl status
systemctl status apa<Tab> → systemctl status apache2.service
```

### Где и как используют системные администраторы?

**Работа с пакетами:**

```bash
# Ввод: sudo dnf inst<Tab>
sudo dnf install

# Ввод: sudo apt-get pur<Tab>
sudo apt-get purge
```

**Навигация по длинным путям:**

```bash
# Ввод: cd /etc/nginx/conf.d/avai<Tab>
cd /etc/nginx/conf.d/available-sites/
```

### Что точно нужно знать:

- **Установка:** `sudo dnf install bash-completion` (для Fedora/RHEL)

- **Перезагрузка:** после установки перезапустите терминал

- **Двойное нажатие Tab:** показывает все возможные варианты

- **Частичное дополнение:** работает даже если вы ввели только часть слова

**Bash-completion** — ваш главный помощник в скорости работы. Экономит время и уменьшает количество опечаток.