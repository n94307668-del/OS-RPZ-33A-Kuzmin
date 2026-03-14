# Звіт до лабораторної роботи №7

**Тема:** Створення скриптових сценаріїв та визначення апаратної конфігурації системи.

**Мета роботи:**
1. Отримання практичних навиків роботи з командною оболонкою Bash.
2. Знайомство з базовими діями при роботі зі скриптовими сценаріями.

**Виконав:** Студент групи РПЗ-33А Кузьмін Нікіта

## 1. Словник англійських термінів

| Термін (English) | Переклад (Українська) | Визначення (English) |
|---|---|---|
| **Shell Script** | Скриптовий сценарій оболонки | A computer program designed to be run by the Unix shell, a command-line interpreter. The scripts are typically used for automating tasks. |
| **Bash (Bourne-Again SHell)** | Bash (Оболонка Борна-знову) | A Unix shell and command language, and is the default login shell for many Linux distributions. |
| **Shebang (`#!`)** | Шебанг | The character sequence consisting of the characters number sign and exclamation mark (`#!`) at the beginning of a script file. It specifies the interpreter for the script. |
| **`chmod` (Change Mode)** | `chmod` (Змінити режим) | A command-line utility used to change the file permissions (read, write, execute) of files and directories. |
| **`echo`** | `echo` | A command-line utility that outputs the strings it is given as arguments to standard output. |
| **`date`** | `date` | A command-line utility that displays or sets the system date and time. |
| **`cal` (Calendar)** | `cal` (Календар) | A command-line utility that displays a calendar for a given month or year. |
| **`lscpu` (List CPU)** | `lscpu` (Список CPU) | A command-line utility that displays information about the CPU architecture. |
| **`free`** | `free` | A command-line utility that displays the total amount of free and used physical and swap memory in the system, as well as the buffers and caches used by the kernel. |
| **`lspci` (List PCI)** | `lspci` (Список PCI) | A command-line utility that displays information about all PCI buses and devices in the system. |
| **`lsusb` (List USB)** | `lsusb` (Список USB) | A command-line utility that displays information about USB buses and the devices connected to them. |
| **`lsmod` (List Modules)** | `lsmod` (Список модулів) | A command-line utility that displays the status of currently loaded kernel modules. |
| **`fdisk` (Fixed Disk)** | `fdisk` (Фіксований диск) | A command-line utility for disk partitioning. |
| **`uname` (Unix Name)** | `uname` (Ім'я Unix) | A command-line utility that prints system information, such as the kernel name, network node hostname, kernel release, kernel version, machine hardware name, processor type, hardware platform, and operating system. |
| **`df` (Disk Free)** | `df` (Диск вільний) | A command-line utility that displays the amount of available disk space for file systems. |
| **`grep` (Global Regular Expression Print)** | `grep` (Глобальний друк регулярних виразів) | A powerful command-line utility used to search for patterns (regular expressions) within text files and display lines that match the pattern. |
| **Variable** | Змінна | A named storage location that holds a value, used to store temporary information within a script. |
| **Conditional Statement** | Умовний оператор | A programming construct that executes different blocks of code based on whether a specified condition is true or false (e.g., `if`, `else`, `elif`). |
| **Loop** | Цикл | A programming construct that repeatedly executes a block of code as long as a certain condition is met or for a specified number of iterations (e.g., `for`, `while`). |
| **Motherboard** | Материнська плата | The main printed circuit board (PCB) in general-purpose computers and other expandable systems. It holds and allows communication between many of the crucial electronic components of a system. |
| **MBR (Master Boot Record)** | MBR (Головний завантажувальний запис) | A special type of boot sector at the very beginning of partitioned computer mass storage devices. It holds the boot loader and the disk's partition table. |
| **GPT (GUID Partition Table)** | GPT (Таблиця розділів GUID) | A standard for the layout of the partition table on a physical computer storage device, using globally unique identifiers (GUIDs). It is a newer standard than MBR. |
| **Mounting** | Монтування | The process by which the operating system makes files and directories on a storage device (like a hard drive, USB drive, or network share) available for users to access through the computer's file system. |

## 2. Завдання попередньої підготовки

### 2.1. Охарактеризуйте поняття скриптового сценарію у командній оболонці.

**Скриптовий сценарій (shell script)** у командній оболонці - це текстовий файл, що містить послідовність команд, які можуть бути виконані командним інтерпретатором (оболонкою), таким як Bash. Ці сценарії дозволяють автоматизувати рутинні завдання, виконувати складні операції, які вимагають декількох команд, і створювати власні утиліти. Скрипти можуть включати логіку (умовні оператори, цикли), змінні та функції, що робить їх потужним інструментом для системного адміністрування та розробки.

### 2.2. Яким чином створюються та редагуються скрипти, що треба зробити щоб запустити скрипт?

**Створення та редагування скриптів:**
Скрипти створюються за допомогою будь-якого текстового редактора. У Linux популярними є `nano`, `vi`/`vim`, `gedit` (графічний). Процес виглядає так:
1.  **Відкриття редактора:** Наприклад, `nano myscript.sh`.
2.  **Написання коду:** Першим рядком скрипта зазвичай є **шебанг** (`#!`), який вказує системі, який інтерпретатор використовувати для виконання скрипта (наприклад, `#!/bin/bash` для Bash-скриптів). Далі йдуть команди та логіка.
3.  **Збереження файлу:** Зберегти файл з розширенням `.sh` (хоча це не обов'язково, але є хорошою практикою).

**Запуск скрипта:**
Щоб запустити скрипт, необхідно виконати два кроки:
1.  **Надати права на виконання:** За замовчуванням, новостворені файли не мають прав на виконання. Їх потрібно надати за допомогою команди `chmod`:
    ```bash
    chmod +x myscript.sh
    ```
    Ця команда додає права на виконання (`x`) для всіх користувачів (`+x`).
2.  **Виконати скрипт:**
    *   **Прямий запуск:** Якщо скрипт знаходиться в поточному каталозі, його можна запустити, вказавши шлях до нього:
        ```bash
        ./myscript.sh
        ```
        Якщо скрипт знаходиться в каталозі, який є частиною змінної середовища `$PATH`, його можна запустити просто за іменем: `myscript.sh`.
    *   **Запуск через інтерпретатор:** Можна явно вказати інтерпретатор:
        ```bash
        bash myscript.sh
        ```
        У цьому випадку шебанг ігнорується, і скрипт виконується вказаним інтерпретатором.

### 2.3. Які основні компоненти материнської плати ви знаєте?

Материнська плата є центральним компонентом комп'ютера, що забезпечує зв'язок між усіма іншими пристроями. Основні компоненти включають:
*   **Сокет процесора (CPU Socket):** Місце для встановлення центрального процесора (CPU).
*   **Слоти пам'яті (RAM Slots):** Роз'єми для встановлення модулів оперативної пам'яті (RAM), зазвичай DDR4 або DDR5.
*   **Чіпсет (Chipset):** Набір мікросхем, що керує взаємодією між CPU, пам'яттю та периферійними пристроями. Зазвичай складається з північного мосту (Northbridge, якщо є) та південного мосту (Southbridge).
*   **Слоти розширення (Expansion Slots):** Роз'єми для встановлення додаткових карт, таких як відеокарти (PCIe x16), звукові карти, мережеві карти (PCIe x1) тощо.
*   **Порти зберігання даних (Storage Ports):** Роз'єми для підключення накопичувачів, таких як SATA (для HDD/SSD) та M.2 (для NVMe SSD).
*   **Порти вводу/виводу (I/O Ports):** Роз'єми на задній панелі для підключення периферійних пристроїв (USB, Ethernet, HDMI, DisplayPort, аудіороз'єми).
*   **BIOS/UEFI (Basic Input/Output System / Unified Extensible Firmware Interface):** Прошивка, що зберігається на чіпі пам'яті, яка ініціалізує апаратне забезпечення під час завантаження та надає базові функції вводу/виводу.
*   **Батарея CMOS (CMOS Battery):** Живить чіп CMOS, який зберігає налаштування BIOS/UEFI та системний час, коли комп'ютер вимкнений.
*   **Коннектори живлення (Power Connectors):** Роз'єми для підключення блоку живлення (24-контактний ATX, 8-контактний CPU).
*   **Вбудовані контролери:** Контролери для USB, Ethernet, аудіо, які інтегровані в материнську плату.

### 2.4. Коротко охарактеризуйте для яких пристроїв оперують поняттями MBR та GPT?

**MBR (Master Boot Record) та GPT (GUID Partition Table)** - це два стандарти для розмітки розділів на пристроях зберігання даних, таких як жорсткі диски (HDD) та твердотільні накопичувачі (SSD).

*   **MBR (Master Boot Record):** Це старіший стандарт, який був представлений з IBM PC DOS 2.0 у 1983 році. Він використовується для:
    *   **Завантажувальних дисків:** Диски, з яких завантажується операційна система.
    *   **Дисків розміром до 2 ТБ:** MBR має обмеження на максимальний розмір диска в 2 ТБ. Якщо диск більший, то простір понад 2 ТБ не буде доступний.
    *   **Обмеженої кількості розділів:** MBR підтримує до 4 основних розділів або 3 основних і 1 розширений розділ, який може містити логічні диски.
    *   **Старіших систем:** Зазвичай використовується на системах з BIOS-завантаженням.

*   **GPT (GUID Partition Table):** Це новіший стандарт, який є частиною UEFI (Unified Extensible Firmware Interface), що прийшов на зміну BIOS. GPT використовується для:
    *   **Дисків розміром понад 2 ТБ:** GPT не має практичних обмежень на розмір диска (до 9.4 ЗБ).
    *   **Великої кількості розділів:** GPT підтримує до 128 розділів за замовчуванням (кількість може бути збільшена).
    *   **Сучасних систем:** Зазвичай використовується на системах з UEFI-завантаженням.
    *   **Підвищеної надійності:** GPT зберігає кілька копій таблиці розділів по всьому диску і має контрольні суми CRC для виявлення пошкоджень, що робить його більш надійним, ніж MBR.


### 2.5. В чому суть операції монтування, для чого вона потрібна?

**Монтування (mounting)** - це процес, за допомогою якого операційна система робить файли та каталоги на пристрої зберігання даних (наприклад, жорсткому диску, USB-накопичувачі, CD/DVD-ROM, мережевому ресурсі) доступними для користувача через файлову систему комп'ютера.

**Суть операції:**
Коли пристрій монтується, його файлова система приєднується до певного каталогу (точки монтування) в існуючій файловій системі операційної системи. Після монтування вміст пристрою стає доступним, ніби він є частиною цього каталогу. Доступ до файлів на пристрої здійснюється через цю точку монтування, а не безпосередньо через сам пристрій.

**Для чого вона потрібна:**
1.  **Доступ до даних:** Без монтування операційна система не знає, як інтерпретувати дані на пристрої зберігання. Монтування надає структурований доступ до файлів та каталогів.
2.  **Організація файлової системи:** Дозволяє інтегрувати різні пристрої зберігання в єдину, логічну ієрархічну файлову систему, що зручно для користувача та програм.
3.  **Управління пристроями:** Операційна система може керувати доступом до пристроїв, правами користувачів та іншими параметрами через точки монтування.
4.  **Гнучкість:** Дозволяє підключати та відключати пристрої "на льоту" (наприклад, USB-флешки) без перезавантаження системи.
5.  **Різноманітність файлових систем:** Linux підтримує безліч файлових систем (ext4, NTFS, FAT32, XFS тощо). Монтування дозволяє системі працювати з ними всіма, перетворюючи їх у єдиний інтерфейс.

Приклад: Коли ви підключаєте USB-ф накопичувач до комп'ютера з Linux, операційна система автоматично (або вручну) монтує його до каталогу, наприклад, `/media/username/usb_drive`. Після цього ви можете отримати доступ до файлів на флешці, перейшовши до цього каталогу.

## 3. Хід роботи

### 3.1. Опрацювання прикладів команд з лабораторних робіт 11 та 12

Наступна таблиця містить опис команд, виконаних під час лабораторних робіт 11 та 12, а також їх функціональність та результати.

| Назва команди | Її призначення та функціональність | Результат виконання |
|---|---|---|
| `echo "Hello World" > mymessage` | Створює або перезаписує файл `mymessage` з вмістом "Hello World". | Файл `mymessage` створено/перезаписано з вмістом "Hello World". |
| `cat mymessage` | Виводить вміст файлу `mymessage` на стандартний вивід. | Виведено вміст "Hello World". |
| `cat << 'EOF' > sample.sh ... EOF` | Створює файл `sample.sh` з багаторядковим вмістом, використовуючи "here document" синтаксис. | Файл `sample.sh` створено з вказаним вмістом скрипта. |
| `bash sample.sh` | Виконує скрипт `sample.sh` за допомогою інтерпретатора `bash`. | Виведено привітання та календар на поточний місяць. |
| `ls -l sample.sh` | Виводить детальну інформацію про файл `sample.sh` (права доступу, власник, розмір, дата). | Показано права доступу, які не включають виконання. |
| `chmod a+x sample.sh` | Надає права на виконання (`+x`) файлу `sample.sh` для всіх користувачів (`a`). | Права на виконання додано до `sample.sh`. |
| `ls -l sample.sh` | Повторно виводить детальну інформацію про файл `sample.sh` для перевірки змінених прав. | Показано права доступу, які тепер включають виконання (`-rwxrwxr-x`). |
| `./sample.sh` | Виконує скрипт `sample.sh` безпосередньо, використовуючи шебанг для визначення інтерпретатора. | Виведено привітання та календар на поточний місяць. |
| `echo "Today is $(date +%A)"` | Виводить рядок "Today is" з назвою поточного дня тижня, отриманою за допомогою команди `date +%A`. | Виведено "Today is Saturday" (або інший поточний день). |
| `echo $PATH` | Виводить значення змінної середовища `PATH`, яка містить список каталогів, де система шукає виконувані файли. | Виведено список каталогів у `PATH`. |
| `lscpu | head -n 15` | Виводить інформацію про архітектуру CPU та перенаправляє перші 15 рядків на стандартний вивід. | Виведено основні характеристики CPU (архітектура, кількість ядер, модель тощо). |
| `head -n 20 /proc/cpuinfo` | Виводить перші 20 рядків файлу `/proc/cpuinfo`, який містить детальну інформацію про процесор(и). | Виведено детальну інформацію про CPU. |
| `free -m` | Виводить інформацію про використання оперативної пам'яті та swap-розділу в мегабайтах (`-m`). | Показано загальний, використаний та вільний обсяг RAM та Swap у МБ. |
| `free -g` | Виводить інформацію про використання оперативної пам'яті та swap-розділу в гігабайтах (`-g`). | Показано загальний, використаний та вільний обсяг RAM та Swap у ГБ. |
| `lspci | head -n 10` | Виводить інформацію про всі PCI-пристрої та перенаправляє перші 10 рядків на стандартний вивід. | Виведено список PCI-пристроїв (відеокарти, мережеві адаптери тощо). |
| `lsusb` | Виводить інформацію про всі USB-пристрої, підключені до системи. | Виведено список USB-припристроїв. |
| `lsmod | head -n 10` | Виводить список завантажених модулів ядра та перенаправляє перші 10 рядків на стандартний вивід. | Виведено список завантажених модулів ядра. |
| `sudo fdisk -l | head -n 20` | Виводить інформацію про розділи на всіх дисках (`-l`) з правами суперкористувача (`sudo`) та перенаправляє перші 20 рядків на стандартний вивід. | Виведено інформацію про дискові розділи (розмір, тип, ідентифікатор). |

### 3.2. Створення скриптових сценаріїв

#### 3.2.1. Сценарій привітання користувача та інформації про систему

**greeting.sh**
```bash
#!/bin/bash

echo "Hello, $USER!"
echo "Today is $(date)"
echo "System Information:"
uname -a
```

**Результат виконання:**
```text
--- Running greeting.sh ---
Hello, ubuntu!
Today is Sat Mar 14 08:19:03 EDT 2026
System Information:
Linux 6e143c33abae 6.1.102 #1 SMP PREEMPT_DYNAMIC Tue Mar 25 09:02:46 UTC 2025 x86_64 x86_64 x86_64 GNU/Linux
```

#### 3.2.2. Сценарій інформації про апаратну конфігурацію

**hw_info.sh**
```bash
#!/bin/bash

echo "--- CPU Information ---"
lscpu | grep -E "Architecture|CPU\(s\)|Model name"
echo ""
echo "--- Memory Information ---"
free -h
echo ""
echo "--- Disk Information ---"
df -h | grep "^/dev/"
```

**Результат виконання:**
```text
--- Running hw_info.sh ---
--- CPU Information ---
Architecture:                         x86_64
CPU(s):                               6
On-line CPU(s) list:                  0-5
Model name:                           Intel(R) Xeon(R) Processor @ 2.50GHz
NUMA node0 CPU(s):                    0-5

--- Memory Information ---
               total        used        free      shared  buff/cache   available
Mem:           3.8Gi       799Mi       1.3Gi        11Mi       1.8Gi       2.8Gi
Swap:          2.0Gi          0B       2.0Gi

--- Disk Information ---
/dev/root        42G   11G   32G  25% /
```

#### 3.2.3. Власний приклад скриптового сценарію (перевірка використання диска)

**disk_alert.sh**
```bash
#!/bin/bash
# A script to check disk usage and alert if it's above 80%

THRESHOLD=80
USAGE=$(df / | grep / | awk '{ print $5 }' | sed 's/%//g')

echo "Current disk usage on / is ${USAGE}%"

if [ "$USAGE" -gt "$THRESHOLD" ]; then
    echo "WARNING: Disk usage is above ${THRESHOLD}%!"
else
    echo "Disk usage is within safe limits."
fi
```

**Результат виконання:**
```text
--- Running disk_alert.sh ---
Current disk usage on / is 25%
Disk usage is within safe limits.
```

## 4. Контрольні запитання

### 4.1. В чому відмінність між командами `arch` та `lscpu`?

*   **`arch`:** Ця команда виводить архітектуру машини (наприклад, `x86_64`). Вона є простою і надає лише базову інформацію про тип процесора, для якого скомпільована операційна система. Це еквівалентно `uname -m`.
*   **`lscpu`:** Ця команда надає більш детальну інформацію про архітектуру CPU, включаючи кількість CPU, ядер, потоків, модель, vendor ID, кеш, режими роботи CPU (32-бітний, 64-бітний) та багато іншого. Вона збирає інформацію з `/proc/cpuinfo` та інших джерел і представляє її у зручному для читання форматі.

**Основна відмінність:** `arch` надає дуже загальну інформацію про архітектуру системи, тоді як `lscpu` надає вичерпну та детальну інформацію про центральний процесор та його характеристики.

### 4.2. Якою командою можна отримати інформацію про стан використання RAM поточною системою?

Інформацію про стан використання оперативної пам'яті (RAM) можна отримати за допомогою команди `free`.

*   **`free`:** Виводить загальний обсяг фізичної та swap-пам'яті, а також обсяг використаної та вільної пам'яті, буферів та кешу.
    *   `free -h`: Виводить інформацію у зручному для читання форматі (наприклад, ГБ, МБ).
    *   `free -m`: Виводить інформацію в мегабайтах.
    *   `free -g`: Виводить інформацію в гігабайтах.

Приклад виводу `free -h`:
```text
               total        used        free      shared  buff/cache   available
Mem:           3.8Gi       799Mi       1.3Gi        11Mi       1.8Gi       2.8Gi
Swap:          2.0Gi          0B       2.0Gi
```

### 4.3. Яким чином у скриптах можна опрацьовувати змінні та створювати розгалужені та циклічні сценарії?

#### Опрацювання змінних

У Bash-скриптах змінні використовуються для зберігання даних. Їх можна оголошувати, присвоювати їм значення та використовувати.

*   **Оголошення та присвоєння:**
    ```bash
    MY_VARIABLE="Hello, Bash!"
    NUMBER=10
    ```
    (Без пробілів навколо знака `=`).
*   **Доступ до значення:** Для доступу до значення змінної використовується префікс `$`:
    ```bash
    echo $MY_VARIABLE
    echo "The number is: ${NUMBER}"
    ```
*   **Змінні середовища:** Існують також змінні середовища (наприклад, `PATH`, `USER`, `HOME`), які доступні у всіх скриптах та програмах.

#### Розгалужені сценарії (умовні оператори)

Розгалуження дозволяє скрипту виконувати різні блоки коду залежно від певних умов. Найпоширенішим є оператор `if`.

*   **Синтаксис `if/else`:**
    ```bash
    if [ condition ]; then
        # Команди, якщо умова істинна
    else
        # Команди, якщо умова хибна
    fi
    ```
*   **Синтаксис `if/elif/else`:**
    ```bash
    if [ condition1 ]; then
        # Команди для condition1
    elif [ condition2 ]; then
        # Команди для condition2
    else
        # Команди, якщо жодна умова не істинна
    fi
    ```
*   **Приклади умов:**
    *   `[ -f "file.txt" ]`: Перевірка, чи існує файл `file.txt`.
    *   `[ -d "my_dir" ]`: Перевірка, чи існує каталог `my_dir`.
    *   `[ "$VAR1" = "$VAR2" ]`: Порівняння рядків.
    *   `[ $NUM1 -gt $NUM2 ]`: Порівняння чисел (`-gt` - більше, `-lt` - менше, `-eq` - дорівнює).

#### Циклічні сценарії

Цикли дозволяють повторювати виконання блоку коду. Найпоширеніші цикли - `for` та `while`.

*   **Цикл `for`:**
    ```bash
    for i in 1 2 3 4 5; do
        echo "Number: $i"
    done
    
    for file in *.txt; do
        echo "Processing file: $file"
    done
    ```
*   **Цикл `while`:**
    ```bash
    COUNT=1
    while [ $COUNT -le 5 ]; do
        echo "Count: $COUNT"
        COUNT=$((COUNT + 1))
    done
    ```

### 4.4. Які команди для перегляду стану підключення периферійних пристроїв можна використати в терміналі?

Для перегляду стану підключення периферійних пристроїв у терміналі Linux можна використовувати наступні команди:

*   **`lspci`:** Виводить інформацію про всі пристрої, підключені до шини PCI (Peripheral Component Interconnect). Це включає відеокарти, мережеві адаптери, звукові карти, контролери SATA/NVMe тощо.
    *   `lspci -v`: Більш детальний вивід.
    *   `lspci -nn`: Виводить ID виробника та пристрою.

*   **`lsusb`:** Виводить інформацію про всі USB-пристрої та USB-контролери. Це включає клавіатури, миші, принтери, зовнішні накопичувачі, веб-камери тощо.
    *   `lsusb -v`: Більш детальний вивід.

*   **`lsblk` (List Block Devices):** Виводить інформацію про всі блокові пристрої (жорсткі диски, SSD, USB-накопичувачі, CD/DVD-ROM) та їх розділи у вигляді дерева. Показує точки монтування.
    *   `lsblk -f`: Показує інформацію про файлові системи.

*   **`fdisk -l` / `sfdisk -l` / `parted -l`:** Ці команди використовуються для перегляду таблиць розділів на дисках. `fdisk` працює з MBR та GPT, `parted` є більш сучасною та підтримує GPT.
    *   `sudo fdisk -l`: Виводить інформацію про розділи на всіх дисках.

*   **`dmesg | grep -i usb` / `dmesg | grep -i pci`:** Команда `dmesg` виводить повідомлення ядра. Фільтруючи їх за `usb` або `pci`, можна побачити події, пов'язані з підключенням/відключенням цих пристроїв.

*   **`ip a` (або `ifconfig`):** Для мережевих адаптерів показує їхній стан (підключено/відключено), IP-адреси, MAC-адреси.

*   **`aplay -l` / `arecord -l`:** Для аудіопристроїв показує список доступних звукових карт та пристроїв вводу/виводу.

### 4.5. Які можливості застосунку `gparted`?

`GParted` (GNOME Partition Editor) - це потужний графічний редактор розділів диска для Linux, який дозволяє керувати дисковим простором. Він є частиною проекту GNOME і часто використовується як інструмент для відновлення системи, зміни розміру розділів або підготовки диска.

**Основні можливості `GParted`:**

1.  **Створення розділів:** Дозволяє створювати нові розділи на диску, вибираючи тип файлової системи (ext2/3/4, FAT16/32, NTFS, XFS, Btrfs, swap тощо).
2.  **Видалення розділів:** Дозволяє видаляти існуючі розділи, звільняючи місце на диску.
3.  **Зміна розміру розділів:** Можливість збільшувати або зменшувати розмір існуючих розділів без втрати даних (для підтримуваних файлових систем). Це корисно, наприклад, для розширення системного розділу.
4.  **Переміщення розділів:** Дозволяє переміщувати розділи на диску.
5.  **Копіювання розділів:** Можливість копіювати розділи з одного диска на інший або в інше місце на тому ж диску.
6.  **Форматування розділів:** Дозволяє форматувати розділи в різні файлові системи.
7.  **Перевірка файлових систем:** Може перевіряти файлові системи на наявність помилок та виправляти їх.
8.  **Управління прапорами розділів:** Дозволяє встановлювати або знімати прапори (наприклад, `boot`, `hidden`, `lvm`).
9.  **Підтримка різних таблиць розділів:** Працює як з MBR (Master Boot Record), так і з GPT (GUID Partition Table).
10. **Відновлення даних:** Хоча `GParted` не є інструментом для відновлення файлів, він може допомогти відновити пошкоджені таблиці розділів, що є першим кроком до відновлення даних.
11. **Графічний інтерфейс:** Надає інтуїтивно зрозумілий графічний інтерфейс, що робить управління дисками доступним навіть для менш досвідчених користувачів, на відміну від командних утиліт, таких як `fdisk` або `parted`.

`GParted` є дуже цінним інструментом для будь-якого користувача або системного адміністратора Linux, який потребує гнучкого та надійного управління дисковим простором.

## 5. Висновки (Conclusions)

In this laboratory work, we successfully explored the fundamentals of shell scripting in Bash and gained practical experience with commands for determining system hardware configuration. We learned how to create, edit, and execute shell scripts, understanding the importance of the shebang line and file permissions. The exercises involved developing scripts for user greetings, displaying system information, and monitoring disk usage, which are essential skills for task automation and system administration.

Furthermore, we investigated various Linux commands such as `lscpu`, `free`, `lspci`, `lsusb`, `lsmod`, and `fdisk -l` to gather detailed information about the CPU, memory, PCI devices, USB devices, kernel modules, and disk partitioning. This provided a comprehensive understanding of how to assess the hardware components of a Linux system. We also delved into theoretical concepts, including the components of a motherboard, the differences between MBR and GPT partitioning schemes, and the significance of the mounting operation in a Unix-like file system. The capabilities of `gparted` as a graphical partition editor were also examined.