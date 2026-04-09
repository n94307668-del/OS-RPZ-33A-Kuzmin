# Work-case 6: Керування користувачами та налаштування оболонок


## Завдання 1: Встановлення командних інтерпретаторів

### 1.1 Обрані оболонки

На додаток до **bash** (за замовчуванням), були встановлені наступні оболонки:

| Оболонка | Команда встановлення | Опис |
|----------|---------------------|------|
| **Zsh** | `sudo apt install zsh` | Потужна оболонка з розширеним автодоповненням, підтримкою плагінів та кастомізацією через Oh-My-Zsh |
| **Fish** | `sudo apt install fish` | Дружня оболонка з підсвічуванням синтаксису, автопропозиціями та веб-налаштуванням |

![Zsh](D:\Downloads\Коледж\Install-ZSH-in-Ubuntu.png)
### 1.2 Можливості кожної оболонки

#### Zsh (Z Shell)
- **Розширене таб-доповнення** з розумінням контексту
- **Глоббінг** - потужний пошук файлів за шаблонами
- **Екосистема плагінів** через фреймворк Oh-My-Zsh
- **Підтримка тем** з кастомізованими prompt
- **Виправлення помилок** в командах
- **Спільна історія команд** між сесіями
- **Сумісність** з bash-скриптами

#### Fish (Friendly Interactive Shell)
- **Підсвічування синтаксису** - команди забарвлюються під час набору
- **Автопропозиції** - пропонує команди на основі історії
- **Веб-налаштування** - `fish_config` відкриває налаштування в браузері
- **Дружній за замовчуванням** - не потребує додаткової конфігурації
- **Простий скриптинг** - легший синтаксис для початківців
- **Автоматичні доповнення** з man-сторінок

### 1.3 Команди встановлення

```bash
# Оновлення списку пакетів
sudo apt update

# Встановлення Zsh
sudo apt install zsh -y

# Встановлення Fish
sudo apt install fish -y

# Перевірка встановлених оболонок
cat /etc/shells
```

---

## Завдання 2: Створення користувачів та груп

### 2.1 Структура груп

| Група | Призначення | Користувачі |
|-------|-------------|-------------|
| `techsupport` | Технічна підтримка, системні адміністратори | tech1, tech2 |
| `developers` | Розробники, технічні спеціалісти | dev1, dev2 |
| `financiers` | Бухгалтерія, економісти | fin1, fin2 |
| `founders` | Керівництво | founder1, founder2 |
| `guests` | Гості | guest1, guest2 |

### 2.2 Команди створення

```bash
# Створення груп
sudo groupadd techsupport
sudo groupadd developers
sudo groupadd financiers
sudo groupadd founders
sudo groupadd guests

# Створення користувачів з домашніми каталогами
# Технічна підтримка (оболонка bash)
sudo useradd -m -G techsupport -s /bin/bash tech1
sudo useradd -m -G techsupport -s /bin/bash tech2

# Розробники (оболонка zsh)
sudo useradd -m -G developers -s /usr/bin/zsh dev1
sudo useradd -m -G developers -s /usr/bin/zsh dev2

# Фінансисти (без доступу до оболонки)
sudo useradd -m -G financiers -s /usr/sbin/nologin fin1
sudo useradd -m -G financiers -s /usr/sbin/nologin fin2

# Керівництво (оболонка fish)
sudo useradd -m -G founders -s /usr/bin/fish founder1
sudo useradd -m -G founders -s /usr/bin/fish founder2

# Гості (без доступу до оболонки)
sudo useradd -m -G guests -s /usr/sbin/nologin guest1
sudo useradd -m -G guests -s /usr/sbin/nologin guest2

# Встановлення паролів для всіх користувачів
sudo passwd tech1
sudo passwd tech2
sudo passwd dev1
sudo passwd dev2
sudo passwd fin1
sudo passwd fin2
sudo passwd founder1
sudo passwd founder2
sudo passwd guest1
sudo passwd guest2
```

