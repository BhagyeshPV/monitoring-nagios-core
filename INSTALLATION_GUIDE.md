
<div align="center">
  <h1>Nagios Master Server Installation</h1>
  <p>This file should contain all commands.</p>
</div>

---

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
# Sudo make all
# sudo make install
# sudo make install-init
# sudo make install-commandmode
# sudo make install-config
# sudo make install-webconf
# sudo make test
```
<img width="600" height="375" alt="image" src="https://github.com/user-attachments/assets/c6d1f116-fdc0-4c91-9fe1-d4f4ff50b2dd" />
<img width="600" height="275" alt="image" src="https://github.com/user-attachments/assets/cd4f5947-1fcc-4304-a333-12d144038cb5" />

# 4. Configure Web Access
```bash
# sudo htpasswd -c /usr/local/nagios/etc/htpasswd.users nagiosadmin
# sudo a2enmod rewrite cgi
# sudo systemctl restart apache2
```
# 5. Install Nagios Plugins
```bash
# cd /tmp
# wget https://nagios-plugins.org/download/nagios-plugins-2.4.6.tar.gz 
# tar -xzf nagios-plugins-2.4.6.tar.gz
# cd nagios-plugins-2.4.6
# ./configure --with-nagios-user=nagios --with-nagios-group=nagios
# make
# sudo make install
```
# 6. Install NRPE Plugin on Master
```bash
# cd /tmp
# wget https://github.com/NagiosEnterprises/nrpe/releases/download/nrpe-4.1.3/nrpe-4.1.3.tar.gz 
# tar -xvf nrpe-4.1.3.tar.gz
# cd nrpe-4.1.3
# ./configure --enable-command-args --with-nagios-user=nagios --with-nagios-group=nagios --with-ssl=/usr/bin/openssl --with-ssl-lib=/usr/lib/x86_64-linux-gnu
# make
# sudo make all
# sudo make check_nrpe
# sudo make install-plugin

# sudo make install-daemon
# sudo make install-config
# sudo make install-init
# sudo systemctl enable --now nrpe.service
```
<img width="482" height="190" alt="image" src="https://github.com/user-attachments/assets/943edc78-628e-413f-8753-8aae2f86761f" />

# 7. Enable, Verify and Restart services
```bash
# sudo systemctl enable --now nagios.service
# sudo systemctl status nagios.service
# sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
# sudo systemctl restart nagios
# sudo systemctl status nagios
```

---

<div align="center">
  <h1>CLIENT SERVER SETUP</h1>
</div>

---



