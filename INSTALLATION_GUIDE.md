This file should contain all commands.

# 1. System Preparation (Update & Install Dependencies)
```bash
# sudo apt update
# sudo apt upgrade -y 
# sudo apt install -y build-essential autoconf gcc make apache2 php libapache2-mod-php php-gd libgd-dev unzip wget libssl-dev mailutils
(mailutils – this is optional)
# sudo apt list --installed | less
```

# 2. Create Nagios user and group
```bash
# sudo useradd nagios
# sudo groupadd nagcmd
# sudo usermod -aG nagcmd nagios
# sudo usermod -aG nagcmd www-data
```

# 3. Install Nagios Core
```bash
# cd /tmp
# wget https://assets.nagios.com/downloads/nagioscore/releases/nagios-4.5.14.tar.gz 
# tar -xvf nagios-4.5.14.tar.gz
# cd nagios-4.5.14
# sudo ./configure --with-nagios-user=nagios --with-nagios-group=nagios --with-httpd-conf=/etc/apache2/sites-enabled
# chown -R nagios:nagios /tmp/nagios-4.5.14
# chmod -R 755 /tmp/nagios-4.5.14
# apt install -y libperl-dev libtest-simple-perl

<img width="940" height="371" alt="image" src="https://github.com/user-attachments/assets/c6d1f116-fdc0-4c91-9fe1-d4f4ff50b2dd" />

```
