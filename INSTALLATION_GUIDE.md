
<div align="center">
  <h1>Part 1 - Nagios Master Server Installation</h1>
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
  <h1>Part 2 - CLIENT SERVER SETUP</h1>
</div>

---

# 1. System Preparation (Update & Install Dependencies)
```bash
# sudo apt update
# sudo apt upgrade -y
# sudo apt install -y build-essential libgd-dev openssl libssl-dev xinetd
```
# 2. Create Nagios user and group
```bash
# sudo useradd nagios
# sudo groupadd nagios
# sudo usermod -aG nagios nagios
```

# 3. Install NRPE 4.1.3 and plugins
```bash
# cd /tmp
# wget https://github.com/NagiosEnterprises/nrpe/releases/download/nrpe-4.1.3/nrpe-4.1.3.tar.gz 
# tar -xzf nrpe-4.1.3.tar.gz
# cd nrpe-4.1.3
# ./configure --enable-command-args --with-nagios-user=nagios --with-nagios-group=nagios --with-ssl=/usr/bin/openssl --with-ssl-lib=/usr/lib/x86_64-linux-gnu
# sudo make
# sudo make all
# sudo make check_nrpe
# sudo make install-plugin
# sudo make install-daemon
# sudo make install-config
# sudo make install-init
# sudo systemctl enable --now nrpe.service
# sudo chown -R nagios:nagios /usr/local/nagios
```

# 4. Install Nagios Plugins
```bash
# cd /tmp
# wget https://nagios-plugins.org/download/nagios-plugins-2.4.6.tar.gz 
# tar -xzf nagios-plugins-2.4.6.tar.gz
# cd nagios-plugins-2.4.6
# ./configure --with-nagios-user=nagios --with-nagios-group=nagios
# make
# sudo make install
```
Verify plugin binaries
```bash
# ls -l /usr/local/nagios/libexec/check_load
# ls -l /usr/local/nagios/libexec/check_disk
# ls -l /usr/local/nagios/libexec/check_mem
# ls -l /usr/local/nagios/libexec/check_swap
```

# 5. Start NRPE service
```bash
# sudo systemctl daemon-reload
# sudo systemctl enable --now nrpe.service
# sudo systemctl status nrpe.service
```

# 6. Configure NRPE (nrpe.cfg) - /usr/local/nagios/etc/nrpe.cfg
```bash
# nano /usr/local/nagios/etc/nrpe.cfg
Add Below Details – 
allowed_hosts=127.0.0.1,<Nagios_Master_IP>
dont_blame_nrpe=1
ssl_enabled=1
```

# 7. Test Connectivity from Nagios Master (SSL)
```bash
# /usr/local/nagios/libexec/check_nrpe -H <Client IP>
```
<img width="1226" height="121" alt="image" src="https://github.com/user-attachments/assets/15155907-2b1f-4802-a2ee-978df6b38848" />

<sub><i>Expected output: NRPE v4.1.3</i></sub>

<div align="center">
  <h1>Part 3 - Service Check Setup</h1>
</div>

