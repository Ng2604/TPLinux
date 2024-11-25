# Cours 4 TP

## TP Avancé : "Mission Ultime : Sauvegarde et Sécurisation"

### Étape 1 : Analyse et nettoyage du serveur

**1. Lister les tâches cron pour détecter des backdoors :**

    for user in $(cut -f1 -d: /etc/passwd); do echo "Crontab : $user:"; crontab -u $user -l 2>/dev/null || echo "Aucune tâche"; done

**2. Identifier et supprimer les fichiers cachés :**

    ls -a /tmp; ls -a /var/tmp; ls -a /home

    rm /tmp/malicious.sh ; rm /var/tmp/.nop ; rm -r /home/attacker


**3. Analyser les connexions réseau actives :**

    ss

### Étape 2 : Configuration avancée de LVM

**1. Créer un snapshot de sécurité pour /mnt/secure_data :**

**2. Tester la restauration du snapshot :**

    rm sensitive2.txt

    sudo umount /mnt/secure_data/

    lvconvert --merge /dev/vg_secure/snap_data

    mount /dev/vg_secure/secure_data /mnt/secure_data/

**3. Optimiser l’espace disque :**

    lvextend -l +1 /dev/mapper/vg_secure-secure_data

### Étape 3 : Automatisation avec un script de sauvegarde

**1. Créer un script secure_backup.sh :**

    vim secure_backup.sh 
    #! /bin/bash

    now=$(date +"%Y-%m-%d")

    cp -r /mnt/secure_data /backup
    mv /backup/secure_data /backup/secure_$now.tar.gz
    rm /backup/secure_$now.tar.gz/*.{log,tmp}

**2. Ajoutez une fonction de rotation des sauvegardes :**

Je rajoute a mon script :

    tar -czf /backup/secure_data_$now -C /backup secure_data_run
    rm -rf /backup/secure_data_run

    list=$(ls -1t /backup/)
    total_sauv=$(echo "$list" | wc -l)

    if [ $total_sauv -gt 6 ]; then
        backup_a_del=$(echo "$list" | tail -n +7)
        echo "$backup_a_del"

        rm -r /backup/$backup_a_del
        echo "backup supprimer"
    else
        echo "moins de 7 sauvegardes"

**4. Automatisez avec une tâche cron :**

    0 3 * * * /bin/bash /root/secure_backup.sh

### Étape 4 : Surveillance avancée avec auditd

**1. Configurer auditd pour surveiller /etc :**

    auditctl -w /etc -p wa -k etc_changes

### Étape 5 : Sécurisation avec Firewalld

**1. Configurer un pare-feu pour SSH et HTTP/HTTPS uniquement :**

    sudo firewall-cmd --zone=public --add-service=ssh --permanent
    
    sudo firewall-cmd --zone=public --add-service=http --permanent
    
    sudo firewall-cmd --zone=public --add-service=https --permanent
    
    sudo firewall-cmd --zone=public --set-target=DROP --permanent
    
    sudo firewall-cmd --zone=public --remove-service=cockpit --permanent
    
    sudo firewall-cmd --zone=public --remove-service=dhcpv6-client --permanent
    
    sudo firewall-cmd --reload

**2. Bloquer des IP suspectes :**

    firewall-cmd --add-rich-rule="rule family=ipv4 source address=A.B.C.D reject" --permanent

**3. Restreindre SSH à un sous-réseau spécifique :**

    sudo firewall-cmd --zone=public --remove-service=ssh --permanent

    sudo firewall-cmd --permanent --zone=public --add-source=192.168.56.0/24

    sudo firewall-cmd --reload