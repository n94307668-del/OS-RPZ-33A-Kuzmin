# Звіт по виконанню Work-case №3

**Виконав:** студент групи РПЗ-33А Кузьмін Нікіта  

---

## 1. Клонування та експорт віртуальної ОС

### 1.1. Клонування віртуальної машини

Клонування віртуальної машини дозволяє створити повну копію існуючої ВМ. У VirtualBox це можна зробити двома способами:

**Спосіб 1: Через графічний інтерфейс**

1.  Відкрийте головне вікно VirtualBox Manager
2.  Клікніть правою кнопкою миші на потрібній віртуальній машині
3.  Оберіть пункт меню **"Clone..."** (Клонувати)
4.  У вікні майстра клонування вкажіть:
    *   **New machine name** - ім'я для клону (наприклад, "Ubuntu-Clone")
    *   **Path** - шлях для збереження клону
    *   **Type of clone** - тип клонування:
        *   **Full clone** - повне клонування (створює незалежну копію всіх дисків)
        *   **Linked clone** - зв'язане клонування (створює диференціальний диск, залежить від оригіналу)
5.  Натисніть "Clone" для завершення процесу

**Спосіб 2: Через командний рядок (VBoxManage)**

```bash
# Повне клонування
VBoxManage clonevm "Ubuntu-Original" --name "Ubuntu-Clone" --register

# Зв'язане клонування
VBoxManage clonevm "Ubuntu-Original" --name "Ubuntu-Linked-Clone" --mode machine --options link --register

```
[![alt text](image.png)]
```

### 1.2. Експорт віртуальної ОС

Для перенесення віртуальної машини на інший комп'ютер або в інше віртуальне середовище використовується експорт у форматі **OVF** (Open Virtualization Format) або **OVA** (Open Virtualization Archive).

**Процес експорту:**

1.  У VirtualBox Manager оберіть потрібну ВМ
2.  Перейдіть до меню **File → Export Appliance** (Файл → Експортувати віртуальний апарат)
3.  Виберіть віртуальну машину для експорту
4.  Вкажіть шлях та формат файлу:
    *   **OVF Format 2.0** - розділені файли (.ovf + .vmdk/.vdi)
    *   **OVA Format 2.0** - один архівний файл (.ova)
5.  Натисніть "Next" та "Export" для завершення

*Вікно експорту віртуального апарату*

```
[![alt text](image-1.png)]
```

*Процес експорту/результат експорту (файл .ova)*

```
[![alt text](image-2.png)]
```

**Імпорт на іншому середовищі:**

```bash
# Через графічний інтерфейс: File → Import Appliance
# Або через командний рядок:
VBoxManage import "/path/to/exported.ova"
```

---

## 2. Типи мережевих з'єднань у віртуальних машинах

VirtualBox підтримує кілька режимів роботи мережевого адаптера, кожен з яких має свої особливості:

### 2.1. Трансляція мережевих адрес (NAT)

**Опис:** NAT (Network Address Translation) - режим за замовчуванням. ВМ отримує доступ до зовнішньої мережі через віртуальний маршрутизатор, який транслює її приватні IP-адреси в публічні.

**Особливості:**
- ВМ має вихід в Інтернет через хост-систему
- ВМ отримує IP-адресу з приватного діапазону (зазвичай 10.0.2.x)
- Хост-система НЕ має прямого доступу до ВМ
- Кілька ВМ в режимі NAT ізольовані одна від одної
- Підходить для базового доступу до Інтернету

**Налаштування:**
```
Settings → Network → Adapter 1 → Attached to: NAT
```

### 2.2. Мережевий міст (Bridged Adapter)

**Опис:** Bridged Adapter з'єднує віртуальний мережевий адаптер безпосередньо з фізичним адаптером хост-системи.

**Особливості:**
- ВМ підключається до фізичної мережі як окремий пристрій
- ВМ отримує IP-адресу від DHCP-сервера мережі (таку ж, як і інші пристрої)
- ВМ доступна з інших пристроїв у локальній мережі
- Ідеально підходить для серверних додатків та тестування мережевих сервісів
- Вимагає наявності фізичного мережевого адаптера

**Налаштування:**
```
Settings → Network → Adapter 1 → Attached to: Bridged Adapter
Name: [вибрати фізичний адаптер, наприклад, eth0 або Wi-Fi адаптер]
```

### 2.3. Віртуальний адаптер хоста (Host-only Adapter)

**Опис:** Host-only Adapter створює закриту мережу між хост-системою та віртуальними машинами.

**Особливості:**
- Мережа між хостом і ВМ без доступу до зовнішньої мережі
- ВМ можуть спілкуватися між собою та з хостом
- ВМ НЕ мають доступу до Інтернету
- Корисно для тестування в ізольованому середовищі
- VirtualBox створює віртуальний адаптер vboxnet0 на хості

**Налаштування:**
```
Settings → Network → Adapter 1 → Attached to: Host-only Adapter
Name: vboxnet0
```

### 2.4. Внутрішня мережа (Internal Network)

**Опис:** Internal Network створює повністю ізольовану мережу тільки між віртуальними машинами.

**Особливості:**
- Тільки ВМ можуть спілкуватися між собою
- Хост-система НЕ має доступу до цієї мережі
- ВМ НЕ мають доступу до Інтернету
- Ідеально для тестування кластерів та мережевих топологій
- Потрібно вказати ім'я мережі (наприклад, "intnet")

**Налаштування:**
```
Settings → Network → Adapter 1 → Attached to: Internal Network
Name: intnet
```

*налаштувань мережі в VirtualBox*

```
[![alt text](image-3.png)]
```

---

## 3. Розгортання мережі між робочою ОС та її клоном

### 3.1. Налаштування мережевих параметрів

Для організації мережі між двома ВМ з виходом в Інтернет використаємо комбінацію адаптерів:
- **Adapter 1:** NAT - для доступу до Інтернету
- **Adapter 2:** Internal Network - для зв'язку між ВМ

**Налаштування на обох ВМ:**

```
Settings → Network:
- Adapter 1: Attached to: NAT
- Adapter 2: Attached to: Internal Network, Name: intnet
```

**Місце для скріншоту 3.1.1:** *Налаштування мережевих адаптерів (Adapter 1 - NAT)*

```
[]
```

**Місце для скріншоту 3.1.2:** *Налаштування мережевих адаптерів (Adapter 2 - Internal Network)*

```
[ВСТАВИТИ СКРІНШОТ 3.1.2 ТУТ]
```

### 3.2. Базові команди для налаштування мережі

**Перегляд мережевих інтерфейсів:**

```bash
# Перегляд всіх мережевих інтерфейсів
ip addr show
# або
ifconfig -a
```

**Місце для скріншоту 3.2.1:** *Вивід команди ip addr show (показує обидва адаптери)*

```
[ВСТАВИТИ СКРІНШОТ 3.2.1 ТУТ]
```

**Налаштування статичної IP-адреси (для Internal Network):**

```bash
# На першій ВМ (Ubuntu-Original)
sudo ip addr add 192.168.100.10/24 dev enp0s8

