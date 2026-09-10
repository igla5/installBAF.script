# BAS Server Installer — Public Release

## 🇬🇧 English

### Overview

`install-baf` is a Bash installer and updater for BAS Server on supported Debian/Ubuntu-based Linux systems.

The public release is designed so that the BAS Server installation package is **provided by the user locally**. The installer does **not download BAS Server packages from CloudForNet or any third-party server**.

The user must download the required BAS Server package directly from the official BAS download site:

https://dl.bas-soft.eu/

The installer then uses the locally provided package to perform the installation or upgrade.

### Supported systems

The installer supports:

- Ubuntu 22.04
- Ubuntu 24.04
- Ubuntu 26.04
- Linux Mint 21.x
- Linux Mint 22.x
- Debian 11
- Debian 12
- Debian 13 (Trixie)

Architecture:

- `amd64` / `x86_64`

Other operating systems or architectures are rejected by the installer.

---

## 1. Download the BAS package

Create the installation directory as your normal user:

```bash
mkdir -p ~/installBAF
```

Alternatively, you can use:

```text
/install/installBAF/
```

Download the required BAS Server package personally from the official BAS website:

https://dl.bas-soft.eu/

For example:

```text
setup-full-8.3.23.2301-x86_64.tar
```

Place the downloaded file into:

```text
~/installBAF/
```

or:

```text
/install/installBAF/
```

The installer can also use supported BAS archives placed next to the installer.

### Important

The public installer does **not** download the BAS Server package automatically.

Do not place credentials or private download links into the public installer.

---

## 2. Run the installer

Make the script executable:

```bash
chmod +x install-baf
```

Run it with root privileges:

```bash
sudo ./install-baf
```

The installer will:

1. Check the operating system and architecture.
2. Check/create the required system user and directories.
3. Install required system packages.
4. Configure locales and timezone.
5. Install required fonts and dependencies.
6. Install/configure Webmin.
7. Locate the locally supplied BAS package.
8. Extract and validate the BAS installer.
9. Detect the BAS version.
10. Create a backup of the previous BAS installation when applicable.
11. Install or update BAS Server.
12. Configure BAS and RAS services.
13. Start and verify the services.
14. Clean temporary files.
15. Display the final installation status.

---

## 3. Command-line options

### `--timezone=ZONE`

Set the system timezone.

Example:

```bash
sudo ./install-baf --timezone=Europe/Berlin
```

If not specified, the script uses its configured default timezone.

### `--keep-archive`

Keep an archive downloaded by the installer.

In the **public release**, BAS packages are supplied locally by the user, so user-provided BAS archives are never deleted by the installer.

The option is retained for compatibility with the installer logic.

### `--force-setup`

Force the setup/configuration stage even when BAS is already installed.

```bash
sudo ./install-baf --force-setup
```

### `--downgrade`

Allow installation of a BAS version older than the currently installed version.

```bash
sudo ./install-baf-pub --downgrade
```

The installer will still require confirmation before performing a downgrade when applicable.

`--allow-downgrade` is also supported.

### Help

```bash
./install-baf --help
```

---

## 4. BAS package handling

The installer searches for a locally supplied BAS package.

Supported package/archive types include:

```text
.run
.tar
.tar.gz
.tgz
.tar.bz2
.tbz2
.tar.xz
.zip
.rar
```

The BAS setup file is expected to have a name similar to:

```text
setup-full-8.3.23.2301-x86_64.tar
```

The installer extracts nested archives when required and searches for the BAS `.run` installer.

If no valid local BAS package is found, the installation stops and instructs the user to download the package from the official BAS download site.

---

## 5. Barcode font

The installer attempts to download the optional barcode font:

```text
EanBwrP36Tt.ttf
```

If the font can be downloaded successfully, it is installed and the font cache is updated.

If the download fails, **the BAS installation continues**.

A warning is displayed in the final installation summary:

```text
WARNING: Barcode font was NOT installed.
If barcode printing is required, install this font manually.
```

Therefore, failure to download the barcode font does not mean that the BAS Server installation failed.

If barcode printing is required and the font was not installed automatically, the user must install the font manually.

---

## 6. Backups

Before replacing an existing BAS installation, the installer creates a backup of the previous BAS version according to its normal backup logic.

Backups are stored under:

```text
/opt/baf-archives
```

Installed BAS packages are stored under:

```text
/opt/baf-packages
```

The installation working directory is:

```text
/opt/install-baf
```

The installer log is:

```text
/var/log/baf_install.log
```

---

## 7. Services

The installer configures the BAS Server and RAS systemd services.

The exact service names depend on the installed BAS version.

After installation, the installer starts and checks the services and reports their status.

