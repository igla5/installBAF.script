PostgreSQL for BAS / 1C Smart Installer

English | Українська
English
Overview

This Bash script automates the installation, tuning, and security setup of PostgreSQL (versions 14 through 18+) specifically compiled and optimized for BAS / 1C enterprise applications.

It automatically detects downloaded local .tar.bz2 release packages, configures locale settings, applies recommended performance tuning parameters (online_analyze, plantuner), secures administrative access, creates a standard database admin user (bafadmin), and holds installed packages from accidental system updates.
Key Features & Requirements

    Supported Operating Systems:

        Ubuntu: 22.04 LTS, 24.04 LTS (and compatible versions)

        Debian: 11 (Bullseye), 12 (Bookworm), 13 (Trixie)

        Linux Mint: 21.x, 22.x (automatically mapped to Ubuntu bases)

    Supported Architecture: amd64 (x86_64) only.

    PostgreSQL Versions: Supports official BAS / 1C builds (PG 14, 15, 16, 17, 18+) with various release suffixes (e.g., 14.20, 15.15-1.1C, 16.11-1.1C, 17.7-1.1C, 18.1-2.1C).

Important Prerequisites: Package Download Procedure

The script does not download PostgreSQL packages automatically because official 1C/BAS packages require authorized access or local distribution.
First Run & Directory Creation

When you run the script for the first time, it automatically checks for the installation folder:

~/install/installPostgre/

(Path relative to the home directory of the non-root user running sudo).

If this directory does not exist, the script creates it and exits immediately with instructions:
Plaintext

============================================================
ATTENTION: INSTALLATION DIRECTORY CREATED
============================================================
Directory for installation packages was created at:
  /home/username/install/installPostgre or ~/install/installPostgre/