# На другій ВМ (Ubuntu-Clone)
sudo ip addr add 192.168.100.11/24 dev enp0s8
```

**Активація інтерфейсу:**

```bash
sudo ip link set enp0s8 up
```

**Перевірка налаштувань:**

```bash
ip addr show enp0s8
```

**Місце для скріншоту 3.2.2:** *Налаштування IP-адреси на першій ВМ*

```
[ВСТАВИТИ СКРІНШОТ 3.2.2 ТУТ]
```

**Місце для скріншоту 3.2.3:** *Налаштування IP-адреси на другій ВМ (клоні)*

```
[ВСТАВИТИ СКРІНШОТ 3.2.3 ТУТ]
```

### 3.3. Перевірка виходу в Інтернет

**Перевірка з'єднання з Інтернетом:**

```bash
# Перевірка ping до зовнішнього сервера
ping -c 4 8.8.8.8
ping -c 4 google.com
```

**Місце для скріншоту 3.3.1:** *Результат команди ping google.com*

```
[ВСТАВИТИ СКРІНШОТ 3.3.1 ТУТ]
```

**Відкриття YouTube у браузері:**

Запустіть браузер Firefox/Chrome та відкрийте будь-яке відео на YouTube.

**Місце для скріншоту 3.3.2:** *Браузер з відкритим YouTube на першій ВМ*

```
[ВСТАВИТИ СКРІНШОТ 3.3.2 ТУТ]
```

**Місце для скріншоту 3.3.3:** *Браузер з відкритим YouTube на другій ВМ (клоні)*

```
[ВСТАВИТИ СКРІНШОТ 3.3.3 ТУТ]
```

### 3.4. Обмін повідомленнями між двома ОС

**Перевірка зв'язку між ВМ:**

```bash
# На першій ВМ (192.168.100.10) - перевірка зв'язку з клоном
ping -c 4 192.168.100.11

