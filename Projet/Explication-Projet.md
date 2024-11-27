# Projet : Serveur Minecraft 

 **Groupe : Tao NGO Noah GIREAU Gabriel QUEAU**

## Explication du projet :

<br />

**1. Surveillance et gestion des logs** 
- Configurer logrotate pour gérer les fichiers de logs.
- Détecter des anomalies dans les logs (joueurs bannis, crashs) et envoyer des alertes par email.
- Analyser et compresser les logs historiques

<br />

**2. Sauvegardes et sécurité**
- Automatiser les sauvegardes avec des scripts bash et les planifier via cron.
- Configurer un accès SFTP sécurisé pour récupérer manuellement les sauvegardes.
- Analyser les fichiers de sauvegarde pour détecter les tailles inhabituelles.
- Configurer un espace de stockage dédié pour les sauvegardes avec LVM

<br />

**3. Gestion des utilisateurs**
- Créer un leaderboard dynamique affichable sur un site web ou dans le jeu.
- Ajouter un système d'événements automatiques (ex : spawn aléatoire de mobs à des heures définies).
- Écrire un script pour extraire les données sur les blocs les plus minés ou les monstres tués par joueur.
- Créer des rapports d'activité des joueurs avec des scripts (ex : temps passé, zones explorées).
- Configurer un système de whitelist dynamique basé sur une base de données ou un fichier JSON.

<br />

**4. Sécurité et réseau**
- Configurer un pare-feu (ufw ou iptables) pour limiter l'accès au port du serveur.
- Créer un script d'alerte pour surveiller les connexions réseau inhabituelles ou le dépassement de seuils CPU/RAM.

<br />

**5. Optimisation Serveur**
- Créer un script de démarrage et d'arrêt du serveur avec vérification d'état préalable.
- Mettre en place des scripts pour redémarrer automatiquement le serveur en cas de crash.
