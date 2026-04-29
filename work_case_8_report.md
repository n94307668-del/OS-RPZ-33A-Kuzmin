Ворк-кейс №8 

1. 
- Перегляд файлів та папок через файловий менеджер у терміналі.
Команди/пакети:
  • ls — базова команда для перегляду вмісту каталогу (list directory contents).
  • tree — виводить структуру каталогів у вигляді дерева.
  • mc (Midnight Commander) — повноцінний файловий менеджер з текстовим інтерфейсом (TUI).
  • ranger — файловий менеджер з навігацією у стилі Vim.
  • nnn — легкий та швидкий файловий менеджер.
Методика виконання:
  $ ls -la /home
  $ tree -L 2 /mnt
  $ sudo apt install mc && mc
  $ sudo apt install ranger && ranger

- Переглядати веб-сторінки через браузер, що працює у терміналі.
Команди/пакети:
  • lynx — текстовий веб-браузер.
  • w3m — текстовий веб-браузер з підтримкою таблиць і фреймів.
  • links/elinks — текстові браузери.
  • curl — інструмент для передачі даних із URL (можна переглянути сирці HTML або завантажити файли).
Методика виконання:
  $ sudo apt install lynx && lynx https://example.com
  $ sudo apt install w3m && w3m https://example.com
  $ curl -s https://example.com | less

- Перегляд електронної пошти в терміналі.
Команди/пакети:
  • mutt — текстовий поштовий клієнт.
  • neomutt — сучасний форк mutt.
  • alpine — простий у використанні текстовий поштовий клієнт.
  • mail/mailx — базова утиліта для роботи з поштою (SMTP/MTA).
Методика виконання:
  $ sudo apt install neomutt && neomutt
  $ sudo apt install alpine && alpine

- Слухати музику через термінал.
Команди/пакети:
  • cmus — консольний музичний програвач.
  • moc (Music on Console) — легкий аудіоплеєр.
  • mpg123 / ogg123 — консольні програвачі для MP3/OGG.
  • cvlc — консольна версія VLC.
  • mpv — медіаплеєр, який може працювати без GUI.
  • speaker-test — утиліта ALSA для тестування аудіо.
Методика виконання:
  $ sudo apt install cmus && cmus /path/to/music
  $ sudo apt install moc && mocp
  $ mpv --no-video track.mp3

- Скачувати торенти через термінал.
Команди/пакети:
  • rtorrent — текстовий BitTorrent-клієнт.
  • transmission-cli — CLI-версія Transmission.
  • aria2c — універсальна утиліта для завантаження (підтримує Magnet-посилання з додатковими налаштуваннями).
Методика виконання:
  $ sudo apt install rtorrent && rtorrent file.torrent
  $ sudo apt install transmission-cli && transmission-cli file.torrent
  $ aria2c magnet:?xt=urn:btih:...

- Планувати дії в календарі та нагадувати про них через термінал.
Команди/пакети:
  • calcurse — календар і органайзер з текстовим інтерфейсом.
  • remind — потужна система нагадувань.
  • calendar — класична BSD-утиліта для подій.
  • at — планувальник разових завдань.
  • cron — регулярне планування завдань.
  • date — перегляд поточної дати та обчислення дат.
Методика виконання:
  $ sudo apt install calcurse && calcurse
  $ echo "backup.sh" | at 02:00 tomorrow
  $ crontab -e

- Переглядати зображення в терміналі.
Команди/пакети:
  • fbi (Linux framebuffer) / fim — перегляд зображень у консолі.
  • chafa — перетворення зображень на текстові символи (ASCII/Unicode).
  • catimg — швидкий перегляд зображень у терміналі.
  • img2txt (з AAlib) — конвертація зображення в ASCII-арт.
  • timg — перегляд зображень і відео в терміналі з підтримкою кольорів.
Методика виконання:
  $ sudo apt install fim && fim image.jpg
  $ sudo apt install chafa && chafa image.jpg
  $ sudo apt install catimg && catimg image.png

2. 
- Вводити, редагувати, видаляти текст (редактори файлів).
Пакети:
  • nano — простий консольний редактор для початківців.
  • vim — потужний модальний редактор.
  • emacs — розширюваний редактор з власною екосистемою.
  • micro — сучасний редактор з мишкою і підсвічуванням синтаксису.