# На другій ВМ (192.168.100.11) - перевірка зв'язку з оригіналом
ping -c 4 192.168.100.10
```

**Місце для скріншоту 3.4.1:** *Ping з першої ВМ на другу*

```
[ВСТАВИТИ СКРІНШОТ 3.4.1 ТУТ]
```

**Місце для скріншоту 3.4.2:** *Ping з другої ВМ на першу*

```
[ВСТАВИТИ СКРІНШОТ 3.4.2 ТУТ]
```

**Встановлення netcat для обміну повідомленнями:**

```bash
# На обох ВМ
sudo apt update
sudo apt install netcat-openbsd -y
```

**Запуск сервера на першій ВМ:**

```bash
# Прослуховування порту 12345
nc -l 12345
```

**Підключення клієнта з другої ВМ:**

```bash
# Підключення до сервера
nc 192.168.100.10 12345
```

**Відправка повідомлення:**

Після підключення просто введіть текст та натисніть Enter - повідомлення з'явиться на сервері.

**Місце для скріншоту 3.4.3:** *Термінал з netcat-сервером (перша ВМ)*

```
[ВСТАВИТИ СКРІНШОТ 3.4.3 ТУТ]
```

**Місце для скріншоту 3.4.4:** *Термінал з netcat-клієнтом (друга ВМ) та відправлене повідомлення*

```
[ВСТАВИТИ СКРІНШОТ 3.4.4 ТУТ]
```

### 3.5. Налаштування спільної мережевої папки

**Створення спільної папки на хост-системі:**

1.  Створіть папку на хості (наприклад, `C:\SharedVM` на Windows або `~/SharedVM` на Linux)

**Налаштування в VirtualBox:**

```
Settings → Shared Folders → Add new shared folder
- Folder Path: [шлях до папки на хості]
- Folder Name: shared
- Auto-mount: [позначити]
- Mount point: /mnt/shared
- Read-only: [не позначати]
```

**Місце для скріншоту 3.5.1:** *Налаштування спільної папки в VirtualBox*

```
[ВСТАВИТИ СКРІНШОТ 3.5.1 ТУТ]
```

**Підключення спільної папки в ВМ:**

```bash
# Створення точки монтування
sudo mkdir -p /mnt/shared

# Монтування спільної папки
sudo mount -t vboxsf shared /mnt/shared

# Перевірка вмісту
ls -la /mnt/shared
```

**Місце для скріншоту 3.5.2:** *Монтування спільної папки в терміналі*

```
[ВСТАВИТИ СКРІНШОТ 3.5.2 ТУТ]
```

**Копіювання файлів:**

```bash
# Копіювання зі спільної папки в домашній каталог (перша ВМ)
cp /mnt/shared/example.txt ~/

