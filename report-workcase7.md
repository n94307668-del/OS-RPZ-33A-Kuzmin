# Work-case 7: Планувальники завдань в ОС

**Тема:** Планування задач в ОС Linux

## Завдання 1: Теоретичні питання

### Основні функції планувальника завдань

Будь-який планувальник задач виконує такі функції:

| Функція | Опис |
|---------|------|
| **Запуск у визначений час** | Одноразове або повторюване виконання |
| **Періодичне виконання** | Заданий інтервал (година, день, тиждень) |
| **Умовний запуск** | Тригери: завантаження, простій, подія |
| **Привілеї та безпека** | Запуск від імені конкретного користувача |
| **Логування** | Запис результатів виконання |
| **Повідомлення** | Попередження про помилки, e-mail звіти |

### Порівняння: Windows vs Linux

| Критерій | Windows Task Scheduler | Linux Cron |
|----------|----------------------|------------|
| **Команда/Інструмент** | `taskschd.msc`, `schtasks` | `crontab`, `cron` |
| **Інтерфейс** | GUI + PowerShell/CLI | Тільки CLI |
| **Тригери** | Час, події, завантаження, простій | Час (`@reboot` для завантаження) |
| **Формат розкладу** | Майстер налаштування, XML | Текстовий `* * * * *` |
| **Повідомлення** | Вбудована пошта, Event Log | Зовнішні скрипти (mailx) |
| **Служба** | `Schedule` (Windows Service) | `cron` (systemd service) |

**Приклад Windows Task Scheduler (PowerShell):**

![Windows Task Scheduler](images/windows_tasks.png)

### Планувальник Cron в Linux

**Cron** — фоновий демон, що виконує команди за розкладом.

**Основні команди:**
- `crontab -e` — редагувати розклад
- `crontab -l` — переглянути розклад
- `crontab -r` — видалити всі задачі
- `sudo systemctl status cron` — перевірити статус служби

**Формат запису:**

```
* * * * * command
│ │ │ │ │
│ │ │ │ └─── День тижня (0-7)
│ │ │ └───── Місяць (1-12)
│ │ └─────── День місяця (1-31)
│ └───────── Година (0-23)
└─────────── Хвилина (0-59)
```

**Спеціальні макроси:**

| Макрос | Значення |
|--------|----------|
| `@reboot` | При завантаженні системи |
| `@yearly` / `@annually` | Щорічно 1 січня о 00:00 |
| `@monthly` | Щомісяця 1-го числа о 00:00 |
| `@weekly` | Щонеділі о 00:00 |
| `@daily` / `@midnight` | Щодня о 00:00 |
| `@hourly` | Щогодини |

![Синтаксис Cron](images/cron_syntax.png)

### Альтернативи Cron

| Планувальник | Особливості |
|-------------|-------------|
| **systemd timers** | Інтеграція з systemd, точне планування, залежності між службами |
| **anacron** | Для систем без 24/7 роботи (допускає пропуски) |
| **fcron** | Підтримує "запуск при простої", складні умови |
| **bcron** | Підвищена безпека, розділення привілеїв |
| **at** | Одноразові задачі (не періодичні) |

**Anacron** — оптимальний для ноутбуків/робочих станцій, що вимикаються. Використовує файл `/etc/anacrontab`.

![Anacron](images/anacron.png)

---

## Завдання 2: Планування задач через Cron

**1. О 8:00 ранку — резервне копіювання**
```cron
0 8 * * * /home/user/scripts/backup.sh
```

**2. Двічі на день (9:00 та 18:30) — очищення `/tmp`**
```cron
0 9,18 * * * /home/user/scripts/cleanup.sh
```

**3. Будні (пн-пт) з 8 до 18 — перевірка диска**
```cron
0 8-18 * * 1-5 /home/user/scripts/diskcheck.sh
```

**4. Щогодини — запис часу у файл**
```cron
0 * * * * date >> /home/user/hourly_log.txt
```

**5. Щодня о 6:00 — оновлення пакетів**
```cron
0 6 * * * /usr/bin/apt update >> /var/log/apt_cron.log 2>&1
```

**6. Щомісяця 1-го числа о 3:00 — архівування логів**
```cron
0 3 1 * * /home/user/scripts/archive_logs.sh
```

**7. Щорічно 1 січня о 0:00 — святкове привітання**
```cron
0 0 1 1 * echo "Happy New Year!" >> /home/user/newyear.txt
```

**8. При кожному завантаженні — запуск моніторингу**
```cron
@reboot /home/user/scripts/monitor.sh
```

### Скріншоти виконання

**Перегляд усіх задач:**

![crontab -l](images/crontab_list.png)

**Редагування через nano:**

![crontab -e](images/crontab_edit.png)

**Логи виконання cron в syslog:**

![Cron logs](images/cron_logs.png)

---

## Завдання 3: Альтернативний планувальник — systemd timers

Встановлено та налаштовано **systemd timers** як альтернативу cron. Кожна cron-задача реалізована через пару файлов: `.timer` + `.service`.

**Приклад: щоденний backup**

```ini
# /etc/systemd/system/backup-daily.timer
[Unit]
Description=Daily backup timer

[Timer]
OnCalendar=*-*-* 08:00:00
Persistent=true

[Install]
WantedBy=timers.target
```

```ini
# /etc/systemd/system/backup-daily.service
[Unit]
Description=Daily backup service

[Service]
Type=oneshot
ExecStart=/home/user/scripts/backup.sh
```

**Активація:**
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now backup-daily.timer
```

---

## Glossary of Terms

| English Term | Ukrainian Translation | Definition |
|-------------|----------------------|------------|
| **Task scheduler** | Планувальник завдань | System component that executes tasks at specified times or intervals |
| **Cron** | Крон | Time-based job scheduler daemon in Unix-like operating systems |
| **Crontab** | Кронтаб | File containing the schedule of cron jobs for a specific user |
| **Daemon** | Демон | Background process that runs continuously and provides services |
| **Systemd timer** | Таймер systemd | Scheduling mechanism integrated with the systemd init system |
| **Anacron** | Анакрон | Scheduler designed for systems not running 24/7 |
| **Backup** | Резервне копіювання | Copying data to prevent loss in case of failure |
| **Archive** | Архівування | Compressing and storing files for long-term retention |
| **Log rotation** | Ротація логів | Automatic management of log file size and retention |
| **Reboot** | Перезавантаження | Restarting the computer system |
| **One-shot** | Одноразовий | Service type that runs once and then exits |
| **Trigger** | Тригер | Event or condition that initiates task execution |
| **Privilege** | Привілей | Permission level granted to a user or process |
| **Timezone** | Часовий пояс | Geographic region with the same standard time |

---

## Conclusions

This practical work covered task scheduling in Linux operating systems. I configured the **Cron daemon** to run 8 different automated tasks: daily backups, twice-daily cleanup, weekday disk checks, hourly logging, monthly archiving, yearly greetings, system updates, and boot-time monitoring. All tasks were verified through syslog logs confirming successful execution.

The **Cron syntax** proved to be concise and powerful for time-based scheduling, using the five-field format (`* * * * *`). Special macros like `@reboot`, `@daily`, `@monthly`, and `@yearly` simplify common scheduling patterns significantly.

As an alternative, I explored **systemd timers** which offer several advantages over traditional cron: tighter integration with the system initialization process, built-in logging through `journalctl`, service dependency management, and more precise timing capabilities. Systemd timers use `.timer` and `.service` unit files, making them more verbose but also more flexible than cron entries.

**Anacron** was also examined as a solution for non-continuous systems, ensuring missed jobs run when the system becomes available.

---