### 2.3 Команди перевірки

```bash
grep -E "tech|dev|fin|founder|guest" /etc/passwd

# Перевірка членства в групах
groups tech1
groups dev1
groups fin1
groups founder1
groups guest1

# Перевірка призначених оболонок
getent passwd tech1 dev1 fin1 founder1 guest1
```

---

## Завдання 3: Конфігурація оболонок

| Група користувачів | Оболонка | Конфігурація |
|-------------------|----------|--------------|
| Technical support | `/bin/bash` | Повний доступ, стандартна оболонка Linux |
| Developers | `/usr/bin/zsh` | Підвищена продуктивність з плагінами |
| Financiers | `/usr/sbin/nologin` | **Доступ до оболонки ЗАБОРОНЕНО** |
| Founders | `/usr/bin/fish` | Дружня інтерактивна оболонка |
| Guests | `/usr/sbin/nologin` | **Доступ до оболонки ЗАБОРОНЕНО** |

### 3.1 Навіщо використовувати `/usr/sbin/nologin`?

Оболонка `/usr/sbin/nologin` ввічливо відмовляє у вході з повідомленням:
```
This account is currently not available.
```

Це рекомендований спосіб відключення доступу до оболонки при збереженні:
- FTP/SFTP доступу (якщо налаштовано)
- Доступу до пошти (якщо налаштовано)
- Власності файлів для сервісів

---

## Завдання 4: Демонстрація роботи груп

### 4.1 Група Technical Support (Bash)

**Користувач:** `tech1`  
**Оболонка:** `/bin/bash`

```bash
su - tech1

echo "=== Системна інформація ==="
uname -a                         
cat /etc/os-release               
hostname                           
uptime                            

echo "=== Поточна конфігурація ==="
pwd                             
whoami                         
id                                
date                             

# Системні 
echo "=== Системні ресурси ==="
free -h                           
df -h                             
ps aux | head -10                 
```

**результат:**
```
Linux ubuntu 5.15.0-91-generic #101-Ubuntu SMP...
PRETTY_NAME="Ubuntu 22.04.3 LTS"
/home/tech1
tech1
uid=1001(tech1) gid=1001(tech1) groups=1001(tech1),1005(techsupport)
```

### 4.2 Група Developers (Zsh)

**Користувач:** `dev1`  
**Оболонка:** `/usr/bin/zsh`

```zsh
su - dev1

echo "=== Zsh оточення ==="
echo $SHELL                       
zsh --version                     

echo "=== Операції з каталогами ==="
ls -la                            
cd /var/log && pwd                
pushd /etc && dirs               
popd

echo "=== Шаблони файлів ==="
ls /etc/*.conf(N)                 
echo /usr/bin/z*              

echo "=== Статус системи ==="
cat /proc/cpuinfo | grep "model name" | head -1
lscpu | grep "CPU(s):"
```


### 4.3 Група Financiers (Без доступу)

**Користувач:** `fin1`  
**Оболонка:** `/usr/sbin/nologin`

```bash
su - fin1
```

**результат:**
```
This account is currently not available.
```

**Альтернативні методи доступу:**
```bash
# Як root, ви можете переглядати їх файли
sudo ls -la /home/fin1
sudo cat /home/fin1/.bashrc 2>/dev/null || echo "No shell config"

id fin1
getent passwd fin1
```

**Вивід:**
```
uid=1005(fin1) gid=1005(fin1) groups=1005(fin1),1003(financiers)
fin1:x:1005:1005::/home/fin1:/usr/sbin/nologin
```

### 4.4 Група Founders (Fish)

**Користувач:** `founder1`  
**Оболонка:** `/usr/bin/fish`

```fish
su - founder1

echo "=== Особливості Fish ==="
echo $SHELL                       
fish --version                   

echo "=== Огляд системи ==="
date                              
uname -v                          

echo "Наберіть 'ls -la' та спостерігайте за кольором"

echo "=== Швидка статистика ==="
command uptime
command free -h
df -h /home                       
```

