# TP Rendu

**ssh root@wilfart.fr -p 6209**

## Nouveau User

sudo apt update

sudo apt install nginx

sudo adduser noah

sudo passwd noah

## Permission SSH

sudo nano /etc/ssh/sshd_config
    
    PermitRootLogin no

sudo systemctl restart sshd

sudo usermod -aG sudo noah

nano /etc/ssh/sshd_config

    #Authentication: 
    
    PermitRootLogin no
    AllowUsers noah

sudo systemctl restart sshd

## FireWall

sudo apt install ufw -y

sudo ufw enable

sudo ufw status

>*on liste les connexion active pour activé sur le firewall uniquement ce dont on a besoin*

ss -tuln

    udp   UNCONN 0      0            0.0.0.0:68        0.0.0.0:*
    tcp   LISTEN 0      511          0.0.0.0:80        0.0.0.0:*
    tcp   LISTEN 0      128          0.0.0.0:22        0.0.0.0:*
    tcp   LISTEN 0      511             [::]:80           [::]:*
    tcp   LISTEN 0      128             [::]:22           [::]:*


sudo ufw allow ssh

sudo ufw allow http

sudo systemctl start ufw

sudo ufw enable

sudo systemctl status ufw

    Loaded: loaded (/lib/systemd/system/ufw.service; enabled; preset: >
     Active: active (exited) since Mon 2024-12-16 11:27:43 CET; 29s ago

sudo ufw status

    Status: active

    To                         Action      From
    --                         ------      ----
    80/tcp                     ALLOW       Anywhere
    22/tcp                     ALLOW       Anywhere
    80/tcp (v6)                ALLOW       Anywhere (v6)
    22/tcp (v6)                ALLOW       Anywhere (v6)

## Backup

cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup-original

nginx -s reload

## Backup automatique avec cron 

cd /var/log/

touch nginx_backup.log

nano ~/backup_nginx.sh

    #!/bin/bash

    cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.backup-original

    echo "Sauvegarde nginx.conf effectuée le $(date)" >> /var/log/nginx_backup.log

chmod +x ~/backup_nginx.sh

crontab -e

    0 2 * * 1 /bin/bash /home/user/backup_nginx.sh

crontab -l

## fail2ban

nano /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
    
    [sshd]
    enabled = true
    port = ssh
    logpath = %(sshd_log)s
    backend = %(ssh_backend)s

    [nginx-http-auth]
    enabled = true
    port = http, https
    logpath = /var/log/nginx/error.log