Please download the required PostgreSQL release for BAS / 1C from:
  https://dl.bas-soft.eu
  (or https://releases.1c.eu)

Place the downloaded .tar.bz2 package file into:
  /home/username/install/installPostgre/ or ~/install/installPostgre/

After downloading the file, please run this script again.
============================================================

Steps to Proceed:

    Download the required .tar.bz2 archive matching your OS from https://dl.bas-soft.eu or https://releases.1c.eu.

    Copy the .tar.bz2 file into ~/install/installPostgre/.

    (Optional) Place the matching *addon*.tar.bz2 package in the same directory if you intend to install optional addons.

    Re-run the script.

Usage & CLI Options

Make the script executable and run it with sudo:

chmod +x install_pg.sh
sudo ./install_pg.sh

Command-Line Arguments:

    --pg=<version> — Force selection of a major version if multiple archives exist (e.g., --pg=16).

    --addon — Extract and install matching addon packages (*addon*.tar.bz2).

    --no-addon — Skip addon package installation (default behavior).

    --timezone=<zone> — Pre-set system timezone without interactive prompt (e.g., --timezone=Europe/Kyiv).

    -h, --help — Show help message.

What the Script Does Step-by-Step

    Safety & Lock Check: Verifies root privileges and checks apt/dpkg locks before performing system operations.

    Auto-Detection: Scans ~/install/installPostgre/.

        If 1 package is found, it is automatically selected.

        If multiple packages are found, an interactive choice menu is presented.

    Existing Instance Backup: If PostgreSQL is already installed, the script offers to create a full backup (/var/backups/postgresql_backup_<timestamp>.tar.gz) including database dumps (pg_dumpall), /etc/postgresql, and /var/lib/postgresql before purging old packages.

    Configuration: Installs system locales (uk_UA.UTF-8, en_US.UTF-8, de_DE.UTF-8, ru_RU.UTF-8) and updates system timezone.

    BAS / 1C Optimization: Applies tuned values to postgresql.conf:

        shared_buffers = 2GB, work_mem = 64MB, max_connections = 500

        shared_preload_libraries = 'online_analyze,plantuner'

        Sets md5 authentication in pg_hba.conf only local host!!! 
      !!!  If you need to install md5 for 127.0.0.1 and change other settings - stop the PostgreSQL service and make changes to the file /etc/postgresql/{vers.PG}/pgpg_hba.conf  yourself, then start PostgreSQL again.

    User Provisioning: Prompts for passwords to secure the postgres superuser and creates the dedicated bafadmin administrator role.

    Package Protection: Places all installed PostgreSQL packages on apt-mark hold to prevent inadvertent upgrades during routine apt upgrade operations.
______________________________________________________________________________________________________________________________________________________________________________________________________________________________

Українська


Цей Bash-скрипт автоматизує процес встановлення, налаштування продуктивності та безпеки PostgreSQL (версій 14–18+) зі спеціальними патчами та оптимізаціями для систем BAS / 1C.

Скрипт автоматично виявляє локально завантажені дистрибутиви у форматі .tar.bz2, налаштовує локалі та часовий пояс, застосовує рекомендовані параметри конфігурації (online_analyze, plantuner), створює адміністратора бази даних (bafadmin) та блокує автоматичне оновлення встановлених пакетів через apt.
Системні вимоги та підтримувані ОС

    Підтримувані операційні системи:

        Ubuntu: 22.04 LTS, 24.04 LTS (та сумісні)

        Debian: 11 (Bullseye), 12 (Bookworm), 13 (Trixie)

        Linux Mint: 21.x, 22.x (автоматично адаптується під базу Ubuntu)

    Підтримувана архітектура: Тільки amd64 (x86_64).

    Версії PostgreSQL: Офіційні збірки BAS / 1C (PG 14, 15, 16, 17, 18+) з довільними суфіксами релізів (наприклад, 14.20, 15.15-1.1C, 16.11-1.1C, 17.7-1.1C, 18.1-2.1C).

Важливий нюанс: Порядок завантаження пакетів

Скрипт не завантажує архіви PostgreSQL з мережі самостійно, оскільки офіційні пакети BAS / 1C потребують авторизованого доступу.
Перший запуск та створення директорії

При першому запуску скрипт перевіряє наявність робочої директорії:

~/install/installPostgre/

(Шлях обчислюється відносно домашньої папки користувача, який викликав sudo).

Якщо папка відсутня, скрипт створює її та завершує роботу з інструкцією:
Plaintext

============================================================
УВАГА: СТВОРЕНО ДИРЕКТОРІЮ ДЛЯ ВСТАНОВЛЕННЯ
============================================================
Папку для пакетів встановлення створено за шляхом:
  /home/username/install/installPostgre або ~/install/installPostgre/

Будь ласка, завантажте необхідний реліз PostgreSQL для BAS / 1C з:
  https://dl.bas-soft.eu
  (або https://releases.1c.eu)

Помістіть завантажений файл архіву .tar.bz2 у папку:
  /home/username/install/installPostgre/ або ~/install/installPostgre/

Після завантаження файла запустіть цей скрипт знову.
============================================================

Порядок дій:

    Завантажте відповідний .tar.bz2 архів для вашої ОС з порталу https://dl.bas-soft.eu або https://releases.1c.eu.

    Скопіюйте файл .tar.bz2 у директорію ~/install/installPostgre/.

    (За бажанням) Помістіть архів аддонів *addon*.tar.bz2 у ту ж папку, якщо плануєте їх використовувати.

    Повторно запустіть скрипт.

Інструкція з запуску та параметри CLI

Надайте скрипту права на виконання та запустіть від імені root:
Bash

chmod +x install_pg.sh
sudo ./install_pg.sh

Параметри командного рядка:

    --pg=<версія> — Примусовий вибір мажорної версії, якщо у папці кілька архівів (наприклад, --pg=16).

    --addon — Установити додаткові пакети аддонів з архіву *addon*.tar.bz2.

    --no-addon — Пропустити встановлення аддонів (значення за замовчуванням).

    --timezone=<зона> — Встановити часовий пояс без інтерактивного запиту (наприклад, --timezone=Europe/Kyiv).

    -h, --help — Показати довідку.

Покроковий алгоритм роботи скрипта

    Перевірка прав та блокувань: Перевіряє наявність привілеїв root та стану блокувань менеджерів пакетів apt/dpkg.

    Автовизначення дистрибутива: Сканує папку ~/install/installPostgre/.

        Якщо знайдено 1 пакет, він обирається автоматично.

        Якщо знайдено кілька пакетів, виводиться інтерактивне меню вибору.

    Резервне копіювання старого PostgreSQL: Якщо в системі виявлено встановлений PostgreSQL, скрипт запропонує зробити повний логічний та фізичний бекап (/var/backups/postgresql_backup_<timestamp>.tar.gz), включаючи pg_dumpall, перед видаленням старих пакетів.

    Конфігурація системних параметрів: Встановлює локалі (uk_UA.UTF-8, en_US.UTF-8, de_DE.UTF-8, ru_RU.UTF-8) та налаштовує часовий пояс.

    Оптимізація під BAS / 1C: Вносить оптимізації у postgresql.conf:

        shared_buffers = 2GB, work_mem = 64MB, max_connections = 500

        shared_preload_libraries = 'online_analyze,plantuner'

       !!! Переводить автентифікацію в pg_hba.conf на md5.
        !!! Якщо вам потрібно встановити md5 для 127.0.0.1 та змінити інші налаштування – зупиніть службу PostgreSQL та внесіть зміни до файлу /etc/postgresql/{vers.PG}/pgpg_hba.conf самостійно, а потім знову запустіть PostgreSQL.

    Налаштування користувачів: Запитує пароль для суперкористувача postgres та створює адміністратора bafadmin із правами SUPERUSER CREATEDB CREATEROLE.

    Захист від оновлень: Застосовує apt-mark hold до всіх встановлених пакетів PostgreSQL, запобігаючи їх випадковому оновленню при системному apt upgrade.