---

## 8. Webmin

The installer installs and configures Webmin as part of the normal system setup.

After successful installation, the Webmin address is displayed in the final summary.

---

## 9. Security and distribution model

This is a public release.

The public release is intentionally designed so that:

- BAS packages are supplied by the end user.
- BAS packages are downloaded from the official BAS distribution source.
- The installer does not contain a CloudForNet BAS package download URL.
- The installer does not require CloudForNet credentials.
- The public release does not redistribute the BAS Server package itself.

Users are responsible for obtaining the appropriate BAS Server package and complying with the BAS software license and applicable terms of use.

---

## 10. Requirements

Before running the installer:

- Root/sudo access is required.
- The system must be a supported Linux distribution.
- The system must use `amd64` architecture.
- Internet access is recommended/required for system package installation and components that are downloaded during setup.
- A valid BAS Server package must be supplied locally.

---

## 11. Troubleshooting

Check the installer log:

```bash
sudo less /var/log/baf_install.log
```

Check BAS-related services:

```bash
systemctl --type=service | grep -E 'srvbaf|ras'
```

Check the installed BAS directory:

```bash
ls -la /opt/BAF/x86_64
```

If the installer reports that no BAS package was found, verify that the package is located in:

```text
~/installBAF/
```

or:

```text
/install/installBAF/
```

and that it is a supported BAS package/archive.

---

# BAS Server Installer — Public Release

## 🇺🇦 Українська версія

### Опис

`install-baf` — це Bash-інсталятор та оновлювач BAS Server для підтримуваних Linux-систем на базі Debian/Ubuntu.

Публічна версія розроблена таким чином, щоб пакет BAS Server **надавав користувач локально**. Інсталятор **не завантажує пакети BAS Server із CloudForNet або будь-якого стороннього сервера**.

Користувач повинен самостійно завантажити необхідний пакет BAS Server з офіційного сайту BAS:

https://dl.bas-soft.eu/

Після цього пакет використовується інсталятором локально для встановлення або оновлення BAS Server.

### Підтримувані системи

Інсталятор підтримує:

- Ubuntu 22.04
- Ubuntu 24.04
- Ubuntu 26.04
- Linux Mint 21.x
- Linux Mint 22.x
- Debian 11
- Debian 12
- Debian 13 (Trixie)

Архітектура:

- `amd64` / `x86_64`

Інші операційні системи або архітектури інсталятор відхиляє.

---

## 1. Завантаження пакета BAS

Створіть директорію встановлення від свого користувача:

```bash
mkdir -p ~/installBAF
```

Також можна використовувати:

```text
/install/installBAF/
```

Необхідний пакет BAS Server потрібно самостійно завантажити з офіційного сайту BAS:

https://dl.bas-soft.eu/

Наприклад:

```text
setup-full-8.3.23.2301-x86_64.tar
```

Помістіть завантажений файл у:

```text
~/installBAF/
```

або:

```text
/install/installBAF/
```

Інсталятор також може використовувати підтримуваний пакет BAS, розташований поруч із самим інсталятором.

### Важливо

Публічна версія інсталятора **не завантажує пакет BAS Server автоматично**.

Не додавайте облікові дані або приватні посилання на завантаження до публічного інсталятора.

---

## 2. Запуск інсталятора

Зробіть файл виконуваним:

```bash
chmod +x install-baf
```

Запустіть його з правами root:

```bash
sudo ./install-baf
```

Інсталятор:

1. Перевіряє операційну систему та архітектуру.
2. Перевіряє/створює необхідного системного користувача та директорії.
3. Встановлює необхідні системні пакети.
4. Налаштовує локалі та часовий пояс.
5. Встановлює необхідні шрифти та залежності.
6. Встановлює/налаштовує Webmin.
7. Знаходить локально наданий пакет BAS.
8. Розпаковує та перевіряє інсталятор BAS.
9. Визначає версію BAS.
10. За необхідності створює резервну копію попередньої версії BAS.
11. Встановлює або оновлює BAS Server.
12. Налаштовує сервіси BAS та RAS.
13. Запускає та перевіряє сервіси.
14. Видаляє тимчасові файли.
15. Виводить підсумковий статус встановлення.

---

## 3. Параметри командного рядка

### `--timezone=ZONE`

Встановлення часового поясу системи.

Наприклад:

```bash
sudo ./install-baf --timezone=Europe/Berlin
```

Якщо параметр не вказаний, використовується часовий пояс, заданий у конфігурації скрипта.

### `--keep-archive`

Зберігати архів, завантажений інсталятором.

У **публічній версії** пакети BAS надаються користувачем локально, тому локальні архіви BAS, надані користувачем, інсталятор не видаляє.

