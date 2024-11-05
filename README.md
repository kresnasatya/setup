# Setup

My setup on computer.

## macOS

### Homebrew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Zsh Config

- [.zshrc](https://raw.githubusercontent.com/kresnasatya/setup/refs/heads/main/.zshrc)

### Text Editor

- Visual Studio Code (VS Code).
- [VS Code settings](https://raw.githubusercontent.com/kresnasatya/setup/refs/heads/main/vscode/settings.json) and [keybindings](https://raw.githubusercontent.com/kresnasatya/setup/refs/heads/main/vscode/keybindings.json)
- Zed editor

### Additional Software

- Table Plus
- DBngin
- Postman
- Notion
- Figma
- [Herd by Laravel](https://herd.laravel.com)
- [Bezel](https://getbezel.app/)
- [Orbstack for Docker](https://orbstack.dev/)
- [App Cleaner](https://freemacsoft.net/appcleaner/)
- [The Unarchiver](https://theunarchiver.com/)
- Discord
- Cloudflare Warp
- Google Chrome
- Firefox
- [Zotero for Citation Paper (alternative of Mendeley)](https://www.zotero.org/) -> Optional
- Android Studio -> Optional
- Swift Playgrounds -> Optional
- Office 365 -> Optional
- [Screenflow](https://www.telestream.net/screenflow/overview.htm) -> Optional

## Windows

First of all, please install [Windows Terminal](https://apps.microsoft.com/detail/9n0dx20hk701) on Microsoft Store in order to make it easier to manage terminal between environment Windows and WSL itself.

### WSL2

This step below is a WSL2 fresh install of WSL2. Open Windows Shell and run as administrator.

```sh
wsl --install -d Ubuntu
```

After Ubuntu distro downloaded, you must restart your computer to make it works properly.

Next, open Windows Terminal then choose option for Ubuntu terminal (?). For the first time, the Ubuntu will set an update then there will be a prompt to set username and password.

> Usually, I set the password same with the username. 😅

Next, run `sudo apt update && sudo apt full-upgrade`

Additional information.

```sh
# To get list of distro
wsl -l -v
# Shutdown WSL
wsl --shutdown
# Unregister the distro
wsl --unregister Ubuntu
# Close the terminal and open again
```

**Reference**

[Install Ubuntu on WSL2 with GUI Support](https://ubuntu.com/tutorials/install-ubuntu-on-wsl2-on-windows-11-with-gui-support#1-overview)

#### ZSH and Oh My Zsh

```bash
sudo apt install wget git zsh
sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

#### Homebrew {#homebrew-wsl2}

```sh
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

After download, then add Homebrew to the PATH.

```sh
(echo; echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"') >> /home/usdidev/.zshrc
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

Next, install Homebrew's dependencies.

```bash
sudo apt-get install -y build-essential
```

Install PHP, Composer, Volta, and PNPM.

```sh
brew install volta php composer
```

Install NodeJS and PNPM with Volta.

```sh
volta install node pnpm
```

#### MySQL Server

Install MySQL Version 8 and Reset Password with mysql_native_password.
    
```sh
sudo apt install -y mysql-server
```

Start MySQL server.

```sh
sudo /etc/init.d/mysql start
```

Run the script security then choose No option!

```sh
sudo mysql_secure_installation
# Change root password with empty password since it's development mode
sudo mysql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '';
# Exit

# Change port and allow access remotely with bind_address option
sudo vi /etc/mysql/my.cnf
# Add config below
[mysqld]
bind_address = 0.0.0.0
port = 33061

# Restart mysql server
sudo service mysql restart

# Related issue
https://stackoverflow.com/questions/62987154/mysql-wont-start-error-su-warning-cannot-change-directory-to-nonexistent

# Create root with access remote (can access mysql with IP Address of my computer)
mysql -u root
CREATE USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '';
# Then grant all access to that user
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' WITH GRANT OPTION;
```

**Reference**

[How to install MySQL on WSL 2 (Ubuntu)](https://pen-y-fan.github.io/2021/08/08/How-to-install-MySQL-on-WSL-2-Ubuntu/)

#### Redis

```bash
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list

sudo apt-get update
sudo apt-get install redis
```

To run Redis server and client use commands below.

```bash
# First tab
sudo service redis-server start
# Second tab (make sure Redis server has started)
redis-cli
# Test connection inside redis-cli with "ping" command
```

I want to make Redis can be accessed with my computer IP Address.

```bash
# First, stop the Redis server
# Next, go to redis.conf file
sudo vi /etc/redis/redis.conf
# Change value of bind directive
bind 0.0.0.0
# Change protected-mode from yes to no
protected-mode no
# Save it and restart Redis Server
sudo service redis-server restart
```

Install PHP Redis Extension with PECL command then just HIT ENTER.

```bash
pecl install redis
# Then check if redis module or extension has been installed in PHP
php -m
```

**Reference**

[Install Redis on Windows](https://redis.io/docs/latest/operate/oss_and_stack/install/install-redis/install-redis-on-windows/)