# On Nagios Master Server Setup
Create configuration files
```bash

# sudo touch /usr/local/nagios/etc/objects/hosts.cfg
# sudo touch /usr/local/nagios/etc/objects/services.cfg
# sudo chown nagios:nagios /usr/local/nagios/etc/objects/*.cfg
# sudo chmod 664 /usr/local/nagios/etc/objects/*.cfg

===========================================================================
For Master server checks –

Configure NRPE
File: /usr/local/nagios/etc/nrpe.cfg

# nano /usr/local/nagios/etc/nrpe.cfg

command[check_load]=/usr/local/nagios/libexec/check_load -r
command[check_load]=/usr/local/nagios/libexec/check_load -r -w .15,.10,.05 -c .30,.25,.20
command[check_disk_root]=/usr/local/nagios/libexec/check_disk -w 20% -c 10% -p /
command[check_disk_boot]=/usr/local/nagios/libexec/check_disk -w 20% -c 10% -p /boot
command[check_disk_tmp]=/usr/local/nagios/libexec/check_disk -w 20% -c 10% -p /tmp
command[check_disk_var]=/usr/local/nagios/libexec/check_disk -w 20% -c 10% -p /var
command[check_mem]=/usr/local/nagios/libexec/check_mem -w 80 -c 90
command[check_swap]=/usr/local/nagios/libexec/check_swap -w 20% -c 10%
command[check_chronyd]=/usr/local/nagios/libexec/check_procs -c 1: -a chronyd
command[check_nrpe]=/usr/local/nagios/libexec/check_procs -c 1: -a nrpe
command[check_rsyslog]=/usr/local/nagios/libexec/check_procs -c 1: -a rsyslogd
command[check_ssh]=/usr/local/nagios/libexec/check_procs -c 1: -a sshd

===========================================================================

# cat /usr/local/nagios/etc/objects/commands.cfg

define command {
    command_name    check_nrpe
    command_line    $USER1$/check_nrpe -H $HOSTADDRESS$ -c $ARG1$
}

===========================================================================

# cat /usr/local/nagios/etc/objects/localhost.cfg
##----- Disk Check ---------------

define service{
    use                 generic-service
    host_name           localhost
    service_description Disk /
    check_command       check_nrpe!check_disk_root
}

define service{
    use                 generic-service
    host_name           localhost
    service_description Disk /boot
    check_command       check_nrpe!check_disk_boot
}

define service{
    use                 generic-service
    host_name           localhost
    service_description Disk /tmp
    check_command       check_nrpe!check_disk_tmp
}

define service{
    use                 generic-service
    host_name           localhost
    service_description Disk /var
    check_command       check_nrpe!check_disk_var
}

===========================================================================

Edit nagios.cfg file and add

# nano /usr/local/nagios/etc/nagios.cfg
  cfg_file=/usr/local/nagios/etc/objects/hosts.cfg
  cfg_file=/usr/local/nagios/etc/objects/services.cfg

===========================================================================

Define hosts
File: /usr/local/nagios/etc/objects/hosts.cfg

# nano /usr/local/nagios/etc/objects/hosts.cfg

define host{
    use             linux-server
    host_name       <hostname>
    alias           Client Server 1
    address         <client host IP>
}

define host{
    use             linux-server
    host_name       <hostname>
    alias           Client Server 2
    address         <client host IP>
}

===========================================================================

Define NRPE command

File: /usr/local/nagios/etc/objects/commands.cfg
# nano /usr/local/nagios/etc/objects/commands.cfg
define command{
    command_name    check_nrpe
    command_line    $USER1$/check_nrpe -H $HOSTADDRESS$ -c $ARG1$
}

===========================================================================

Define service checks (Below is few examples of checks)

File: /usr/local/nagios/etc/objects/services.cfg

# nano /usr/local/nagios/etc/objects/services.cfg

##----- Generic Check ------

define service{
    use                 generic-service
    host_name           <hostname>
    service_description PING
    check_command       check_ping!100.0,20%!500.0,60%
}

##----- Resource Check --------

define service{
    use                 generic-service
    host_name           <hostname>
    service_description CPU Load
    check_command       check_nrpe!check_load
}

define service{
    use                 generic-service
    host_name           <hostname>
    service_description Memory Usage
    check_command       check_nrpe!check_mem
}

##----- Disk Check --------

define service{
    use                 generic-service
    host_name           <hostname>
    service_description Disk /
    check_command       check_nrpe!check_disk_root
}

define service{
    use                 generic-service
    host_name           <hostname>
    service_description Disk /boot
    check_command       check_nrpe!check_disk_boot
}

##----- system service Check --------

define service{
    use                 generic-service
    host_name           <hostname>
    service_description Service Chrony
    check_command       check_nrpe!check_chronyd
}

define service{
    use                 generic-service
    host_name           <hostname>
    service_description Service NRPE
    check_command       check_nrpe!check_nrpe
}

===========================================================================

Validate and restart

# sudo /usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg
# sudo systemctl restart nagios.service
```
<img width="940" height="180" alt="image" src="https://github.com/user-attachments/assets/c1302787-23ec-41c5-84bb-6babd8473ce5" />