# Копіювання зі спільної папки на робочий стіл (друга ВМ)
cp /mnt/shared/example.txt ~/Desktop/
```

**Місце для скріншоту 3.5.3:** *Файл, скопійований у домашній каталог першої ВМ*

```
[ВСТАВИТИ СКРІНШОТ 3.5.3 ТУТ]
```

**Місце для скріншоту 3.5.4:** *Файл, скопійований на робочий стіл другої ВМ*

```
[ВСТАВИТИ СКРІНШОТ 3.5.4 ТУТ]
```

---

## 4. Обмін інформацією між основною ОС та віртуальними ОС

### 4.1. Способи обміну файлами

**Спосіб 1: Спільні папки (Shared Folders)**

Найзручніший спосіб, описаний у розділі 3.5.

**Спосіб 2: Drag and Drop (Перетягування)**

```
Settings → General → Advanced:
- Shared Clipboard: Bidirectional
- Drag'n'Drop: Bidirectional
```

Після налаштування можна просто перетягувати файли між хостом і ВМ.

**Спосіб 3: USB-пристрої**

1.  Підключіть USB-накопичувач до хост-системи
2.  У вікні ВМ: Devices → USB → [вибрати USB-пристрій]
3.  Файли копіюються на USB, потім USB підключається до іншої системи

**Спосіб 4: Мережеве оточення (SMB/NFS)**

Налаштування SMB-сервера на ВМ для доступу з Windows-хоста.

### 4.2. Копіювання аудіо-файлу з основної ОС на ВМ

**Через спільну папку:**

1.  Скопіюйте аудіо-файл у спільну папку на хості
2.  В ВМ файл автоматично з'явиться в `/mnt/shared`
3.  Скопіюйте файл на робочий стіл:

```bash
cp /mnt/shared/audio.mp3 ~/Desktop/
```

**Місце для скріншоту 4.2.1:** *Аудіо-файл у спільній папці на хості*

```
[ВСТАВИТИ СКРІНШОТ 4.2.1 ТУТ]
```

**Місце для скріншоту 4.2.2:** *Аудіо-файл на робочому столі віртуальної ОС*

```
[ВСТАВИТИ СКРІНШОТ 4.2.2 ТУТ]
```

**Місце для скріншоту 4.2.3:** *Аудіо-файл на робочому столі клона ВМ*

```
[ВСТАВИТИ СКРІНШОТ 4.2.3 ТУТ]
```

### 4.3. Зворотне копіювання (з ВМ на основну ОС)

**Через спільну папку:**

```bash
# У ВМ - копіювання файлу в спільну папку
cp ~/Desktop/document.txt /mnt/shared/
```

Після цього файл автоматично з'явиться у спільній папці на хост-системі.

**Місце для скріншоту 4.3.1:** *Копіювання файлу з ВМ у спільну папку*

```
[ВСТАВИТИ СКРІНШОТ 4.3.1 ТУТ]
```

**Місце для скріншоту 4.3.2:** *Файл у спільній папці на основній ОС*

```
[ВСТАВИТИ СКРІНШОТ 4.3.2 ТУТ]
```

---

## 5. Glossary of English Terms

*   **Clone (Клон):** An exact copy of a virtual machine, including its configuration and virtual disks.
*   **Linked Clone (Зв'язаний клон):** A clone that depends on the original VM's disks, saving disk space but requiring the original to function.
*   **Full Clone (Повний клон):** An independent copy of a VM with its own separate virtual disks.
*   **Export Appliance (Експорт віртуального апарату):** The process of packaging a VM into a portable format (OVF/OVA) for transfer to another system.
*   **OVF (Open Virtualization Format):** An open standard for packaging and distributing virtual appliances.
*   **OVA (Open Virtualization Archive):** A single archive file containing an OVF package.
*   **NAT (Network Address Translation):** A networking mode that allows VMs to access external networks while hiding their private IP addresses.
*   **Bridged Adapter (Мережевий міст):** A networking mode that connects a VM directly to the physical network as a separate device.
*   **Host-only Adapter (Віртуальний адаптер хоста):** A networking mode that creates a private network between the host and VMs only.
*   **Internal Network (Внутрішня мережа):** An isolated network connecting only virtual machines, with no host access.
*   **Shared Folder (Спільна папка):** A directory on the host system that is accessible from within a virtual machine.
*   **Mount Point (Точка монтування):** A directory in the filesystem where a storage device or shared resource is attached.
*   **IP Address (IP-адреса):** A unique numerical identifier assigned to each device connected to a computer network.
*   **Ping:** A network utility used to test connectivity between devices by sending ICMP echo requests.
*   **Netcat (nc):** A versatile networking utility for reading from and writing to network connections.
*   **DHCP (Dynamic Host Configuration Protocol):** A protocol that automatically assigns IP addresses to devices on a network.
*   **Drag and Drop:** A feature allowing files to be moved between host and guest systems by dragging with a mouse.
*   **Clipboard Sharing (Спільний буфер обміну):** A feature that synchronizes the copy-paste buffer between host and guest systems.

---

## 6. Conclusion

During the execution of Work-case №3, I gained practical skills in managing virtual machines in Oracle VM VirtualBox. The main achievements include:

1.  **Cloning and Export:** I successfully mastered the process of cloning virtual machines using both full and linked clone methods. I also learned how to export VMs to OVF/OVA formats for transfer between different virtualization environments.

2.  **Network Configuration:** I studied and configured all four types of network connections available in VirtualBox: NAT for Internet access, Bridged Adapter for full network integration, Host-only Adapter for isolated host-VM communication, and Internal Network for VM-to-VM communication.

3.  **Inter-VM Networking:** I successfully deployed a network between the original VM and its clone, configured static IP addresses, verified Internet connectivity, and established message exchange between the two systems using netcat utility.

4.  **File Sharing:** I configured shared folders for bidirectional file exchange between the host system and virtual machines, and successfully copied files in both directions.

These skills are essential for system administration, software testing, and network configuration tasks. Understanding virtual networking concepts and VM management techniques provides a solid foundation for working with modern virtualization technologies in professional environments.