Методика виконання:
  $ sudo apt install nano && nano file.txt
  $ sudo apt install vim && vim file.txt
  $ sudo apt install micro && micro file.txt

- Здійснювати моніторинг процесів та ресурсів системи (аналог диспетчера задач або системного монітора в графічній оболонці).
Пакети:
  • top — базовий перегляд процесів і ресурсів у реальному часі.
  • htop — покращена версія top з кольоровим інтерфейсом і підтримкою миші.
  • btop — сучасний монітор з графіками (CPU, RAM, диск, мережа).
  • glances — розширений монітор з веб-інтерфейсом.
  • vmstat / iostat / mpstat — статистика ресурсів.
  • nethogs — моніторинг мережевого трафіку по процесах.
Методика виконання:
  $ sudo apt install htop && htop
  $ sudo apt install btop && btop
  $ top -b -n 1

3. 
- Якщо Ви мрієте про подорож, то термінал може Вам показати зображення парового локомотиву з вагоном (гарний натяк на дорогу).
  • sl (Steam Locomotive) — анімація паровоза, що проїжджає через термінал.
  Методика:
  $ sudo apt install sl && sl

- Якщо Ви фанат зоряних війн, то термінал може їх Вам показати.
  • ASCII-арт версія «Зоряних воєн» через telnet.
  Методика:
  $ sudo apt install telnet && telnet towel.blinkenlights.nl
  (Або: $ nc towel.blinkenlights.nl 23)

- Якщо ви любите тварин, то термінал Вам може організувати діалог з коровою.
  • cowsay — ASCII-арт корова, що вимовляє введений текст.
  • cowthink — думки корови.
  Методика:
  $ sudo apt install cowsay && cowsay "Hello Linux"
  $ sudo apt install fortune-mod cowsay && fortune | cowsay

- Можливо Ви знаєте якісь цікаві інтерактиви, які не знаю я, то поділіться ними зі мною :)))
  • cmatrix — «Матриця» з падаючими символами.
  • lolcat — райдужне фарбування тексту.
  • figlet / toilet — великі ASCII-заголовки.
  • oneko — кішка, що бігає за курсором (у графічному терміналі).
  • xeyes — очі, що слідкують за курсором (X11).
  • rig — генератор випадкових ідентифікаційних даних.
  Методика:
  $ sudo apt install cmatrix && cmatrix
  $ sudo apt install lolcat && echo "Rainbow" | lolcat
  $ sudo apt install figlet && figlet "LINUX"


Словник англ. термінів:
  • Terminal — термінал (пристрій/програма для введення команд)
  • CLI (Command Line Interface) — інтерфейс командного рядка
  • TUI (Text-based User Interface) — текстовий інтерфейс користувача
  • File manager — файловий менеджер
  • Web browser — веб-браузер
  • Email client — поштовий клієнт
  • Media player — медіаплеєр
  • Torrent client — торрент-клієнт
  • Calendar / Scheduler — календар / планувальник
  • Image viewer — переглядач зображень
  • Text editor — текстовий редактор
  • System monitor / Process monitor — системний / процесний монітор
  • Task manager — диспетчер задач
  • Easter egg — пасхалка (прихована функція або жарт)
  • ASCII art — ASCII-графіка (зображення з текстових символів)
  • Package / Packet — пакет (програмний модуль або мережевий пакет)
  • Distribution (distro) — дистрибутив (операційна система на базі Linux)
  • Resource — ресурс (процесор, пам’ять, диск тощо)
  • Process — процес (запущена програма)

---

Conclusions:
The practical study of Linux terminal utilities confirms that most everyday and administrative tasks can be performed efficiently without a graphical desktop environment. File browsing (ls, tree, mc), web access (lynx, w3m, curl), email (mutt, alpine), multimedia (cmus, mpv), torrents (rtorrent, transmission-cli), scheduling (calcurse, cron, at), and even image preview (chafa, fim) all have robust CLI/TUI counterparts. For system administration, top/htop/btop provide comprehensive process and resource monitoring, while vim/nano/micro cover text-editing needs. Furthermore, terminal easter eggs such as sl, cowsay, cmatrix, and the Star Wars ASCII animation via telnet demonstrate that the command line can be both highly productive and entertaining.