Параметр збережений для сумісності з існуючою логікою інсталятора.

### `--force-setup`

Примусово виконати етап початкового налаштування навіть якщо BAS уже встановлений.

```bash
sudo ./install-baf --force-setup
```

### `--downgrade`

Дозволити встановлення версії BAS, яка старіша за поточну встановлену версію.

```bash
sudo ./install-baf --downgrade
```

У відповідній ситуації інсталятор додатково попросить підтвердження перед downgrade.

Також підтримується:

```bash
--allow-downgrade
```

### Довідка

```bash
./install-baf --help
```

---

## 4. Робота з пакетом BAS

Інсталятор шукає пакет BAS локально.

Підтримуються такі типи файлів:

```text
.run
.tar
.tar.gz
.tgz
.tar.bz2
.tbz2
.tar.xz
.zip
.rar
```

Файл BAS зазвичай має назву на зразок:

```text
setup-full-8.3.23.2301-x86_64.tar
```

Інсталятор за необхідності розпаковує вкладені архіви та знаходить інсталятор BAS `.run`.

Якщо відповідний локальний пакет BAS не знайдений, встановлення припиняється та користувачу буде запропоновано завантажити пакет з офіційного сайту BAS.

---

## 5. Шрифт для штрихкодів

Інсталятор намагається автоматично завантажити додатковий шрифт для штрихкодів:

```text
EanBwrP36Tt.ttf
```

Якщо шрифт успішно завантажений, він встановлюється та оновлюється кеш шрифтів.

Якщо завантаження шрифту не вдалося, **встановлення BAS продовжується**.

У фінальному повідомленні буде показано попередження:

```text
WARNING: Barcode font was NOT installed.
If barcode printing is required, install this font manually.
```

Таким чином, помилка завантаження шрифту **не означає помилку встановлення BAS Server**.

Якщо потрібен друк штрихкодів і шрифт не був встановлений автоматично, користувач повинен встановити його самостійно.

---

## 6. Резервні копії

Перед заміною існуючої версії BAS інсталятор створює резервну копію попередньої версії відповідно до своєї стандартної логіки backup.

Резервні копії зберігаються:

```text
/opt/baf-archives
```

Пакети встановлених версій BAS:

```text
/opt/baf-packages
```

Робоча директорія:

```text
/opt/install-baf
```

Лог інсталятора:

```text
/var/log/baf_install.log
```

---

## 7. Сервіси

Інсталятор налаштовує systemd-сервіси BAS Server та RAS.

Точні назви сервісів залежать від встановленої версії BAS.

Після встановлення інсталятор запускає сервіси та перевіряє їхній статус.

---

## 8. Webmin

Webmin встановлюється та налаштовується в рамках стандартного процесу налаштування системи.

Після успішного встановлення адреса Webmin буде показана у фінальному повідомленні.

---

## 9. Безпека та модель розповсюдження

Це публічна версія інсталятора.

Публічна версія спеціально розроблена таким чином:

- пакет BAS надається кінцевим користувачем;
- пакет BAS завантажується з офіційного джерела BAS;
- інсталятор не містить URL CloudForNet для завантаження пакета BAS;
- інсталятор не потребує облікових даних CloudForNet;
- публічна версія не розповсюджує сам пакет BAS Server.

Користувач самостійно відповідає за отримання відповідного пакета BAS Server та дотримання ліцензії BAS і чинних умов використання програмного забезпечення.

---

## 10. Вимоги

Перед запуском інсталятора необхідно:

- мати права `root` / `sudo`;
- використовувати підтримуваний дистрибутив Linux;
- використовувати архітектуру `amd64`;
- мати доступ до Інтернету для встановлення системних пакетів та компонентів, які завантажуються під час налаштування;
- мати відповідний пакет BAS Server, розміщений локально.

---

## 11. Усунення несправностей

Переглянути лог інсталятора:

```bash
sudo less /var/log/baf_install.log
```

Перевірити BAS-сервіси:

```bash
systemctl --type=service | grep -E 'srvbaf|ras'
```

Перевірити встановлену BAS:

```bash
ls -la /opt/BAF/x86_64
```

Якщо інсталятор повідомляє, що пакет BAS не знайдений, перевірте, що файл знаходиться в:

```text
~/installBAF/
```

або:

```text
/install/installBAF/
```

та є підтримуваним пакетом/архівом BAS.

---

## License and BAS software

This installer is a third-party installation utility and is not a distribution of BAS Server itself.

BAS Server packages are not included with this public release. Users must obtain BAS Server packages from the official BAS source and comply with the applicable BAS license and terms.