**Особливості Fish:**
- **Автопропозиції:** Сірий текст пропонує попередні команди
- **Підсвічування синтаксису:** Зелений = валідна команда, червоний = невалідна
- **Спрощений синтаксис:** Не потрібен `$` в командах `set`

### 4.5 Група Guests (Без доступу)

**Користувач:** `guest1`  
**Оболонка:** `/usr/sbin/nologin`

```bash
su - guest1
```

**результат:**
```
This account is currently not available.
```

**Перевірка:**
```bash
id guest1
getent passwd guest1
groups guest1

ls -ld /home/guest1
sudo ls -la /home/guest1
```

**Вивід:**
```
uid=1009(guest1) gid=1009(guest1) groups=1009(guest1),1006(guests)
guest1:x:1009:1009::/home/guest1:/usr/sbin/nologin
drwxr-x--- 2 guest1 guest1 4096 Jan 15 10:30 /home/guest1
```
## Conclusions (Висновки англійською)

This work-case demonstrated the practical implementation of Linux user management and shell configuration:

1. **Shell Installation**: Successfully installed Zsh and Fish shells alongside the default Bash, providing users with alternative command-line interfaces tailored to their needs.

2. **User Creation**: Created 10 users distributed across 5 functional groups (Technical Support, Developers, Financiers, Founders, and Guests), each with appropriate permissions and access levels.

3. **Security Implementation**: Applied the principle of least privilege by assigning `/usr/sbin/nologin` to Financiers and Guests groups, effectively preventing terminal access while maintaining user accounts for other potential services.

4. **Role-Based Shell Assignment**: Configured appropriate shells for each user group:
   - Bash for Technical Support (standard, reliable)
   - Zsh for Developers (advanced features, productivity)
   - Fish for Founders (user-friendly, intuitive)
   - Nologin for restricted groups (security)

5. **Practical Verification**: Demonstrated system operations for each user type, confirming that access controls and shell configurations work as intended.

6. **System Administration Skills**: Gained hands-on experience with essential Linux administration commands including `useradd`, `groupadd`, `usermod`, `passwd`, and user verification tools.

---

## Glossary / Vocabulary (English)

| English Term | Description |
|--------------|-------------|
| **Shell** | A command interpreter that translates user commands into instructions for the computer |
| **Command interpreter** | Software that reads and executes commands from the user |
| **User** | An account that allows a person to access and use the system |
| **Group** | A collection of users that share common permissions and access rights |
| **Home directory** | A personal folder assigned to each user for storing their files |
| **Permission** | Access rights that determine what actions a user can perform on files and directories |
| **Technical support** | IT specialists responsible for maintaining and troubleshooting systems |
| **Developers** | Programmers and technical specialists who write and maintain software |
| **Financiers** | Accounting and economics professionals who manage financial operations |
| **Founders** | Management and leadership personnel |
| **Guests** | Temporary users with limited access to the system |
| **Shell access** | The ability to open a terminal and execute commands |
| **Default shell** | The command interpreter automatically assigned to a user upon login |
| **Nologin** | A special shell that denies interactive login access |
| **System administrator** | A person responsible for managing and maintaining computer systems |
| **Bash** | Bourne Again Shell - the default Linux command interpreter |
| **Zsh** | Z Shell - an advanced shell with plugin support |
| **Fish** | Friendly Interactive Shell - a user-friendly shell with modern features |
| **Useradd** | Command to create a new user account |
| **Groupadd** | Command to create a new group |
| **Passwd** | Command to set or change a user's password |
| **/etc/passwd** | System file containing user account information |
| **/etc/shells** | File listing available login shells on the system |
| **/etc/group** | System file containing group information |
| **Sudo** | Command to execute actions with superuser privileges |
| **Su** | Command to switch to another user account |

---