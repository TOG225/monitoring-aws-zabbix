# AWS Hybrid Infrastructure Monitoring with Zabbix

Ce projet présente la mise en place d'une solution de supervision centralisée pour un parc informatique hybride hébergé sur AWS. L'architecture permet de surveiller en temps réel des instances Linux (Ubuntu) et Windows Server via un serveur Zabbix conteneurisé.

🚀 Architecture du Projet
Serveur de Monitoring : Zabbix Server déployé via Docker sur une instance EC2 Ubuntu.

Hôtes Supervisés :

Instance Linux Ubuntu (Agent Zabbix).

Instance Windows Server (Agent Zabbix).

Réseau : Configuration des Security Groups AWS pour autoriser le trafic sur le port 10050.

🛠️ Défis Techniques & Solutions
1. Instabilité de l'adressage Docker
Après un redémarrage de l'instance AWS, l'IP interne du conteneur Zabbix est passée de 172.18.0.3 à 172.18.0.4.

Solution : Fixation de la communication sur l'interface de boucle locale (127.0.0.1) et autorisation du segment réseau Docker complet (172.18.0.0/16) dans la configuration de l'agent.

2. Connectivité Windows Server
L'agent Windows refusait initialement les connexions du serveur avec l'erreur Connection reset by peer.

Solution : Ajustement de la directive Server dans le fichier zabbix_agentd.conf pour inclure l'IP privée du serveur Zabbix et configuration du Pare-feu Windows.

📊 Visualisation (Dashboards)
Le projet inclut un tableau de bord global (Global Infrastructure Monitoring) regroupant :

Graphiques de charge CPU : Comparaison en temps réel des performances entre les deux systèmes d'exploitation.

Suivi de la RAM : Monitoring de l'utilisation mémoire pour anticiper les saturations sur les instances de type "micro".

Alertes Proactives : Configuration de triggers personnalisés (ex: CPU > 10%) avec sévérité "High" pour une réactivité immédiate.

📝 Configuration de l'Agent (Exemple)
Pour reproduire la connectivité stable établie dans ce projet, la directive Server doit être configurée comme suit :

Bash
Server=127.0.0.1,10.0.4.89,172.18.0.0/16
Auteur : [Oklin Ghislain TOURE] Environnement : AWS EC2, Docker, Zabbix.
